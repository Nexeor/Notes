## **Buffer Cache** 
**DATA STRUCTURES**
The buffer cache is a static array filled with a new buffer_cache_entry struct. A buffer_cache_entry handles the metadata and synchronization primitives for that slot of the buffer_cache. 

The full description of a buffer_cache_entry struct can be seen here:
```
#define BUFFER_CACHE_SIZE 64

struct buffer_cache_entry
{
    block_sector_t block_sector_id; // the block sector this entry is reserved  for. NULL if free
    bool dirty;                     // true if data has been modified and hasn't been written to disk
    bool valid;                     // true if the data from block_sector_id has been loaded
    uint8_t num_readers;            // How many threads are reading (>=0)
    uint8_t num_writers;            // How many threads are writing (0 or 1)
    uint8_t num_read_requests;      // How many reads are incoming
    uint8_t num_write_requests;     // How many writes are incoming
    struct lock block_lock;         // Protects the above metadata
    
    struct condition read_condition;  // Condition variable for reading entry
    struct condition write_condition; // Condition variable for writing entry
    uint8_t dataBlock[BLOCK_SECTOR_SIZE]; // The data stored in this block
    
    struct list_elem lru_elem; // List elem for LRU replacement
};
```

**ALGORITHMS**
The buffer_cache uses the LRU algorithm as its replacement policy. Besides the buffer_cache itself, a separate list is kept to track which blocks are in use or not. When a block is requested using cache_get_block, it is pushed to the front of the LRU list, ensuring that the most recently requested blocks will be at the top of the LRU list. When the buffer cache is full, and a block must be replaced, the final entry in the LRU list is popped off, written back to disk, and then given back to cache_get_block. Additionally, a cache entry with pending read_requests or write_requests are treated as "pinned" and is not available for eviction.

Unfortunately we were not able to get our implementation of write-behind and read-ahead working with our full implementation. The branch "P4-Only-Cache" does demonstrate how we intended to implement these features. When the buffer cache is created with initialize_buffer_cache, two child threads are spawned, a write_back_thread and a read_back_thread. The write_back_thread then goes into the write_back function. Inside this function timer_sleep (from project one) is called to force the thread to wait thirty seconds. After waiting, write_back calls the flush_buffer_cache function, which writes every buffer_cache_entry back to disk and clears the dirty/valid bits.  

Read-Ahead is similar, with the child thread going into it's own read_ahead function. However, it requires its own structures, detailed here:

```
struct read_ahead_request {
    block_sector_t sector; // The sector requested 
    struct list_elem elem;
};
struct list read_ahead_list; // Read ahead requests are queued here
struct condition read_ahead_cond; // Signal the read_ahead thread
struct lock read_ahead_lock; // Synchnous control for read_ahead_list
```

It waits in the read_ahead function until a read_ahead_request struct is created and added to the read_ahead_list. This occurs in cache_read_block. When a block is read in for the first time, a read_ahead_request for the next sector is created and added to the read_ahead_list before signaling the read_ahead thread using read_ahead_cond. When signaled, the read_ahead thread works in the background to load the next sector into the buffer cache.  

**SYNCHRONIZATION**
Synchronization for the buffer_cache is handled within the buffer_cache_entry struct. The "block_lock" field controls access to all the metadata corresponding to the block. However, this lock is not needed to access the dataBlock. This allows us to have fine grain control over our buffer_cache_entries. Multiple threads can be working on the buffer_cache or accessing an entries dataBlock, but only a single thread can edit a block's metadata at a time. 

## **Extensible Files**
**DATA STRUCTURES**
To implement indexed files and subdirectories we've modified the inode_disk struct with some additional fields: 

```
#define NUM_DIRECT_BLOCKS 123
#define NUM_INDIRECT_BLOCKS 128

/* On-disk inode.
   Must be exactly BLOCK_SECTOR_SIZE (512 bytes) bytes long. */
struct inode_disk
  {
    off_t length;                       /* File size in bytes. */ 
    unsigned magic;                     /* Magic number. */
    bool isDirectory;

    block_sector_t direct_blocks[NUM_DIRECT_BLOCKS];  // Direct access to blocks 
    block_sector_t indirect_blocks; // Pointer to indirect_blocks
    block_sector_t doubly_indirect_blocks; // Pointer to doubly_indirect_blocks
  };

/* In-memory inode. */
struct inode 
  {
    struct list_elem elem;              /* Element in inode list. */
    block_sector_t sector;              /* Sector number of disk location. */
    int open_cnt;                       /* Number of openers. */
    bool removed;                       /* True if deleted, false otherwise. */
    int deny_write_cnt;                 /* 0: writes ok, >0: deny writes. */
  };
```

With this structure, we can support large files: 
```
Direct Blocks: 123 sectors * 512 bytes = 62,976 bytes
Indirect Blocks: 128 sectors * 512 bytes = 65,536 bytes
Doubly Indirect Blocks: 128 indirect blocks * 65536 bytes = 8,388,608 bytes
Total Size: 8,388,608 + 62,976 bytes = 8,451,584 bytes
```

The three block_sector_t structs enable our filesystem. The direct_blocks array stores pointers to the data itself. indirect_blocks and doubly_indirect_blocks implement the tiered system. If the filesize exceeds the direct blocks, indirect_blocks points to another block containing additional blocks, and double_indirect_blocks contain pointers to additional indirect_blocks. The byte_to_sector function is used to allocate and access these blocks. When given an inode, byte_to_sector returns the block_sector_t corresponding to the given offset within inode and initializes additonal blocks as needed.

## **Subdirectories**
Subdirectories are implemented using the following structs: 

```
/* A directory. */
struct dir 
  {
    struct inode *inode;                /* Backing store. */
    off_t pos;                          /* Current position. */
  };

/* A single directory entry. */
struct dir_entry 
  {
    block_sector_t inode_sector;        /* Sector number of header. */
    char name[NAME_MAX + 1];            /* Null terminated file name. */
    bool in_use;                        /* In use or free? */
  };
```

Each in-memory directory has a backing inode and an offset within that inode. Directories have several entries, each with their inode_sector and an in_use flag indicating whether they are free or not. 

**Algorithms**
We traverse user-specified paths using two functions, split_path_into_filename_and_directories combined with open_full_path. All methods that traverse the filesystem along a user-specified path utilize these two functions. split_path_into_filename_and_directories parses the given path, identifying directories vs files. This parsed path is then given to open_full_path, which uses a series of directory and file lookups to move along the path. When we reach the last parsed token, we return the directory/file specified. 

This method is used for filesys_remove and filesys_open. However, in filesys_remove the removal of the current working directory is disallowed. We made this decision to cut down on potential synchronization problems that could occur with multiple threads are working on the CWD.






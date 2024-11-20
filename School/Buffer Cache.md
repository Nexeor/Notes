Data Structures:
- free_map: A bitmap that tracks which blocks on disk are in use or not
	- Already implemented for us
	- Hard limit of 8mb 
- inode: Uses metadata to track files in the filesys
- directory: Translates between file names and inodes


filesys: Manages the overall file system
- 

Whenever we read or write to a block, first check the cache
- If in the cache, used the cached block
- Otherwise, fetch the block from disk
- Cache has hard limit of 64 sectors (aka 64 inodes)

Cache: Linked list of 6

Approximate clock algorithm, evict the oldest unreferenced block when new data must be cached
- Blocks containing metadata may need to be held longer than other blocks

Questions:
- From methods do we read/write from blocks? They need to be modified to use the buffer cache
- An iNode can represent a file that is larger than a single sector. 
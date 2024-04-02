We abstract our files as so:
- Segmented in terms of bytes
- Refer to data by its file name
- Has access protection and consistency guarentee

We abstract our disk as so:
- Segmented in terms of bytes
- Refer to data by its block number
- No access protection and no guarantee beyond block write

![[Pasted image 20240402132418.png]]
Device Driver: Interacts directly with the disk and manages free space
Buffer Cache: Recently used blocks are moved here to be accessed faster
- Needs to be implemented
File System: Translates the disk blocks to files and writes changes back to the disk 
- directory.c: translates between 
File Operations: Uses files as a sequence of bytes 

![[Pasted image 20240402135240.png]]
Structures to track open files:
- Open File Table: Manages information about files that are currently open and in use by processes
	- In Pintos this is represented by "directory.c" (???)
	- Is shared between all processes 
	- Maintains an entry for each opened file
- file: Represents one or more processes using a file
	- Separate offsets for byte stream
- dir: Represents an open directory file 
	- directories are treated as a different type of file
- File Descriptors (inode): Data structures used by the file system to represent files and directories
	- SLIGHTLY DIFFERENT from traditional fd's used by processes. A fd that represents a file (not stdin/stdout/stderr) is "backed up" by an inode. The fd is how the process interacts with the file, but the inode is how the filesys actually interacts with the file. (do we need to somehow associate a processes fd list with the files?)
	1) in-memory: Info about an open file, such as the number of openers
		1) Corresponds to an on-disk file descriptor
	2) on-disk: A region on disk that stores persistent information about a file
		1) Who owns it, where is its data, etc. 
		2) Has an entry in the FD table
	3) on-disk cached: When an inode is cached, it is stored as a bytewise copy of the on-disk inode

Opening & Reading a File:
1) Use the directory to find the on-disk inode associated with the file
	- Pintos uses directory.c to handle this
2) Search the "open file table" for the entry
	1) If no entry found, create one
	2) If entry found, increase ref count
3) Use the on-disk inode to locate the file data
4) Read the data

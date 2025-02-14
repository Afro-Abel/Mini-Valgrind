# dmalloc - Dynamic Memory Allocation Tracker

`dmalloc` is a custom memory management library that tracks dynamic memory allocation and deallocation events, providing insights into memory usage, potential memory leaks, and heavy-hitting allocation locations. It aims to assist in debugging memory management issues by using metadata and canary values to detect errors such as double frees, wild writes, and invalid memory access.

## Features

- **Memory Allocation Tracking:** Tracks all dynamically allocated memory with metadata, including size, allocation location (file and line), and allocation timestamp.
- **Canary Values:** Protects allocated memory regions with canaries to detect out-of-bounds writes and overflows.
- **Memory Leak Detection:** Provides a leak report detailing all active allocations that have not been freed.
- **Heavy Hitter Report:** Identifies locations in the code that are responsible for the largest memory allocations.
- **Statistics Reporting:** Outputs detailed statistics on memory usage, including total allocated, active, and failed memory sizes and counts.

## Functions

### `dmalloc_malloc(sz, file, line)`
Allocates a block of memory of size `sz`. The allocation is tracked with metadata, and the memory is protected by canaries.

**Arguments:**
- `sz` (size_t): The size of the memory block to allocate.
- `file` (const char*): The file where the allocation is made.
- `line` (long): The line number where the allocation is made.

**Returns:** A pointer to the allocated memory block.

### `dmalloc_free(ptr, file, line)`
Frees the dynamically allocated memory pointed to by `ptr`. Checks for double-free, wild writes, and invalid memory access.

**Arguments:**
- `ptr` (void*): The pointer to the memory block to free.
- `file` (const char*): The file where the deallocation is called.
- `line` (long): The line number where the deallocation is called.

### `dmalloc_calloc(nmemb, sz, file, line)`
Allocates an array of `nmemb` elements, each of size `sz`, and initializes the memory to zero.

**Arguments:**
- `nmemb` (size_t): The number of elements in the array.
- `sz` (size_t): The size of each element.
- `file` (const char*): The file where the allocation is made.
- `line` (long): The line number where the allocation is made.

**Returns:** A pointer to the newly allocated memory block, initialized to zero.

### `dmalloc_get_statistics(stats)`
Stores the current memory statistics into the provided `dmalloc_statistics` structure.

**Arguments:**
- `stats` (dmalloc_statistics*): A pointer to the structure that will hold the memory statistics.

## Memory Management

The `dmalloc` library uses metadata structures to track memory allocations. For every allocation, a metadata structure is created and linked into a doubly-linked list. Each metadata structure contains information about the size of the allocation, the file and line of allocation, and pointers to the previous and next allocations.

### Canary Protection

To prevent memory overflows and underflows, each allocated memory block is protected with canary values. These values are checked during both memory allocation and deallocation. If a memory corruption is detected (i.e., a canary value is overwritten), the program will raise an error to signal potential issues.

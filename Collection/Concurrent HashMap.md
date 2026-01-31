
A concurrent HashMap 
1. thread safe, 
2. high performance implementation of the Map interface, 
3. it allows concurrent access and updates by multiple threads without locking the entire map,
4. making it suitable for multi-threaded environments.


Key Features:
1. Allows concurrent read and write operations
2. No need to synchronize the whole map
3. High throughput in concurrent scenarios

Internal working:
1. segmentation (java 7 and below):
	1. Map was divided into segments (like mini hash tables).
	2. Each segment had its own lock.
	3. Multiple threads could access different segments simultaneously.
	4. Only one thread could modify a segment at a time.
2. Bucket Locking (java 8+)
	1. Segments are removed; uses a single array of bins( buckets)
	2. Each bin can be a linked list or a tree (for high collision bins)
	3. Uses fine grained locking (synchronized on individual bins) for updates
	4. Read operations are mostly lock-free using volatile and CAS (compare-and swap) operations
3. Put operations:
	1. computes hash and finds the bin
	2. if the bin is empty, uses CAS to insert
	3. if not, synchronizes on the bin and updates the linked list/tree
	4. if the bin becomes too large, it is converted to a tree for faster lookups.
4. Get Operations:
	1. computes hash and finds the bin
	2. traverses the bin (linked list or tree) to find the key
	3. No locking is needed for most reads.
5. Resize operations:
	1. when the map grows, it resizes the internal array
	2. resizing is done in a way that allows other threads to continue working
6. thread safety:
	1. uses volatile for visibility
	2. uses CAS for atomic updates
	3. synchronizes only on bins for updates, not the whole map.


Advantages:
1. High concurrency for both reads and writes
2. No global lock, so threads are less likely to block each other
3. safe for use by multiple threads without external synchronization

Limitations:
1. Null keys and values are not allowed
2. iterators are weakly consistent (they reflect some, but not necessarily all, changes)


## How locking Handling internally:
Java 7 and below: The map is divided into segments, each with its own lock. Only the segment being modified is locked, allowing high concurrency.

Java 8+: the implementation uses a lock-free approach for most read operations using volatile variables and CAS. For write operations, it uses fine grained locks at the buckets level, locking only the affected buckets during updates.
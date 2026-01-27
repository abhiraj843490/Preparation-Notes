Internal structure:

_HashMap_ uses a hash table to store its key-value pairs, which allows for fast lookups and retrieval of data. However, the order of the elements is not guaranteed. 

_LinkedHashMap_ extends _HashMap_ and adds a doubly linked list to maintain the insertion order of the entries. This means that the order in which elements are added is preserved during iteration.

Performance:

_HashMap_ is slightly faster than _LinkedHashMap_ because it doesn’t need to maintain the insertion order. Most operations, like adding, removing, and looking up elements, take constant time, O(1).

_LinkedHashMap_ performs similarly for these operations but has a slight overhead due to the linked list that maintains the order.
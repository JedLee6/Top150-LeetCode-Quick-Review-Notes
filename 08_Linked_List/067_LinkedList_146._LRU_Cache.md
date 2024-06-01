# 067_LinkedList_146. LRU Cache

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

 

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

 

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 104`
- `0 <= value <= 105`
- At most `2 * 105` calls will be made to `get` and `put`.



### Intuition and Main Idea

An LRU cache needs to support two main operations efficiently: `get` and `put`. The challenge is to achieve these operations in \( O(1) \) time complexity. Here's the strategy to achieve this:

1. **HashMap for Fast Access**: We'll use a `HashMap` to store the key-value pairs, which provides \( O(1) \) average time complexity for both `get` and `put` operations.

2. **Doubly Linked List for Order**: To keep track of the order of usage, we'll use a doubly linked list. The most recently used item will be at the head of the list, and the least recently used item will be at the tail. This allows us to quickly move nodes to the head on access and remove nodes from the tail when the capacity is exceeded.

By combining these two data structures, we can achieve the required \( O(1) \) operations for both `get` and `put`.

### Detailed Solution with Annotations

```java
class LRUCache {
    // The class `Node` represents each entry in the doubly linked list. It has `key` and `value` fields, and two pointers pointing to the previous and next nodes.
    class Node {
        int key, value;
        Node prev, next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    private final int capacity;  // Maximum capacity of the cache
    private final Map<Integer, Node> map;  // HashMap to store key to node mappings for O(1) access
    // Dummy nodes to represent the head and tail of the doubly linked list. These help simplify the insert and remove operations
    private final Node head, tail;
    // Implement the LRUCache constructor method with initialization code
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>(capacity);
        // Initialize dummy head and tail nodes and their pointers
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    // Return the value of the key if the key exists, otherwise return -1
    public int get(int key) {
    	// Retrieves the node for the given key from the map 
        Node node = map.get(key);
    	// If the key is not found, it returns -1
        if (node == null) {
            return -1;  // Key not found
        }
        // If the key is found, move the accessed node to the head of the list to mark it as most recently used
        remove(node);
        // Insert the accessed node at the head of the doubly linked list
        insertToHead(node);
        return node.value;
    }
    // Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
    public void put(int key, int value) {
        // Retrieves the node for the given key from the map 
        Node node = map.get(key);
        // If the key exists, updates the value and moves the node to the head of the list.
        if (node != null) {
            // Update the value of the existing node
            node.value = value;
            // Move the updated node to the head
            remove(node);
            // Insert the new node at the head of the doubly linked list
            insertToHead(node);
        } else {
            // If the key does not exist, we'll check the capacity. If the cache is full, remove the least recently used node from the tail.
            if (map.size() >= capacity) {
                // Get the reference of the least recently used node
                Node lru = tail.prev;
                // Remove the least recently used node from the doubly linkd list and map
                remove(lru);
                map.remove(lru.key);
            }
            // After removing the least recently used node, we can add the new node to the LRUCache
            // Create the new node
            Node newNode = new Node(key, value);
            // Put the new node into the map
            map.put(key, newNode);
            // Insert the new node at the head of the doubly linked list
            insertToHead(newNode);
        }
    }
    // Helper method to remove a node from the doubly linked list  by adjusting the pointers of the adjacent nodes
    private void remove(Node node) {
        // Make the previous node of the input node's next pointer point to the next node of the input node
        node.prev.next = node.next;
        // Make the next node of the input node's previous pointer point to the previous node of the input node
        node.next.prev = node.prev;
    }
    // Helper method to insert a node at the head of the doubly linked list by adjusting the pointers
    private void insertToHead(Node node) {
        // Make the input node's next pointer point to the next node of the head node
        node.next = head.next;
        // Make the input node's previous pointer point to the head node
        node.prev = head;
        // Make the next node of the head node's previous pointer point to the input node
        head.next.prev = node;
        // Make the head node's next pointer point to the input node
        head.next = node;
    }
}
```

### Time Complexity

Both the `get` and `put` operations run in \( O(1) \) time complexity due to the combination of the `HashMap` and the doubly linked list.

### Space Complexity

The space complexity is \( O(n) \), where \( n \) is the capacity of the cache. This is because we store up to `capacity` nodes and their mappings in the `HashMap`.

### Alternative Solution

Another potential solution could involve using a `LinkedHashMap`, which maintains insertion order and provides methods to remove the eldest entry. However, the custom implementation with a `HashMap` and a doubly linked list is more flexible and directly meets the problem's requirements.

### Conclusion

The chosen solution effectively combines a `HashMap` for fast access and a doubly linked list for maintaining the order of usage, ensuring both `get` and `put` operations run in \( O(1) \) time complexity. This approach is optimal and widely used for implementing LRU caches.
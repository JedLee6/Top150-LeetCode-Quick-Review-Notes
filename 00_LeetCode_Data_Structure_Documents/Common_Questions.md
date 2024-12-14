## LeetCode Data Structure Documents - Common Questions

## 1 what's the time complexity of add() in ArrayList of java, O(1) or O(n)?


The **time complexity** of the `add()` method in **`ArrayList`** depends on the context of its use:

### **1. Adding to the End of the ArrayList**
- If the current capacity of the `ArrayList` is **sufficient** to accommodate the new element:
  - **Time Complexity**: **O(1)** (amortized constant time).
  - Reason: The new element is directly placed at the next available index in the internal array, which is a constant-time operation.

- If the current capacity is **insufficient** (i.e., the internal array needs to be resized):
  - **Time Complexity**: **O(n)** (due to resizing).
  - Reason: When resizing occurs:
    1. A new larger array (typically 1.5x or 2x the previous size) is created.
    2. All elements from the old array are copied to the new array, which takes $O(n)$, where $n$ is the number of elements in the `ArrayList`.

**Amortized Analysis**:
- Resizing does not happen on every addition but only occasionally when the capacity is exceeded. Over a large number of insertions, the **amortized time complexity** of `add()` is **O(1)** because the cost of resizing is spread across many `add()` calls.

---

### **2. Adding to a Specific Index**
When adding an element at a specific index:
- **Time Complexity**: **O(n)**.
- Reason:
  1. All elements after the specified index must be shifted one position to the right to make space for the new element.
  2. The cost of shifting elements grows with the size of the list, resulting in $O(n)$ complexity in the worst case.

---

### **Summary**
| Operation                   | Time Complexity      |
| --------------------------- | -------------------- |
| `add(element)` (at the end) | **O(1)** (amortized) |
| `add(index, element)`       | **O(n)**             |

### **Practical Implications**
- For use cases where frequent additions are expected at the end of the list, **`ArrayList`** is highly efficient due to the amortized $O(1)$ complexity.
- For insertions at arbitrary positions, a **LinkedList** may be preferred as it avoids shifting elements (though it has other trade-offs).

## 2 Is there any data structure in java supporting both Stack and Queue feature?

Yes, in **Java**, the `Deque` (short for "Double Ended Queue") interface in the `java.util` package supports both **stack** and **queue** operations. A `Deque` allows insertion and removal of elements from both ends, making it versatile enough to function as both a stack and a queue.

> A **Double Ended Queue (Deque)** is a data structure that allows adding and removing elements from both the **front** and the **rear** ends. In contrast, a normal **Queue** allows insertion at one end (rear) and removal at the other end (front), adhering to the **FIFO (First In, First Out)** principle.

### **Using `Deque` as a Stack**

- You can use **LIFO (Last In, First Out)** operations with methods like:
  - `push(E e)`: Pushes an element onto the stack (to the front of the deque).
  - `pop()`: Removes and returns the element from the top of the stack (from the front).
  - `peek()`: Retrieves, but does not remove, the element at the top of the stack.

### **Using `Deque` as a Queue**
- You can use **FIFO (First In, First Out)** operations with methods like:
  - `offer(E e)`: Adds an element to the queue (to the end of the deque).
  - `poll()`: Retrieves and removes the head of the queue (from the front).
  - `peek()`: Retrieves, but does not remove, the head of the queue.

### Example Code

```java
import java.util.Deque;
import java.util.LinkedList;

public class DequeAPIDemo {
    public static void main(String[] args) {
        // Initialize a Deque using LinkedList
        Deque<String> linkedList = new LinkedList<>();
        // 1. Adding elements
        linkedList.addFirst("A"); // Add to the front as a List
        linkedList.addLast("B");  // Add to the back as a List
        linkedList.offerFirst("C"); // Add to the front as a List
        linkedList.offerLast("D");  // Add to the back as a List
        System.out.println("After adding elements: " + linkedList); // Output: [C, A, B, D]
        // 2. Accessing elements without removing
        System.out.println("First element (getFirst): " + linkedList.getFirst()); // Output: C, , throw exception if this list is empty.
        System.out.println("Last element (getLast): " + linkedList.getLast());   // Output: D
        // peek() is equivalent to peekFirst(): Retrieves, but does not remove, the head (first element) of this list, return null if this list is empty.
        System.out.println("Peek first element (peek): " + linkedList.peek()); // Output: C
        System.out.println("Peek first element (peekFirst): " + linkedList.peekFirst()); // Output: C
        System.out.println("Peek last element (peekLast): " + linkedList.peekLast());   // Output: D
        // 3. Removing elements, throw exception if this list is empty
        System.out.println("Removed first element (removeFirst): " + linkedList.removeFirst()); // Output: C
        System.out.println("Removed last element (removeLast): " + linkedList.removeLast());   // Output: D
        System.out.println("After removing elements: " + linkedList);// Output: [A, B]
        // 4. Stack-like operations
        linkedList.push("E"); // Push to the front (equivalent to addFirst)
        System.out.println("After push: " + linkedList); // Output: [E, A, B]
        linkedList.peek(); // Returns the first element as a list without removing, the returned value is "E"
        System.out.println("Pop element (pop): " + linkedList.pop()); // Removes and returns the first element (equivalent to removeFirst)
        System.out.println("After pop: " + linkedList); // Output: [A, B]
        // 5. Queue-like operations
        linkedList.offer("F"); // Add to the rear (equivalent to addLast)
        System.out.println("After offer: " + linkedList); // Output: [A, B, F]
        System.out.println("Poll element (poll): " + linkedList.poll()); // Removes and returns the first element, return null if this list is empty
        System.out.println("After poll: " + linkedList); // Output: [B, F]
        // 6. Iteration
        System.out.println("Elements in deque (iterator):");
        for (String element : linkedList) {
            System.out.print(element + " "); // Output: B F
        }
        // 7. Size and emptiness checks
        System.out.println("Size of deque: " + linkedList.size()); // Output: 2
        System.out.println("Is deque empty? " + linkedList.isEmpty()); // Output: false
        // 8. Clear the deque
        linkedList.clear();
        System.out.println("After clearing, is deque empty? " + linkedList.isEmpty()); // Output: true
    }
}
```

### **Advantages of Using `Deque`**
1. **Versatility**: It can act as both a stack and a queue.
2. **Efficiency**: Operations such as `push`, `pop`, `offer`, and `poll` are O(1) since they operate on the ends of the deque.
3. **Thread Safety**: If needed, you can use `LinkedBlockingDeque` for thread-safe operations.

### **Common Implementations of `Deque`**
- **`ArrayDeque`**: Resizable-array implementation of `Deque` (faster than `LinkedList` for most use cases).
- **`LinkedList`**: Doubly-linked list implementation of `Deque`.

### **Conclusion**
The `Deque` interface (implemented by classes like `ArrayDeque` or `LinkedList`) is a powerful data structure in Java that supports both **stack** and **queue** functionalities seamlessly.

### 2.1 **Does a Normal Queue Have Two Ends?**

Yes, a **normal Queue** (like `LinkedList`) technically has two ends:

1. **Rear**: Where elements are added.
2. **Front**: Where elements are removed.

However, **normal Queue operations are restricted**:

- You can **only add** elements at the rear.
- You can **only remove** elements from the front.

A **Deque** extends this by allowing operations at both ends (add/remove from front and rear), making it more flexible than a normal Queue.

------

### 2.2 **Why Do We Still Need Queue or Stack in Java When We Have Deque?**

Even though a **Deque** can mimic both **Queue** and **Stack**, there are reasons why `Queue` and `Stack` still exist as distinct concepts and interfaces in Java:

#### **1. Semantics and Usage Constraints**

- **Queue**:
    - Represents the **FIFO** principle explicitly.
    - Code that uses a `Queue` ensures that operations are **semantically clear** and adheres to FIFO behavior.
    - Example: Using `Deque` for a queue may introduce unintended `LIFO` operations since it allows flexibility, potentially leading to bugs.
- **Stack**:
    - Represents the **LIFO** principle explicitly.
    - Code that uses a `Stack` ensures that operations adhere to this behavior.

------

#### **2. Simplified APIs**

Queue and Stack offer specialized methods that directly correspond to their intended usage:

Queue: offer(), poll(), peek()

Stack: push(), pop(), peek()

Using a Deque requires the programmer to carefully choose appropriate methods (addFirst, addLast, removeFirst, removeLast, etc.), increasing the chance of mistakes.

------

#### **3. Reduced Flexibility**

- In scenarios where only **FIFO** or **LIFO** behavior is needed, `Queue` or `Stack` enforces these constraints at the type level. This makes code easier to understand, maintain, and debug.
- A `Deque` provides more flexibility but can be overkill or even error-prone in simple FIFO or LIFO scenarios.

------

### **Advantages of Queue and Stack Over Deque**

| Aspect               | Queue                                        | Stack                                        |
| -------------------- | -------------------------------------------- | -------------------------------------------- |
| **Clarity**          | Represents FIFO explicitly.                  | Represents LIFO explicitly.                  |
| **Simplicity**       | Provides a focused API for queue operations. | Provides a focused API for stack operations. |
| **Type-Safety**      | Guarantees intended behavior (FIFO or LIFO). | Guarantees intended behavior (LIFO).         |
| **Code Readability** | Code is more intuitive for FIFO scenarios.   | Code is more intuitive for LIFO scenarios.   |

------

### 2.3 **When to Use Deque Instead of Queue or Stack**

You should use a **Deque** if:

1. You need both **stack-like (LIFO)** and **queue-like (FIFO)** behavior in the same application.
2. You need operations at both ends of the data structure (e.g., adding/removing from front and rear).
3. You want a single versatile data structure rather than using separate `Queue` and `Stack`.

------

### **Conclusion**

- **Deque** is more flexible and powerful, but **Queue** and **Stack** are simpler and enforce specific behaviors at the API level.
- Use **Queue** for FIFO operations, **Stack** for LIFO operations, and **Deque** when you need flexibility to perform operations at both ends or mix FIFO and LIFO behaviors.

### 2.4 Difference between **ArrayDeque** and **LinkedList**:

Both `ArrayDeque` and `LinkedList` are implementations of the `Deque` interface in Java, which represents a double-ended queue. However, they differ significantly in their underlying data structure, performance, and usage.

#### **1. Underlying Data Structure**:
- **ArrayDeque**:
  - Uses a **circular dynamic array** as its internal structure.
  - The array dynamically resizes when the capacity is exceeded.
  - Indices are used to track the front and rear of the deque.

- **LinkedList**:
  - Uses a **doubly linked list** as its internal structure.
  - Each element is stored in a node containing references to the previous and next nodes.

---

#### **2. Performance**:
| Operation              | ArrayDeque            | LinkedList                 |
| ---------------------- | --------------------- | -------------------------- |
| Add to front/back      | **O(1)** (amortized)  | **O(1)**                   |
| Remove from front/back | **O(1)**              | **O(1)**                   |
| Random access          | **O(n)**              | **O(n)**                   |
| Memory overhead        | Lower (compact array) | Higher (pointers in nodes) |

- **ArrayDeque**:
  - Adding and removing elements at both ends is **O(1)** (amortized), as it adjusts indices or resizes the array when needed.
  - Random access requires **O(n)** traversal.
  - Memory overhead is minimal because it's backed by an array (no need for pointers).

- **LinkedList**:
  - Adding and removing elements at both ends is **O(1)** because it involves adjusting the node pointers.
  - Memory overhead is higher due to the storage of pointers (two per node: `next` and `prev`).
  - Accessing an element at a specific index is **O(n)** because it requires traversal from the head or tail.

---

#### **3. When to Use**:
- **Use ArrayDeque**:
  - When you need a double-ended queue with **fast, compact memory usage**.
  - Suitable for stack or queue operations due to lower memory overhead and fast performance.

- **Use LinkedList**:
  - When you need frequent **insertions/removals in the middle of the list**.
  - If memory overhead is not a concern and you want a simple linked structure.

---

### Principles of **ArrayDeque**:

The **ArrayDeque** is backed by a **circular array**. This means the internal array is treated as if it wraps around in a circle. Here's how it works:

1. **Circular Array**:
   - Elements are stored in an array.
   - Two pointers (`head` and `tail`) are used to track the front and rear of the deque.
   - When the front or rear reaches the end of the array, it wraps around to the beginning, leveraging the modular arithmetic (`index % array.length`).

2. **Adding to the Front or Back**:
   - To add an element to the **front**, `head` is decremented (or wrapped around if it goes below 0) and the element is placed there.
   - To add an element to the **back**, `tail` is incremented (or wrapped around if it reaches the end of the array).

3. **Dynamic Resizing**:
   - When the array is full, a new array with double the capacity is created.
   - All elements are copied to the new array, starting with the element at `head`.
   - The `head` and `tail` are updated accordingly to fit the new array.

4. **Efficiency**:
   - Adding an element to the front or back is typically **O(1)** because it only involves adjusting an index and assigning a value.
   - Resizing is **O(n)**, but this is amortized over many operations, resulting in an average cost of **O(1)** for additions.

---

### 2.5 Why Adding an Element to the Front Costs **O(1)** in **ArrayDeque**:

The **O(1)** complexity for adding an element to the front of an `ArrayDeque` comes from the following:
1. The operation involves **decrementing the `head` pointer** (or wrapping it around using modular arithmetic).
2. The new element is directly inserted at the adjusted index in the array.
3. No shifting of elements is required because the array is circular.

Unlike an `ArrayList`, which requires shifting elements to make room for new elements at the front, `ArrayDeque` avoids this by using the circular array approach.

---

### Example of How **ArrayDeque** Works Internally:

#### Initial State (Array size = 8, empty deque):
```
[ null, null, null, null, null, null, null, null ]
  head -> 0
  tail -> 0
```

#### Add 10 to the back:
```
[ 10, null, null, null, null, null, null, null ]
  head -> 0
  tail -> 1
```

#### Add 20 to the back:
```
[ 10, 20, null, null, null, null, null, null ]
  head -> 0
  tail -> 2
```

#### Add 5 to the front:
```
[ 10, 20, null, null, null, null, null, 5 ]
  head -> 7  (wrapped around)
  tail -> 2
```

#### Remove from front:
```
[ 10, 20, null, null, null, null, null, null ]
  head -> 0
  tail -> 2
```

This shows how the **head** and **tail** pointers dynamically adjust to achieve O(1) operations for adding/removing elements at both ends.

---

### Key Differences Recap:

- **ArrayDeque** is more memory-efficient and faster for most use cases but requires resizing (amortized O(1)).
- **LinkedList** supports efficient middle operations but has higher memory overhead.

### 2.6 Is dynamic array equal to circular array?


A **dynamic array** and a **circular array** are **not the same**, although they can sometimes be used together. Let’s break down both concepts to understand their similarities and differences.

---

## **1. Dynamic Array**

A **dynamic array** is a resizable array that can grow (or sometimes shrink) in size when elements are added beyond its current capacity. It is often implemented using a **fixed-size array** internally, but when the array becomes full, it allocates a new array with a larger size and copies the existing elements over.

### Characteristics:
- **Resizable**: Unlike a static array, its size can change dynamically.
- **Doubling Strategy**: When the array reaches its capacity, it is resized to a larger size (usually double the previous size) to minimize the frequency of resizing.
- **Amortized O(1)** for appending an element: Resizing takes **O(n)** time, but this cost is spread out across many operations.
- Used in structures like **ArrayList** in Java or `std::vector` in C++.

### Example: Resizing in a Dynamic Array
Imagine we have a dynamic array with an initial capacity of 4:
```
[ 10, 20, 30, 40 ]
```
Adding one more element (50) causes the array to resize:
1. A new array with **double** the size (capacity = 8) is created.
2. Existing elements are copied to the new array.
3. The new element is added:
```
[ 10, 20, 30, 40, 50, null, null, null ]
```

---

## **2. Circular Array**

A **circular array** (or circular buffer) is an array where the **end of the array wraps around to the beginning**, forming a logical circle. This is often implemented using two pointers (or indices) for the **front** and **rear** positions. It is commonly used for **queue** operations, especially when both ends of the array need to be accessed efficiently.

### Characteristics:
- The array behaves as if it is "circular," so when you reach the end, you wrap around to the beginning.
- Used to optimize operations like adding/removing elements from both ends of a queue.
- Requires modular arithmetic: `index % array.length` to ensure wrap-around.
- Fixed size: A circular array generally has a fixed size unless combined with dynamic resizing.

### Example of Circular Array:
For a queue with capacity 5:
```
[ null, null, null, null, null ]
   front -> 0
   rear  -> 0
```
Adding 10, 20, and 30:
```
[ 10, 20, 30, null, null ]
   front -> 0
   rear  -> 3
```
Removing 10 (front moves forward):
```
[ null, 20, 30, null, null ]
   front -> 1
   rear  -> 3
```
Adding 40 and 50:
```
[ null, 20, 30, 40, 50 ]
   front -> 1
   rear  -> 0 (wrapped around)
```

---

## **Dynamic Array vs Circular Array**

| Feature               | **Dynamic Array**                    | **Circular Array**                                  |
| --------------------- | ------------------------------------ | --------------------------------------------------- |
| **Resizing**          | Can grow dynamically (double size).  | Fixed size unless combined with a dynamic strategy. |
| **Wrap-around**       | No wrap-around. Logical end is end.  | Wrap-around behavior (like a circle).               |
| **Usage**             | Common for dynamic lists or vectors. | Common for queues or ring buffers.                  |
| **Indexing**          | Linear indexing (simple access).     | Modular arithmetic for indexing.                    |
| **Memory Management** | Resizes by creating a larger array.  | Fixed size (memory-efficient).                      |

---

## **Are They Equal?**

- **No**, a dynamic array is not the same as a circular array.
- A **dynamic array** can grow in size and doesn’t inherently have circular behavior.
- A **circular array** wraps around the end of the array to the beginning but typically has a fixed size.

---

## **Combination of Both**:
Sometimes, **circular arrays** and **dynamic arrays** are used together, such as in **ArrayDeque** in Java:
- It uses a **circular array** for efficient front and rear operations.
- When the circular array becomes full, it **resizes dynamically** (doubling the size).

This combination ensures **O(1)** (amortized) addition/removal operations at both ends and dynamic growth when needed.

---

### Summary:
- **Dynamic Array**: A resizable array that grows as needed (e.g., `ArrayList`).
- **Circular Array**: An array where the end wraps around to the beginning (e.g., circular buffer).
- Combined, they provide both resizable storage and efficient access at both ends.
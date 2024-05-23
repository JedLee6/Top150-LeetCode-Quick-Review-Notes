# 060_LinkedList_138. Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/e2.png)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

**Example 3:**

**![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/e3-20240523231921671.png)**

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

 

**Constraints:**

- `0 <= n <= 1000`
- `-104 <= Node.val <= 104`
- `Node.random` is `null` or is pointing to some node in the linked list.



### Intuition and Main Idea

To solve the problem of creating a deep copy of a linked list with both `next` and `random` pointers, we need to ensure that we correctly replicate the structure and connections of the original list. The challenge is to maintain the random pointers' integrity, which could point to any node in the list or be null.

The main idea is to use a combination of two passes through the linked list:
1. First Pass: Create a copy of each node and insert it immediately after the original node in the original list. This helps in easily establishing the `random` pointers for the copied nodes in the second pass.
2. Second Pass: Assign the `random` pointers for the copied nodes and separate the original and copied nodes to form the final deep-copied list.

### Step-by-Step Solution with Detailed Explanation

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;
    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
public class Solution {
    // Main function to copy the linked list with random pointers
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null; // If the original list is empty, return null
        }
        // Step 1: Create new nodes and place them next to the original nodes
        Node current = head;
        while (current != null) {
            Node newNode = new Node(current.val); // Create a new node with the same value
            newNode.next = current.next; // Link the new node to the next original node
            current.next = newNode; // Insert the new node right after the original node
            current = newNode.next; // Move to the next original node
        }
        // Step 2: Set the random pointers for the new nodes
        current = head;
        while (current != null) {
            if (current.random != null) {
                current.next.random = current.random.next; // Set the random pointer for the new node
            }
            current = current.next.next; // Move to the next original node
        }
        // Step 3: Separate the new nodes to form the deep-copied list
        Node original = head;
        Node copy = head.next;
        Node newHead = copy; // Head of the new copied list
        while (original != null) {
            original.next = original.next.next; // Restore the original list
            if (copy.next != null) {
                copy.next = copy.next.next; // Separate the copied list
            }
            original = original.next; // Move to the next original node
            copy = copy.next; // Move to the next copied node
        }
        return newHead; // Return the head of the deep-copied list
    }
}
```

### Analysis of Time Complexity and Space Complexity

#### Time Complexity

- **First Pass**: Creating the new nodes and inserting them next to the original nodes takes \(O(n)\) time, where \(n\) is the number of nodes in the linked list.
- **Second Pass**: Setting the random pointers for the new nodes also takes \(O(n)\) time.
- **Third Pass**: Separating the new nodes to form the deep-copied list takes \(O(n)\) time.

Overall, the time complexity is \(O(n)\).

#### Space Complexity

- The algorithm uses constant extra space for pointers and variables (`current`, `original`, `copy`, `newHead`). The space complexity is \(O(1)\) since we are not using any additional data structures that grow with the size of the input list.

### Alternative Solution: Using HashMap

An alternative solution involves using a `HashMap` to store the mapping between original nodes and their corresponding copied nodes. This approach also requires two passes through the linked list.

The main idea is to use a combination of two passes through the linked list:

1. **Traverse the original linked list**: We traverse the original linked list and for each node encountered, we create a new node with the same value. We maintain a mapping between the original nodes and their corresponding copies using a HashMap.
2. **Set the next pointers and random pointer**: After completing the first pass, we traverse the original linked list again. For each node, set the next pointer and random pointer of each copy node by looking up in the HashMap.
3. **Return the head of the copied list**: Finally, we return the head of the copied list.

#### Step-by-Step Solution with HashMap

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;
    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
public class Solution {
    // Main function to copy the linked list with random pointers using HashMap
    public Node copyRandomList(Node head) {
        // If the original list is empty, return null
        if (head == null) {
            return null; 
        }
        // Step 1: Create a HashMap to store the mapping between original nodes and new nodes
        HashMap<Node, Node> map = new HashMap<>();
        Node current = head;
        // Step 2: First Pass, create new nodes and store them in the map
        while (current != null) {
            map.put(current, new Node(current.val)); // Map the original node to the new node
            current = current.next; // make current pointer points to the next node
        }
        current = head; // reset current pointer to head for the second pass
        // Step 3: Second Pass, assign next and random pointers using the map
        while (current != null) {
            // Set the next pointer of each copy node by looking up the next node in the HashMap by the original next node
            map.get(current).next = map.get(current.next);
            // Set the random pointer of each copy node by looking up the random node in the HashMap by the original random node
            map.get(current).random = map.get(current.random); 
            current = current.next;  // make current pointer points to the next node
        }
        // Return the head of the deep-copied list
        return map.get(head);
    }
}
```

### Analysis of Time Complexity and Space Complexity for HashMap Solution

#### Time Complexity

- **First Pass**: Creating the new nodes and storing them in the HashMap takes \(O(n)\) time.
- **Second Pass**: Setting the `next` and `random` pointers for the new nodes also takes \(O(n)\) time.

Overall, the time complexity is \(O(n)\).

#### Space Complexity

- The HashMap uses \(O(n)\) space to store the mapping between original nodes and new nodes.

Overall, the space complexity is \(O(n)\).

### Conclusion

Both solutions are efficient and solve the problem of deep copying a linked list with `random` pointers. The first solution with the three-pass approach has \(O(n)\) time complexity and \(O(1)\) space complexity, making it more space-efficient. The alternative solution with the HashMap also has \(O(n)\) time complexity but uses \(O(n)\) space due to the HashMap. The choice between these methods can depend on the constraints and requirements of the specific use case.
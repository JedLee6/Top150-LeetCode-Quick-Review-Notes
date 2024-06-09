# 074_Binary_Tree_General_117. Populating Next Right Pointers in Each Node II

Given a binary tree

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/117_sample.png)

```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

**Example 2:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 6000]`.
- `-100 <= Node.val <= 100`

 

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.



### Intuition and Main Idea

To solve the problem of populating each next pointer in a binary tree to point to its next right node, we need to understand the structure of the binary tree and the properties of the `next` pointer:
- Each node has a `left` and `right` child, as well as a `next` pointer that should be set to its next right node on the same level.
- If there is no next right node on the same level, the `next` pointer should be set to `NULL`.

### Approach 1: Level Order Traversal Using a Queue

The first approach uses a level order traversal (also known as breadth-first traversal) with the help of a queue. This method processes nodes level by level, connecting the `next` pointers for nodes on the same level.

**Step-by-Step Explanation:**
1. Use a queue to perform a level order traversal of the tree.
2. For each level, keep track of the previous node and set its `next` pointer to the current node.
3. At the end of each level, ensure the last node's `next` pointer is set to `NULL`.

Here's the code implementation with detailed annotations:

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
public class Solution {
    public Node connect(Node root) {
        // If the root is null, return null immediately since there's nothing to connect
        if (root == null) {
            return null;
        }
        // Initialize a queue for level order traversal (breadth-first search)
        Queue<Node> queue = new LinkedList<>();
        // Add the root node to the queue to start the traversal
        queue.add(root);
        // Continue processing nodes until the queue is empty
        while (!queue.isEmpty()) {
            // Get the number of nodes at the current level
            int size = queue.size();
            // Initialize a variable to keep track of the previous node at the current level
            Node prev = null;
            // Iterate through all nodes at the current level
            for (int i = 0; i < size; i++) {
                // Remove the front node from the queue
                Node node = queue.poll();
                // If there is a previous node, connect its next pointer to the current node
                if (prev != null) {
                    prev.next = node;
                }
                // Update the previous node to the current node
                prev = node;
                // Add the left child of the current node to the queue if it exists
                if (node.left != null) {
                    queue.add(node.left);
                }
                // Add the right child of the current node to the queue if it exists
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            // Ensure the last node at the current level points to null
            prev.next = null;
        }
        // Return the root of the modified tree
        return root;
    }
}
```

### Time Complexity

The time complexity of this approach is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because each node is processed exactly once.

### Space Complexity

The space complexity is \(O(n)\) due to the additional space used by the queue to store nodes for each level. In the worst case, the queue will contain all the nodes of the last level of the tree.

### Approach 2: Using Already Established Next Pointers

An alternative approach involves using the already established `next` pointers to traverse the tree without additional space.

**Step-by-Step Explanation:**
1. Use a dummy node to manage the next level's nodes.
2. Use the `next` pointers to traverse the current level and establish the `next` pointers for the next level.

Here's the code implementation with detailed annotations:

```java
public class Solution {
    public Node connect(Node root) {
        // If the root is null, return null immediately since there's nothing to connect
        if (root == null) {
            return null;
        }
        // `current` is initialized to the root node to start the level order traversal
        Node current = root;
        // `dummy` is a dummy node used to mark the start of each next level, as it would point to the start node of each next level
        Node dummy = new Node(0);
        // 'prev' is initially set to `dummy`, which is used to build the 'next' connections for the next level
        Node prev = dummy;
        // The outer loop continues as long as there are levels to process
        while (current != null) {
            // The inner loop iterates through all nodes at the current level
            while (current != null) {
                // If the current node has a left child, connect 'prev.next' to this child
                if (current.left != null) {
                    // Connect 'prev.next' to this child. And if `prev` is exactly `dummy`, `dummy.next` is updated to point into the start node of the next level
                    prev.next = current.left;
                    // Move 'prev' to this newly connected node 'prev.next'
                    prev = prev.next;
                }
                // If the current node has a right child, connect 'prev.next' to this child
                if (current.right != null) {
                    // Connect 'prev.next' to this child. And if `prev` is exactly `dummy`, `dummy.next` is updated to point into the start node of the next level
                    prev.next = current.right;
                    // Move 'prev' to this newly connected node 'prev.next'
                    prev = prev.next;
                }
                // Move `current` to the next node in the current level, as we assigned every next pointer in the last outer iteration, even if `current` is the root node, it's fine, because the root node must not have its next node
                current = current.next;
            }
            // After finishing the current level, move `current` to the first node of the next level `dummy.next`. This is because we made `dummy.next` point into the start node of the next level in the code above
            current = dummy.next;
            // Reset `dummy.next` to null to prepare for building connections in the traversal of next level. If we don't reset it, a endless loop could happen, as in the final outer iteration, `dummy.next` won't be assign to null, and `current` would be assign to `dummy.next` again, which cause the endless loop
            dummy.next = null;
            // Reset 'prev' to the dummy node to start building the next level's connections
            prev = dummy;
        }
        // After the traversal and connection process is complete, return the root of the modified tree
        return root;
    }
}
```

### Time Complexity

The time complexity of this approach is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because each node is processed exactly once.

### Space Complexity

The space complexity is \(O(1)\) since no additional space proportional to the number of nodes is used. The dummy node and pointers used are constant extra space.

### Conclusion

By explaining both the queue-based level order traversal approach and the space-efficient approach using established `next` pointers, we ensure a comprehensive understanding of solving the problem of populating next right pointers in a binary tree.




## 088_Binary_Search_Tree_98. Validate Binary Search Tree

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree1-20250204174534320.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree2-20250204174527722.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`



---

## Problem Recap

Given the root of a binary tree, determine if it is a valid binary search tree (BST). A valid BST satisfies the following for every node:

- All nodes in its left subtree have keys strictly less than the node’s key.
- All nodes in its right subtree have keys strictly greater than the node’s key.
- Both left and right subtrees must also be valid BSTs.

---

## Intuition and Main Idea

The BST property gives us a very useful ordering:
1. **In-order Traversal:**  
   An in-order traversal (left, root, right) of a BST will produce values in strictly increasing order. This observation can be used to check validity by comparing each node’s value with the previously visited node.

2. **Recursive Bounds Checking:**  
   Alternatively, while traversing the tree in pre-order traversal, you can pass down the allowed value range (lower and upper bounds) for each node. Initially, the root can have any value, but as you move left, you update the upper bound, and as you move right, you update the lower bound. If a node’s value is not in its valid range, the tree is not a BST.

Both approaches are valid. Below, I’ll demonstrate multiple solutions.

---

## **Solution 1: Recursive Bounds Checking with pre-order traversal**

**Idea:**  
For each node, maintain a valid range defined by a lower bound and an upper bound. The root node starts with no bounds. When you traverse left, the upper bound becomes the current node’s value; when you traverse right, the lower bound becomes the current node’s value.

**Code with Annotations:**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        // Start recursion with no lower or upper bound
        return isValidBST(root, null, null);
    }
    /**
     * Helper method to validate the BST using bounds.
     * @param node: the current node
     * @param lower: the lower bound (all nodes in this subtree must be greater than this)
     * @param upper: the upper bound (all nodes in this subtree must be less than this)
     * @return true if subtree rooted at node is a valid BST
     */
    private boolean isValidBST(TreeNode node, Integer lower, Integer upper) {
        // Base case: an empty node is valid.
        if (node == null) return true;
        int val = node.val;
        // If there's a lower bound and the current node's value is not greater, return false.
        if (lower != null && val <= lower) return false;
        // If there's an upper bound and the current node's value is not less, return false.
        if (upper != null && val >= upper) return false;
        // Recursively validate the left subtree. The current node's value becomes the new upper bound.
        if (!isValidBST(node.left, lower, val)) return false;
        // Recursively validate the right subtree. The current node's value becomes the new lower bound.
        if (!isValidBST(node.right, val, upper)) return false;
        // If both subtrees are valid, then the current subtree is valid.
        return true;
    }
}
```

**Complexity:**
- **Time:** O(n) — Each node is visited once.
- **Space:** O(h) — Recursive call stack space, where h is the height of the tree (worst-case O(n) for a skewed tree).

---

## **Solution 2: Iterative In-Order Traversal**

**Idea:**  
Use an explicit stack to perform an in-order traversal. As you visit nodes, ensure that each node’s value is greater than the previously visited node’s value.

**Code with Annotations:**

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        // Stack to simulate recursion for in-order traversal.
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        // Variable to store the previous node's value in in-order traversal.
        // It is initialized to null because we haven't visited any nodes yet.
        Integer prev = null;
        // Traverse the tree.
        while (current != null || !stack.isEmpty()) {
            // Reach the leftmost node of the current subtree.
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            // Process the node on the top of the stack.
            current = stack.pop();
            // If the current node's value is not greater than the previous value, it's not a BST.
            if (prev != null && current.val <= prev) {
                return false;
            }
            // Update prev to the current node's value.
            prev = current.val;
            // Move to the right subtree.
            current = current.right;
        }
        // If all nodes are in order, then the tree is a valid BST.
        return true;
    }
}
```

**Complexity:**
- **Time:** O(n) — Each node is visited once.
- **Space:** O(h) — The stack size can go up to the height of the tree (worst-case O(n) for a skewed tree).

---

## **Solution 3: Recursive In-Order Traversal**

**Idea:**  
Perform a recursive in-order traversal. Maintain a global (or class-level) variable that keeps track of the previous node’s value. If at any point the current node’s value is not greater than the previous value, return false.

**Code with Annotations:**

```java
class Solution {
    // Global variable to track the previous node's value in the in-order traversal.
    private Integer prev = null;
    
    public boolean isValidBST(TreeNode root) {
        // Reset the prev value before starting the traversal.
        prev = null;
        return inOrder(root);
    }
    
    /**
     * Helper method that performs an in-order traversal.
     * @param node: current node in the traversal
     * @return true if the subtree rooted at node is a valid BST
     */
    private boolean inOrder(TreeNode node) {
        // Base case: an empty node is valid.
        if (node == null) return true;
        // Traverse the left subtree.
        if (!inOrder(node.left)) return false;
        // Check the current node's value against the previous value.
        if (prev != null && node.val <= prev) return false;
        // Update the previous value.
        prev = node.val;
        // Traverse the right subtree.
        return inOrder(node.right);
    }
}
```

**Complexity:**
- **Time:** O(n) — Each node is visited once.
- **Space:** O(h) — The recursion stack space, where h is the height of the tree.

## Solution 4: Morris In-Order Traversal (Constant Space)

This advanced approach uses **Morris Traversal** to perform an in-order traversal without extra space. It modifies the tree temporarily during traversal. The idea is creating teporary right pointers from a node's in-order predecessor back to the node itself to backtrack without a stack or recursion. As we visit nodes, ensure that each node’s value is greater than the previously visited node’s value.

> We can watch this youtube video to understand it more clearly: https://www.youtube.com/watch?v=wGXB9OWhPTg.

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        // 'prev' will hold the previously visited node's value in the in-order traversal.
        Integer prev = null;
        // 'current' starts at the root of the tree.
        TreeNode current = root;
        // Iterate as long as there is a current node to process.
        while (current != null) {
            // Check if there is left child or not
            if (current.left == null) {
                // If there is no left child, then process the current node.
                // Check if the current node's value is greater than the previous value.
                if (prev != null && current.val <= prev) {
                    // If not, the BST property is violated.
                    return false;
                }
                // Update 'prev' with the current node's value to prepare for next comparison.
                prev = current.val;
                // If the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                current = current.right;
            } else {
                // If a left child do exist, find the in-order predecessor of the current node (the rightmost node in the left subtree).
                TreeNode predecessor = current.left;
                // Create a inner while loop to find the in-order predecessor.
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                // If the temporary link does not exist (`predecessor.right` is null), set the temporary link and move left.
                if (predecessor.right == null) {
                    // Establish a temporary link to the current node.
                    predecessor.right = current;
                    // Move current to its left child to continue traversal.
                    current = current.left;
                } else {
                    // If the temporary link already exists, it means the left subtree has been processed and it's the second time to visit this node. Remove the temporary link and process the node,.
                    // Remove the temporary thread.
                    predecessor.right = null;
                    // Now process the current node.
                    if (prev != null && current.val <= prev) {
                        // If current node's value is not greater than 'prev', it's not a valid BST.
                        return false;
                    }
                    // Update 'prev' with the current node's value to prepare for next comparison.
                    prev = current.val;
                    // If the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                    current = current.right;
                }
            }
        }
        // If we complete the traversal without finding any violations, the tree is a valid BST.
        return true;
    }
}
```

**Time and Space Complexity:**

- **Time Complexity:** O(n) because each node is visited at most twice (once when establishing the temporary link and once when removing it).
- **Space Complexity:** O(1) extra space, aside from the input tree (temporary modifications are undone).

---

## **Summary**

1. **Recursive Bounds Checking:**
   - **Pros:** Directly reflects the BST definition.
   - **Cons:** Requires careful management of bounds.
   - **Time:** O(n), **Space:** O(h).

2. **Iterative In-Order Traversal:**
   - **Pros:** Avoids recursion and its call stack overhead.
   - **Cons:** Slightly more verbose due to explicit stack management.
   - **Time:** O(n), **Space:** O(h).

3. **Recursive In-Order Traversal:**
   - **Pros:** Simple and intuitive if you’re comfortable with recursion.
   - **Cons:** Uses recursion stack space.
   - **Time:** O(n), **Space:** O(h).
4. **Morris Traversal:**  
   - **Pros:** Uses O(1) extra space.  
   - **Cons:** Slightly more complex due to temporary modifications (threads) to the tree structure.

All these approaches efficiently solve the problem. Your choice might depend on your familiarity with recursion versus iteration and the specific constraints or coding style preferred by your team or interviewer.

This detailed explanation with multiple approaches should help demonstrate your deep understanding of the problem and your ability to choose and implement the appropriate solution in Java.
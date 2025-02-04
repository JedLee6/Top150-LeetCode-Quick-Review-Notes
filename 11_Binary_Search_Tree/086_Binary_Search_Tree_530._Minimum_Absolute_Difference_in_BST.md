## 086_Binary_Search_Tree_530. Minimum Absolute Difference in BST

Given the `root` of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.†

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/bst1-20241215175520040.jpg)

```
Input: root = [4,2,6,1,3]
Output: 1
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/bst2.jpg)

```
Input: root = [1,0,48,null,null,12,49]
Output: 1
```

**Constraints:**

- The number of nodes in the tree is in the range `[2, 104]`.
- `0 <= Node.val <= 105`

**Note:** This question is the same as 783: https://leetcode.com/problems/minimum-distance-between-bst-nodes/



### **Intuition and Main Idea**

1. **Properties of a BST**:
   - In a BST, for any node:
     - The left child has a value less than the node.
     - The right child has a value greater than the node.
   - If we perform an **in-order traversal** of a BST, the node values will be visited in sorted order.

2. **Key Insight**:
   - The minimum absolute difference between any two nodes will always occur between two **consecutive values** in the in-order traversal of the BST.

3. **Approach**:
   - Perform an **in-order traversal** to retrieve the node values in sorted order.
   - Compute the absolute differences between consecutive values.
   - Keep track of the smallest difference encountered.

4. **Constraints and Optimization**:
   - Avoid storing all the node values if possible (optimize space).
   - Use only a constant amount of additional space (excluding the recursion stack).

---

### **Solution 1: In-Order Traversal with a List**

In this solution, we store the node values in a list during the in-order traversal, then compute the minimum difference.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int getMinimumDifference(TreeNode root) {
        // Step 1: Store in-order traversal in a list
        List<Integer> values = new ArrayList<>();
        inOrderTraversal(root, values);

        // Step 2: Compute minimum absolute difference
        int minDiff = Integer.MAX_VALUE;
        for (int i = 1; i < values.size(); i++) {
            minDiff = Math.min(minDiff, values.get(i) - values.get(i - 1));
        }
        return minDiff;
    }

    private void inOrderTraversal(TreeNode node, List<Integer> values) {
        if (node == null) {
            return;
        }
        // Recursive left
        inOrderTraversal(node.left, values);
        // Visit current node
        values.add(node.val);
        // Recursive right
        inOrderTraversal(node.right, values);
    }
}
```

#### **Analysis**:
- **Time Complexity**:  
  - The in-order traversal visits each node once: **O(n)**.
  - Calculating differences takes **O(n)**.
  - Overall: **O(n)**.

- **Space Complexity**:  
  - Storing all node values in a list: **O(n)**.
  - Recursion stack for in-order traversal: **O(h)** (where `h` is the height of the tree).

---

### **Solution 2: Optimized In-Order Traversal without Extra Storage**

Instead of storing all node values, we track the previous node value during traversal and calculate the difference on the fly.

```java
class Solution {
    private int minDiff = Integer.MAX_VALUE;
    private Integer prevValue = null; // Tracks the previous node value during traversal and calculate the difference on each step.
    public int getMinimumDifference(TreeNode root) {
        inOrderTraversal(root);
        return minDiff;
    }
    private void inOrderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }
        // Recursive left
        inOrderTraversal(node.left);

        // Visit current node
        if (prevValue != null) {
            // Calculate the difference as they are consecutive values during the in-order traversal
            minDiff = Math.min(minDiff, node.val - prevValue);
        }
        prevValue = node.val;
        // Recursive right
        inOrderTraversal(node.right);
    }
}
```

#### **Analysis**:

- **Time Complexity**:  
  - In-order traversal visits each node once: **O(n)**.

- **Space Complexity**:  
  - No additional storage for values: **O(1)**.
  - Recursion stack for in-order traversal: **O(h)** (where `h` is the height of the tree).

---

### **Solution 3: Iterative In-Order Traversal**

Instead of recursion, we use a stack to perform the in-order traversal iteratively.

```java
import java.util.Stack;

class Solution {
    public int getMinimumDifference(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        Integer prevValue = null;
        int minDiff = Integer.MAX_VALUE;

        // Iterative in-order traversal
        while (!stack.isEmpty() || current != null) {
            // Traverse left subtree
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            // Visit node
            current = stack.pop();
            if (prevValue != null) {
                // Calculate the difference as they are consecutive values during the in-order traversal
                minDiff = Math.min(minDiff, current.val - prevValue);
            }
            prevValue = current.val;

            // Traverse right subtree
            current = current.right;
        }

        return minDiff;
    }
}
```

#### **Analysis**:
- **Time Complexity**:  
  - Traversal still visits each node once: **O(n)**.

- **Space Complexity**:  
  - Stack size depends on the height of the tree: **O(h)**.

---

### **Solution 4: Morris In-Order Traversal (Constant Space)**

This advanced approach uses **Morris Traversal** to perform an in-order traversal without extra space. It modifies the tree temporarily during traversal. The idea is creating teporary right pointers from a node's in-order predecessor back to the node itself to backtrack without a stack or recursion.

> We can watch this youtube video to understand it more clearly: https://www.youtube.com/watch?v=wGXB9OWhPTg.

```java
class Solution {
    public int getMinimumDifference(TreeNode root) {
        // Initialize variables to keep track of the previous node value and the minimum difference.
        Integer prev = null; // Will store the value of the previously visited node.
        int minDiff = Integer.MAX_VALUE;  // Start with the maximum possible difference.
        TreeNode current = root;  // Start traversal from the root.  
        // Begin Morris In-Order Traversal.
        while (current != null) {
            // If there is no left child, process current and move right.
            if (current.left == null) {
                // If prev is set, update minDiff.
                if (prev != null) {
                    // Calculate the difference with previous value.
                    minDiff = Math.min(minDiff, current.val - prev);
                }
                // Update prev to current node's value.
                prev = current.val;
                // If the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                current = current.right;
            } else {
                // Find the inorder predecessor of current.
                TreeNode predecessor = current.left;
                // Navigate to the rightmost node of the left subtree, which is is the rightmost node in the left subtree; The condition "predecessor.right != current" avoids endless loop.
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                // If right child of predecessor is null, establish temporary link to backtrack without extra space.
                if (predecessor.right == null) {
                    predecessor.right = current;  // Create a thread to the current node to backtrack without extra space.
                    current = current.left; // Move to left child to continue traversal.
                } else {
                    // If the thread already exists, it means we've finished left subtree.
                    predecessor.right = null; // Remove the temporary link to avoid endless loop.
                    // Process current node: update minDiff as we did before.
                    if (prev != null) {
                        minDiff = Math.min(minDiff, current.val - prev);
                    }
                    prev = current.val; // Update prev to current value.
                    // After finishing left subtree, if the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                    current = current.right;
                }
            }
        }
        // Return the smallest difference found during traversal.
        return minDiff;
    }
}
```

#### **Analysis**:

- **Time Complexity**:  
  - Visits each node twice: **O(n)**. Each edge in the tree is traversed at most twice (once going down and once coming back up). Thus, the overall time complexity is O(n), where n is the number of nodes in the tree.
- **Space Complexity**:  
  - No extra space (excluding tree modification): **O(1)**. Because it create teporary right pointers from a node's in-order predecessor back to the node itself to backtrack without a stack or recursion. The tree is modified temporarily but restored to its original form, so no additional space is permanently consumed.

---

### **Summary of Solutions**

| Solution               | Time Complexity | Space Complexity | Notes                       |
| ---------------------- | --------------- | ---------------- | --------------------------- |
| **In-Order + List**    | O(n)            | O(n)             | Simple, intuitive           |
| **Optimized In-Order** | O(n)            | O(h)             | Efficient, no extra storage |
| **Iterative In-Order** | O(n)            | O(h)             | Avoids recursion            |
| **Morris Traversal**   | O(n)            | O(1)             | Most space-efficient        |

The **Optimized In-Order Traversal** is usually the best choice due to its balance of simplicity and efficiency.



- 
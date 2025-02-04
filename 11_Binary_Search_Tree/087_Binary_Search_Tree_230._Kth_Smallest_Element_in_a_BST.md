## 086_Binary_Search_Tree_230. Kth Smallest Element in a BST

Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.



**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/kthtree2-20250204161326133.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

 

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

 

---

## Intuition and Main Idea

1. **BST In-Order Property:**  
   Since an in-order traversal of a BST gives the node values in ascending order, the kth element visited during this traversal is the kth smallest element.

2. **Traversals Options:**  
   - **Recursive In-Order Traversal:** Easy to implement, but uses the call stack.
   - **Iterative In-Order Traversal (Stack):** Simulates the recursion explicitly.
   - **Morris In-Order Traversal:** Modifies the tree temporarily to achieve O(1) space (aside from the output).

3. **Approach:**  
   In any approach, we traverse the tree in-order (left, root, right) and keep a counter. When the counter reaches k, we have found our kth smallest element.

---

## Detailed Solutions

### 1. Recursive In-Order Traversal

In this solution, we perform an in-order traversal recursively while maintaining a counter. We also use a variable to store the result once the kth element is reached. To support early termination, we check if the result has already been found before making further recursive calls.

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
class Solution {
    // Counter for visited nodes during in-order traversal.
    private int count = 0;
    // Variable to hold the kth smallest element.
    private int kthSmallestValue = -1;

    public int kthSmallest(TreeNode root, int k) {
        // Start in-order traversal.
        inOrder(root, k);
        return kthSmallestValue;
    }
    
    private void inOrder(TreeNode node, int k) {
        // Base case: if node is null, return.
        if (node == null) return;
        // Traverse left subtree.
        inOrder(node.left, k);
        // Early termination check before processing the current node : If we have already found the kth smallest, return immediately.
        if (kthSmallestValue != -1) return;
        // Process the current node.
        count++;  // Increase the count for this node.
        if (count == k) {
            kthSmallestValue = node.val; // Found kth smallest.
            return;
        }
        // Continue by traversing the right subtree
        inOrder(node.right, k);
    }
}
```

**Complexity:**
- **Time:** O(n) in the worst case (all nodes are visited).
- **Space:** O(h) for the recursion stack, where h is the height of the tree.

---

### 2. Iterative In-Order Traversal Using a Stack

In this approach, we explicitly use a stack to simulate the recursion.

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // Initialize an explicit stack to store nodes.
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        // Iterate until we've processed all nodes.
        while (current != null || !stack.isEmpty()) {
            // The inner loop pushes all left children of the current node onto the stack to reach the leftmost node of the current subtree.
            while (current != null) {
                stack.push(current); // Push current node onto the stack.
                current = current.left;
            }
            // Process the node at the top of the stack, which is the next smallest element in the in-order traversal.
            current = stack.pop();
            k--;  // Decrement k because we've found one more smallest element.
            if (k == 0) {
                // If k becomes zero, current node is the kth smallest.
                return current.val;
            }
            
            // Move to the right subtree to continue the traversal.
            current = current.right;
        }
        // If we haven't returned inside the loop, return -1 (should not happen if k is valid).
        return -1;
    }
}
```

**Complexity:**
- **Time:** O(n) in the worst case.
- **Space:** O(h) where h is the height of the tree (space used by the stack).

---

### 3. Morris In-Order Traversal (Constant Space)

Morris Traversal modifies the tree temporarily to avoid using extra space. We create temporary links (threads) from a node’s in-order predecessor back to the node. This allows us to traverse the tree in-order while using O(1) extra space.

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // Initialize `current` with the root and `count` to track the number of nodes processed.
        TreeNode current = root;
        int count = 0;  // Counter for the number of processed nodes.
        // Begin the while-loop that continues until `current` is null.
        while (current != null) {
            // If there is no left child, we process the current node (increment count and check if it’s the kth smallest).
            if (current.left == null) {
                // No left child, so process this node.
                count++;
                if (count == k) {
                    return current.val;
                }
                // If the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                current = current.right;
            } else {
                // If a left child exists, find the in-order predecessor of the current node (the rightmost node in the left subtree).
                TreeNode predecessor = current.left;
                // Create a inner loop to find the in-order predecessor.
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                // If the thread does not exist (`predecessor.right` is null), set the thread and move left.
                if (predecessor.right == null) {
                    // First time: Create a thread from predecessor to current node.
                    predecessor.right = current;
                    current = current.left;
                } else {
                    // Second time: If the thread already exists, it means the left subtree has been processed. Remove the thread, process the node, and check the kth condition.
                    predecessor.right = null;
                    count++;
                    if (count == k) {
                        return current.val;
                    }
                    // If the current node has real right subtree, then move to the right subtree. Otherwise, the current node backtracks to next in-order node by temporary link.
                    current = current.right;
                }
            }
        }
        // If kth smallest is not found (should not happen for valid k), return -1.
        return -1;
    }
}
```

**Complexity:**

- **Time:** O(n) because each edge is visited at most twice.
- **Space:** O(1) extra space (temporary modifications are made to the tree structure, which is then restored).

---

## Summary

- **Recursive Approach:**  
  - **Pros:** Simple and intuitive.  
  - **Cons:** Uses O(h) space in the recursion stack.
  
- **Iterative Approach (Stack):**  
  - **Pros:** Clear and avoids recursion.  
  - **Cons:** Requires O(h) space for the stack.

- **Morris Traversal:**  
  - **Pros:** Uses O(1) extra space.  
  - **Cons:** Slightly more complex due to temporary modifications (threads) to the tree structure.

Each of these approaches efficiently finds the kth smallest element in a BST. The choice between them depends on your constraints regarding space and code simplicity.


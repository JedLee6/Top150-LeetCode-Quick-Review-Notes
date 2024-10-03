# 077_Binary_Tree_General_124. Binary Tree Maximum Path Sum

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum **path sum** of any **non-empty** path*.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/exx1.jpg)

```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/exx2-20241002194221699.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`



### **Intuition and Main Idea:**

The key challenge of this problem is that the path can start and end at any node, and it can span both the left and right subtrees of any node in the tree. So, we need to explore all potential paths that could give us the maximum sum.

We will solve the problem using a recursive Depth-First Search (DFS) approach. At each node, we'll calculate:
1. The maximum path sum from its left subtree.
2. The maximum path sum from its right subtree.
3. We'll then calculate the potential maximum path sum **passing through this node** and update our global maximum path sum.

Let's go step by step with the code explanation.

### **Solution: Recursive DFS**

```java
class Solution {
    // Global variable to store the maximum path sum during the traversal
    private int maxSum;
    // Main function to calculate the maximum path sum
    public int maxPathSum(TreeNode root) {
        // Initialize maxSum to a very small number to handle negative values in the tree
        maxSum = Integer.MIN_VALUE;
        // We call a helper function to start the DFS traversal from the root node, and we'll update the maxSum inside this method
        dfs(root);
        // Finally, return the global maximum path sum
        return maxSum;
    }
    // Helper function to traverse the tree recursively and calculate maximum path sum that can be obtained from the input node to its parent node.
    private int dfs(TreeNode node) {
        // Base case: if the current node is null, return 0, as a null node does not contribute to the path sum
        if (node == null) return 0;
        // Recursively calculate the maximum path sum from the left subtree
        // If the path sum is negative, we ignore the subtree by taking the maximum of the calculated path sum and 0.
        int leftPathSum = Math.max(dfs(node.left), 0);
        // Recursively calculate the maximum path sum from the right subtree
        // If the path sum is negative, we ignore the subtree by taking the maximum of the calculated path sum and 0.
        int rightPathSum = Math.max(dfs(node.right), 0);
        // Calculate the maximum path sum that passes through the current node
        // This path sum includes the node and both its left and right children
        int currentNodePathSum = node.val + leftPathSum + rightPathSum;
        // Update the global maximum path sum if the current path sum is greater
        maxSum = Math.max(maxSum, currentNodePathSum);
        // Return the providable maximum path sum that this node can contribute to its parent, which is passed back to previous recursion call
        // The providable maximum path sum is made of the maximum of either left or right subtree, plus the current node's value
        return node.val + Math.max(leftPathSum, rightPathSum);
    }
}
```

### **Time and Space Complexity Analysis**:

- **Time Complexity**: 
  - **O(n)**, where `n` is the number of nodes in the tree. We visit each node exactly once during the DFS traversal.
  
- **Space Complexity**: 
  - **O(h)**, where `h` is the height of the tree. This is the space used by the recursion stack. In the worst case (for a skewed tree), this can be O(n), and in the best case (for a balanced tree), it is O(log n).

---

### **Alternative Solution: Using Bottom-Up Dynamic Programming Approach**

Another approach to solve this problem is to think of it in terms of dynamic programming. In this approach, we calculate the maximum path sum at each node using the values calculated at its child nodes. This is essentially a bottom-up approach where the maximum path sum at any node is computed by using the maximum path sums of its left and right children.

However, the recursive DFS approach we used earlier is more intuitive and is already very efficient in terms of both time and space.

---

### **Summary:**

In this problem, we need to find the maximum path sum in a binary tree, where the path can start and end at any node. The key idea is to use a recursive DFS approach, where we calculate the maximum path sum at each node by considering both its left and right children. We keep track of the global maximum path sum using a global variable.

This approach efficiently solves the problem with a time complexity of **O(n)** and a space complexity of **O(h)**, making it optimal for both balanced and unbalanced trees.
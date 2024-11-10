# 084_Binary_Tree_BFS_102. Binary Tree Level Order Traversal

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`



### Intuition and Main Idea

The level order traversal of a binary tree is essentially a breadth-first search (BFS) traversal. The main idea is to use a queue to help us visit each node level by level. We start by enqueuing the root node, then repeatedly dequeue a node, process it, and enqueue its children. This ensures that nodes are processed in the order of their levels.

### Solution 1: Using a Queue for BFS

We'll use a queue data structure to facilitate the level order traversal. Here's a step-by-step breakdown:

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
import java.util.*;
public class BinaryTreeLevelOrderTraversal {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Result list to store level order traversal
        List<List<Integer>> result = new ArrayList<>();
        // Base case: if the root is null, return an empty list since there are no nodes to traverse.
        if (root == null) {
            return result;
        }
        // Initialize a queue and add the root node to it. This queue will help us keep track of nodes to be processed at each level.
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // While there are nodes to process
        while (!queue.isEmpty()) {
            // List to store nodes at the current level
            List<Integer> currentLevel = new ArrayList<>();
            // Determine the number of nodes at the current level by checking the queue size.
            int levelSize = queue.size();
            // Process all nodes at the current level
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll(); // Dequeue the front node
                currentLevel.add(node.val); // Add its value to the current level list
                // Enqueue left subtree if it exists
                if (node.left != null) {
                    queue.add(node.left);
                }
                // Enqueue right subtree if it exists
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            // After processing all nodes at the current level, add the level's result list to the final result list.
            result.add(currentLevel);
        }
        // Once all levels are processed, return the final result list.
        return result;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. Each node is enqueued and dequeued exactly once.
- **Space Complexity**: O(N), which is the maximum number of nodes that can be stored in the queue at any given time. This occurs when the tree is a complete binary tree, and the last level is half full.

### Solution 2: Using Recursion

An alternative approach is to use recursion to achieve level order traversal. This involves using a helper function to traverse the tree and collect nodes at each level.

Here's the Java code for the recursive approach:

```java
import java.util.*;

public class BinaryTreeLevelOrderTraversalRecursive {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        levelOrderHelper(root, 0, result);
        return result;
    }
    // Define a recursive helper function that takes the current node, the current level, and the result list as arguments.
    private void levelOrderHelper(TreeNode node, int level, List<List<Integer>> result) {
        // Base case: if the node is null, do nothing
        if (node == null) {
            return;
        }
        // If the current level does not have a list, add a new list
        if (level >= result.size()) {
            result.add(new ArrayList<>());
        } 
        // Add the current node's value to its corresponding level list
        result.get(level).add(node.val);
        // Recursively call the helper function for the left and right children, incrementing the level by 1.
        levelOrderHelper(node.left, level + 1, result);
        levelOrderHelper(node.right, level + 1, result);
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. Each node is visited once.
- **Space Complexity**: O(N), due to the space used by the recursion stack and the result list. In the worst case, the recursion stack can go as deep as the height of the tree.

Both solutions effectively solve the problem, with the iterative approach using a queue being more intuitive for BFS, while the recursive approach provides an elegant and concise solution.


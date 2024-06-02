# 068_Binary_Tree_General_104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`



### Intuition and Main Idea

The maximum depth of a binary tree can be defined as the number of nodes along the longest path from the root node to the farthest leaf node. To determine this, we can use two main strategies:

1. **Recursive Depth-First Search (DFS)**: This involves exploring each node and its subtrees recursively, calculating the depth of each subtree, and then taking the maximum of these depths.

2. **Iterative Breadth-First Search (BFS)**: This approach uses a queue to traverse the tree level by level. The maximum depth is determined by the number of levels in the tree.

### Solution 1: Recursive Depth-First Search (DFS)

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
public class Solution {
    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        // Recursively calculate the depth of the left subtree
        int leftDepth = maxDepth(root.left);
        // Recursively calculate the depth of the right subtree
        int rightDepth = maxDepth(root.right);
        // The depth of the current node is the maximum of the depths of its subtrees plus one
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

### Solution 2: Iterative Breadth-First Search (BFS)

```java
import java.util.LinkedList;
import java.util.Queue;
public class Solution {
    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        // Initialize a queue to hold nodes at each level and traverse the tree level by level
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        // While there are nodes to process
        while (!queue.isEmpty()) {
            // Get the number of nodes at the current level
            int levelSize = queue.size();
            depth++;
            // Process each node at the current level
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                // Add the left child to the queue if it exists
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                // Add the right child to the queue if it exists
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
        }
        // Return the maximum depth
        return depth;
    }
}
```

### Alternative Solution: Using Stack for Iterative DFS

```java
import java.util.Stack;

public class Solution {

    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        
        // Initialize a stack to hold pairs of (node, current depth)
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, 1));
        int maxDepth = 0;
        
        // While there are nodes to process
        while (!stack.isEmpty()) {
            Pair<TreeNode, Integer> current = stack.pop();
            TreeNode currentNode = current.getKey();
            int currentDepth = current.getValue();
            
            // Update the maximum depth if needed
            maxDepth = Math.max(maxDepth, currentDepth);
            
            // Push the left child to the stack if it exists
            if (currentNode.left != null) {
                stack.push(new Pair<>(currentNode.left, currentDepth + 1));
            }
            
            // Push the right child to the stack if it exists
            if (currentNode.right != null) {
                stack.push(new Pair<>(currentNode.right, currentDepth + 1));
            }
        }
        
        // Return the maximum depth
        return maxDepth;
    }
}

// Utility class to hold pairs of (node, current depth)
class Pair<K, V> {
    private K key;
    private V value;
    
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    
    public K getKey() {
        return key;
    }
    
    public V getValue() {
        return value;
    }
}
```

### Time Complexity and Space Complexity

- **Recursive DFS**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(h) , where h is the height of the tree, due to the recursion stack. In the worst case (unbalanced tree), the height can be n ; in the best case (balanced tree), the height is log n .

- **Iterative BFS**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(n) , due to the queue that holds nodes at each level. In the worst case, the queue can hold up to half of the nodes at the last level (which is O(n) ).

- **Iterative DFS using Stack**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(h) , where h is the height of the tree, due to the stack. In the worst case (unbalanced tree), the height can be n ; in the best case (balanced tree), the height is log n .

### Conclusion

All three solutions effectively determine the maximum depth of a binary tree. The recursive DFS and iterative BFS methods are more commonly used, with iterative BFS being particularly suitable for balanced trees due to its level-order traversal. The iterative DFS using a stack is another robust option that avoids recursion, making it a viable alternative depending on the specific constraints and requirements of the problem.
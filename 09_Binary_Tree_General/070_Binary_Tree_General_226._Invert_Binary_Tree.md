# 070_Binary_Tree_General_226. Invert Binary Tree

Given the `root` of a binary tree, invert the tree, and return *its root*.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/invert2-tree-20240604213248310.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`



### Intuition and Main Idea

The main idea is to swap the left and right children of every node in the binary tree. This operation essentially mirrors the tree around its center.

We can solve this problem using both **recursive** and **iterative** approaches.

### Recursive Approach

The recursive approach involves a recursive depth-first search (DFS) where we recursively invert the left and right subtrees of each node.

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
    // Main method to invert a binary tree
    public TreeNode invertTree(TreeNode root) {
        // Base case: if the node is null, return null
        if (root == null) {
            return null;
        } 
        // Recursively invert the left subtree
        TreeNode left = invertTree(root.left);
        // Recursively invert the right subtree
        TreeNode right = invertTree(root.right);
        // Swap the left and right children
        root.left = right;
        root.right = left;
        // Return the current node as the new root of the inverted subtree
        return root;
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: O(n) , where n is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(h) , where h is the height of the tree. This space is used for the recursion stack. In the worst case, the height of the tree can be n (unbalanced tree), and in the best case, it can be log n (balanced tree).

### Iterative Approach

An iterative breadth-first search (BFS) approach can be implemented using a queue to perform a level-order traversal, where we invert nodes level by level.

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    // Main method to invert a binary tree
    public TreeNode invertTree(TreeNode root) {
        // If the root is null, return null
        if (root == null) {
            return null;
        }
        // Initialize a queue for level-order traversal
        Queue<TreeNode> queue = new LinkedList<>();
        // Add the root node to the queue
        queue.offer(root);
        // Process nodes while the queue is not empty
        while (!queue.isEmpty()) {
            // Remove the front node from the queue
            TreeNode current = queue.poll();
            // Swap the left and right children of the current node
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;
            // Add the left and right children to the queue if they are not null
            if (current.left != null) {
                queue.offer(current.left);
            }
            if (current.right != null) {
                queue.offer(current.right);
            }
        }
        // Return the root of the inverted tree
        return root;
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: O(n) , where n is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(n) , due to the queue storage. In the worst case, the queue could store half of the nodes at the last level of the tree, which is O(n) .

### Conclusion

Both the recursive and iterative approaches effectively invert a binary tree. The recursive approach is simpler and more intuitive but can lead to a deeper call stack for very large trees. The iterative approach, while slightly more complex, avoids deep recursion and can be more space-efficient in certain scenarios. Both approaches have the same time complexity, but their space complexities can differ based on the structure of the trees.
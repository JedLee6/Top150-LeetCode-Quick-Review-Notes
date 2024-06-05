# 071_Binary_Tree_General_101. Symmetric Tree

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).



**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/symtree1-20240604221602192.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100` 

**Follow up:** Could you solve it both recursively and iteratively?



### Intuition and Main Idea

The main idea is to check if the tree is a mirror of itself. This means that for each node, the left subtree should be a mirror image of the right subtree. We need to compare:
1. The left child of the left subtree with the right child of the right subtree.
2. The right child of the left subtree with the left child of the right subtree.

We can solve this problem using both **recursive** depth-first search (DFS) and **iterative** breadth-first search approaches.

### Recursive Approach

The recursive depth-first search approach involves a helper method that checks if two trees are mirrors of each other.

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
    // Main method to check if a tree is symmetric
    public boolean isSymmetric(TreeNode root) {
        // A tree is symmetric if it is empty or if its left and right subtrees are mirrors
        return root == null || isMirror(root.left, root.right);
    }
    // Helper method to check if two trees are mirrors of each other
    private boolean isMirror(TreeNode left, TreeNode right) {
        // If both nodes are null, they are mirrors of each other
        if (left == null && right == null) {
            return true;
        }
        // If one node is null and the other is not, they are not mirrors of each other
        if (left == null || right == null) {
            return false;
        }
        // If the values of two nodes are different, the tree is not symmetric
        if (left.val != right.val) {
            return false;
        }
        // Check if their respective subtrees are mirrors
        return isMirror(left.left, right.right) && isMirror(left.right, right.left);
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: \( O(n) \), where \( n \) is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: \( O(h) \), where \( h \) is the height of the tree. This space is used for the recursion stack. In the worst case, the height of the tree can be \( n \) (unbalanced tree), and in the best case, it can be \( log n \) (balanced tree).

### Iterative Approach

An iterative breadth-first search approach can be implemented using a queue to perform a level-order traversal, where we compare nodes level by level.

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    // Main method to check if a tree is symmetric
    public boolean isSymmetric(TreeNode root) {
        // If the root is null, the tree is symmetric
        if (root == null) {
            return true;
        }
        // Initialize a queue for level-order traversal
        Queue<TreeNode> queue = new LinkedList<>();
        // Add the left and right children of the root to the queue
        queue.offer(root.left);
        queue.offer(root.right);
        // Process nodes while the queue is not empty
        while (!queue.isEmpty()) {
            // Remove two nodes from the front of the queue
            TreeNode left = queue.poll();
            TreeNode right = queue.poll();
            // If both nodes are null, continue to the next pair of nodes
            if (left == null && right == null) {
                continue;
            }
            // If one node is null and the other is not, the tree is not symmetric
            if (left == null || right == null) {
                return false;
            }
            // If the values of the nodes are different, the tree is not symmetric
            if (left.val != right.val) {
                return false;
            }
            // Add the children of the nodes to the queue in a mirrored order
            queue.offer(left.left);
            queue.offer(right.right);
            queue.offer(left.right);
            queue.offer(right.left);
        }
        // If all nodes are processed without finding a mismatch, the tree is symmetric
        return true;
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: \( O(n) \), where \( n \) is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: \( O(n) \), due to the queue storage. In the worst case, the queue could store half of the nodes at the last level of the tree, which is \( O(n) \).

### Conclusion

Both the recursive and iterative approaches effectively determine if a binary tree is symmetric. The recursive approach is simpler and more intuitive but can lead to a deeper call stack for very large trees. The iterative approach, while slightly more complex, avoids deep recursion and can be more space-efficient in certain scenarios. Both approaches have the same time complexity, but their space complexities can differ based on the structure of the trees.
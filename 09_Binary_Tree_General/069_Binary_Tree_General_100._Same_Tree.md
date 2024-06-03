# 069_Binary_Tree_General_100. Same Tree


Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/ex1.jpg)

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/ex2-20240603214922262.jpg)

```
Input: p = [1,2], q = [1,null,2]
Output: false
```

**Example 3:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/ex3.jpg)

```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

 

**Constraints:**

- The number of nodes in both trees is in the range `[0, 100]`.
- `-104 <= Node.val <= 104`



### Intuition and Main Idea

The main idea is to compare the two binary trees node by node. We need to check:
1. If both nodes are null, they are considered the same.
2. If one node is null and the other is not, the trees are not the same.
3. If both nodes are not null, we compare their values.
4. Recursively check the left subtree and the right subtree.

We can solve this problem using both **recursive** and **iterative** approaches.

### Recursive Approach

The recursive approach involves a recursive depth-first search (DFS) where we compare each corresponding node of both trees.

Here is the recursive solution with detailed annotations:

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // If both nodes are null, they are the same
        if (p == null && q == null) {
            return true;
        }
        // If one node is null and the other is not, they are not the same
        if (p == null || q == null) {
            return false;
        }
        // If the values of the nodes are different, they are not the same
        if (p.val != q.val) {
            return false;
        }
        // Recursively check the left and right subtrees
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: O(n) , where n is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(h) , where h is the height of the tree. This space is used for the recursion stack. In the worst case, the height of the tree can be n (unbalanced tree), and in the best case, it can be log n (balanced tree).

### Iterative Approach

An iterative approach can be implemented using two queues to perform a level-order traversal, where we compare nodes level by level.

```java
import java.util.LinkedList;
import java.util.Queue;
public class Solution {
    // Main method to check if two binary trees are the same
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Initialize two queues for level-order traversal
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();
        // Add the root nodes of both trees to their respective queues
        queue1.offer(p);
        queue2.offer(q);
        // Process nodes while both queues are not empty
        while (!queue1.isEmpty() && !queue2.isEmpty()) {
            // Retrieve nodes from both queues at corresponding position
            TreeNode node1 = queue1.poll();
            TreeNode node2 = queue2.poll();
            // If both nodes are null, continue to the next pair of nodes
            if (node1 == null && node2 == null) {
                continue;
            }
            // If one node is null and the other is not, trees are not the same
            if (node1 == null || node2 == null) {
                return false;
            }
            // If the values of the nodes are different, trees are not the same
            if (node1.val != node2.val) {
                return false;
            }
            // If both nodes aren't null, add the left and right children of both nodes to their respective queues to prepare for their subsequent iteration
            queue1.offer(node1.left);
            queue1.offer(node1.right);
            queue2.offer(node2.left);
            queue2.offer(node2.right);
        }
        // If both queues are empty, trees are the same; otherwise, they are not
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```

### Time Complexity and Space Complexity

- **Time Complexity**: O(n) , where n is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(n) , due to the queue storage. In the worst case, the queue could store all the nodes at the last level of the tree, which is O(n) .

### Conclusion

Both the recursive and iterative approaches effectively determine if two binary trees are the same. The recursive approach is simpler and more intuitive but can lead to a deeper call stack for very large trees. The iterative approach, while slightly more complex, avoids deep recursion and can be more space-efficient in certain scenarios. Both approaches have the same time complexity, but their space complexities can differ based on the structure of the trees.
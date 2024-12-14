## 085_Binary_Tree_BFS_103. Binary Tree Zigzag Level Order Traversal

Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree1-20241208210454286.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
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
- `-100 <= Node.val <= 100`



### Problem Understanding:
We are given a **binary tree**, and we are required to traverse it in **zigzag level order**. This means we traverse each level of the tree alternately:
- **Level 1** (root): left to right
- **Level 2**: right to left
- **Level 3**: left to right
- and so on...

Our task is to return a **list of lists**, where each list contains the values of nodes at that level, traversed according to the zigzag pattern.

### Intuition and Approach:
To solve this problem, the approach involves:
1. **Breadth-First Search (BFS)**: Since we need to traverse the tree level by level, a level-order traversal (BFS) is ideal. Using a queue will help in processing nodes level by level.
2. **Zigzag Traversal**: At each level, we alternate the direction of traversal:
   - For odd levels, we process from left to right.
   - For even levels, we process from right to left.
3. **Queue with Two Directions**: A simple BFS can be adapted to handle the zigzag traversal by toggling the direction at each level.

### Approach 1: BFS with Level Tracking

1. We use a **queue** to perform BFS.
2. We keep track of the **current level**.
3. At each level, we decide whether to traverse left-to-right or right-to-left, using a flag.
4. We gather the nodes at each level and append them in the correct order.

### Code Solution (Approach 1):

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
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>(); // Initialize a list to store each level's list
        // Base case: if the root is null, return an empty list since there are no nodes to traverse.
        if (root == null) {
            return result;
        }
        // Initialize a queue and add the root node to it. This queue will help us keep track of nodes to be processed at each level.
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean leftToRight = true; // Flag to toggle the direction of traversal
        // implement a while loop to traverse each level
        while (!queue.isEmpty()) {
            //For each level, we first determine the number of nodes (`levelSize`) at that level.
            int levelSize = queue.size();
            List<Integer> level = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll(); // Poll a treeNode from the queue
                // Add the current node's value to the front or the end of list for this level according to the `leftToRight` flag
                if (leftToRight) {
                    level.add(currentNode.val);
                } else {
                    level.add(0, currentNode.val); // Insert at the beginning for right-to-left
                }
                // Add subtree to the queue
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            // Toggle the direction for the next level
            leftToRight = !leftToRight;
            // Add the current level's list to the result
            result.add(level);
        }
        return result;
    }
}
```

### Time Complexity:
- Each node in the binary tree is visited exactly once, and each operation (inserting a node's value or adding children to the queue) takes constant time.
- The time complexity is **O(n)**, where `n` is the number of nodes in the tree.

### Space Complexity:
- The space complexity is **O(n)** due to the queue that stores nodes at each level. In the worst case, the queue will contain all the nodes at the bottom level, which could be approximately `n / 2` nodes. Thus, the space complexity remains O(n).

---

### Approach 2: DFS with Level Tracking

Alternatively, we can solve this problem using **Depth-First Search (DFS)** by tracking the depth as we traverse the tree. This approach involves using a recursive DFS that maintains the current depth and adds nodes to the result based on whether the depth is odd or even.

1. **DFS Recursion**: We perform a DFS traversal where for each node, we check whether the depth is odd or even.
2. **Add Node Values**: Depending on whether the depth is odd or even, we insert the node value at the appropriate position (beginning or end of the list).
3. **Result Construction**: We construct the result list recursively, passing the current depth and alternating the insertion order.

### Code Solution (Approach 2):

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, 0, result);
        return result;
    }
    /**
     * The `dfs` method takes the current node, the current depth, and the result list as arguments.
     **/
    private void dfs(TreeNode node, int depth, List<List<Integer>> result) {
        if (node == null) {
            return;
        }
        // If the current depth level doesn't exist in the result, add it
        if (result.size() <= depth) {
            result.add(new ArrayList<>());
        }
        // Insert the node value in the appropriate order based on the depth
        if (depth % 2 == 0) {
            result.get(depth).add(node.val); // left to right
        } else {
            result.get(depth).add(0, node.val); // right to left
        }
        // Recursively process left and right subtree
        dfs(node.left, depth + 1, result);
        dfs(node.right, depth + 1, result);
    }
}
```

### Time Complexity:
- Similar to the BFS approach, we visit each node once, and each insertion operation takes constant time.
- The time complexity is **O(n)**, where `n` is the number of nodes in the tree.

### Space Complexity:
- The space complexity is **O(h)** due to the recursion stack, where `h` is the height of the tree. In the worst case (a skewed tree), `h` could be `n`, so the space complexity is O(n).

---

### Comparison and Conclusion:
1. **BFS (Approach 1)**:
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(n)
   - **Pros**: More intuitive for level-order traversal and easier to handle alternate direction.
   - **Cons**: Uses an explicit queue, which might require more space for wide trees.

2. **DFS (Approach 2)**:
   - **Time Complexity**: O(n)
   - **Space Complexity**: O(h), where `h` is the height of the tree.
   - **Pros**: Recursion-based, no explicit queue.
   - **Cons**: Recursion depth could cause issues with deep trees, and handling the zigzag pattern via recursion is slightly less intuitive.

In terms of practical implementation, the BFS approach (Approach 1) is often preferred because it is more straightforward for level-order traversal, and the queue naturally handles the alternating directions.
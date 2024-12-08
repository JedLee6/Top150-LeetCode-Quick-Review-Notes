# 083_Binary_Tree_BFS_637. Average of Levels in Binary Tree

Given the `root` of a binary tree, return *the average value of the nodes on each level in the form of an array*. Answers within 10^(-5) of the actual answer will be accepted.

> "Ten to the negative five" (10^-5) means 0.00001. It is a way to represent very small numbers in scientific notation. The notation "10^-5" indicates that 1 is divided by 10 raised to the power of 5, which results in a decimal point followed by four zeros and a 1 (0.00001). This expression is often used in scientific and mathematical contexts to denote precision or very small quantities.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/avg1-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/avg2-tree-20241109190440755.jpg)

```
Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`




### Problem: **Average of Levels in Binary Tree**

Given the root of a binary tree, we need to calculate the average value of the nodes on each level of the tree and return these averages in an array.

### Intuition and Main Idea

To solve this problem, we need to calculate the average for each level in the binary tree, which involves two main steps:
1. **Level Order Traversal**: Traverse each level of the binary tree. We can achieve this using a **Breadth-First Search (BFS)** approach, where we use a queue to process nodes level by level.
2. **Summing and Averaging Each Level**: At each level, sum up the values of the nodes and divide by the number of nodes in that level to get the average.

There are two main approaches to this problem:
1. **Breadth-First Search (BFS)**: Use a queue to perform level-order traversal.
2. **Depth-First Search (DFS)**: Use recursion to keep track of the sum and count of nodes at each level.

Let's implement both approaches and analyze their time and space complexities.

---

### Approach 1: Breadth-First Search (BFS)

Using a queue, we can process nodes level by level. For each level:
- Calculate the sum of the node values.
- Calculate the average by dividing the sum by the number of nodes in that level.

### Code Implementation (BFS):

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
    public List<Double> averageOfLevels(TreeNode root) {
        // List to store the average of each level
        List<Double> averages = new ArrayList<>();
        // Edge case: if the tree is empty, return an empty list
        if (root == null) {
            return averages;
        }
        // Queue for BFS traversal
        // A queue is used for level-order traversal. 
        Queue<TreeNode> queue = new LinkedList<>();
        // We start by adding the root node to the queue.
        // In Java's Queue interface, queue.add() and queue.offer() are both used to insert elements, but they differ in how they handle capacity limitations or failures to add an element. Queue.add(): Throws an exception on failure. Queue.offer(): Returns false on failure.
        queue.offer(root);
        // BFS traversal
        while (!queue.isEmpty()) {
            // At each level, we determine the number of nodes (`levelSize`).
            int levelSize = queue.size(); // Number of nodes at the current level
            double levelSum = 0; // Initialize the sum of the current level
            // Process each node in the current level
            for (int i = 0; i < levelSize; i++) {
                // For each node, we add its value to a cumulative `levelSum`.
                TreeNode currentNode = queue.poll();
                levelSum += currentNode.val; // Add the node's value to the level sum
                // We also enqueue the node’s left and right subtree for the next level.
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
            // After processing all nodes in the level, we calculate the average by dividing `levelSum` by `levelSize` and add it to `averages`.
            averages.add(levelSum / levelSize);
        }
        // Return the list of averages for each level
        return averages;
    }
}
```

### Time and Space Complexity (BFS):

- **Time Complexity**: O(N), where N is the number of nodes, because we visit each node exactly once.
- **Space Complexity**: O(M), where M is the maximum number of nodes at any level in the tree (i.e., the maximum width of the tree).

---

### Approach 2: Depth-First Search (DFS)

In this method, we use a recursive DFS approach. We maintain two lists:
- **Sum List**: To keep track of the cumulative sum of values at each level.
- **Count List**: To keep track of the number of nodes at each level.

Each time we visit a node, we update the sum and count for the respective level.

### Code Implementation (DFS):

```java
import java.util.*;
public class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        // We initialize two lists, `sums` and `counts`, to keep track of the cumulative sum and count of nodes at each level.
        List<Double> sums = new ArrayList<>();
        List<Integer> counts = new ArrayList<>();
        // Start DFS traversal, the `dfs` method takes the current node, its level, and the `sums` and `counts` lists.
        dfs(root, 0, sums, counts);
        // List to store the averages
        List<Double> averages = new ArrayList<>();
        // After all nodes are processed, we calculate the average for each level by dividing the sum by the count and store it in `averages`.
        for (int i = 0; i < sums.size(); i++) {
            averages.add(sums.get(i) / counts.get(i));
        }
        return averages;
    }

    private void dfs(TreeNode node, int level, List<Double> sums, List<Integer> counts) {
        // Base case: if the node is null, return
        if (node == null) {
            return;
        }
		// For each node: If we are visiting this level for the first time (i.e., `level == sums.size()`), we initialize new entries in `sums` and `counts`. 
        // If this is the first node at this level, add a new entry in sums and counts
        if (level == sums.size()) {
            sums.add((double) node.val);
            counts.add(1);
        } else {
            // But if we’ve already visited nodes at this level, we update the cumulative sum and count for the level.
            // Update the sum and count for the existing level
            sums.set(level, sums.get(level) + node.val);
            counts.set(level, counts.get(level) + 1);
        }
		// We recursively call `dfs` on the left and right subtree, passing `level + 1` to indicate that we are moving down to the next level.
        dfs(node.left, level + 1, sums, counts);
        dfs(node.right, level + 1, sums, counts);
    }
}
```

### Time and Space Complexity (DFS):

- **Time Complexity**: O(N), since we visit each node once.
- **Space Complexity**: O(H), where H is the height of the tree, due to the recursive call stack. In the worst case (skewed tree), it could be O(N).

---

### Approach Comparison

| Approach | Time Complexity | Space Complexity | Explanation                                    |
| -------- | --------------- | ---------------- | ---------------------------------------------- |
| **BFS**  | O(N)            | O(M)             | Processes nodes level by level using a queue   |
| **DFS**  | O(N)            | O(H)             | Uses recursion and level-based cumulative sums |

### Conclusion

Both BFS and DFS approaches effectively solve the problem of calculating the average value at each level in a binary tree:

- **BFS** is straightforward for level-order traversal problems and maintains a queue for each level.
- **DFS** is a recursive solution, useful when we want to keep cumulative sums and counts as we explore deeper levels. This approach is more efficient for balanced trees but could have higher space usage in skewed trees.

The choice between the two depends on preference and the nature of the tree.
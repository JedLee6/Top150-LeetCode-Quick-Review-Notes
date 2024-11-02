# 082_Binary_Tree_BFS_199. Binary Tree Right Side View

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree-20241013173605075.jpg)

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

**Example 2:**

```
Input: root = [1,null,3]
Output: [1,3]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`



### Problem: **Binary Tree Right Side View**

**Given** the root of a binary tree, imagine yourself standing on the right side of it. **Return** the values of the nodes you can see ordered from top to bottom.

### Intuition and Main Idea

When asked to return the right-side view of a binary tree, the goal is to return the last node visible in each level of the tree when viewed from the right. The simplest way to achieve this is to perform a **level-order traversal (BFS)**, where we process the tree level by level, but only add the last node from each level to the result. This is because the last node of each level is the one visible from the right side.

Another approach could be **depth-first search (DFS)**, where we prioritize exploring the right subtree first. If we explore the right subtree before the left, the first node we encounter at each depth is the rightmost node for that level.

> prioritize /praɪˈɒrɪˌtaɪz/ vt. If you prioritize something, you treat it as more important than other things

---

### **Approach 1: Level-Order Traversal (BFS)**

In this approach, we will:
1. Traverse the binary tree level by level.
2. For each level, we store the last node encountered (this is the node visible from the right).
3. Use a queue to maintain the nodes for BFS and iterate through each level.

### Code (BFS):

```java
import java.util.*;

public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        // Initialize result list to store right-side view nodes
        List<Integer> result = new ArrayList<>();
        // Edge case: If the tree is empty, return an empty list
        if (root == null) {
            return result;
        }
        // We use a queue (`LinkedList`) to perform a level-order traversal
        Queue<TreeNode> queue = new LinkedList<>();
        // We start by adding the root node to the queue
        queue.offer(root);
        // Start BFS traversal
        while (!queue.isEmpty()) {
            // In each iteration of the outer loop, we process all the nodes in the current level. The variable `levelSize` keeps track of how many nodes are in the current level.
            int levelSize = queue.size();
            // Iterate through all nodes in the current level
            for (int i = 0; i < levelSize; i++) {
                // The inner loop processes each node in the current level.
                TreeNode currentNode = queue.poll(); // Get the next node from the queue
                
                // If this is the last node in the current level, add it to the result list
                if (i == levelSize - 1) {
                    result.add(currentNode.val);
                }
                //For every node, if it has a left child, we enqueue it. Similarly, if it has a right child, we also enqueue it. Enqueue the left subtree first to obey the sequence of pre-order traversal (root->left->right), so we can process each level's nodes from left to right.
                // Add the left child to the queue for the next level
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                // Add the right child to the queue for the next level
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
        }
        // After processing all the levels, the `result` list will contain the rightmost node of each level.
        return result;
    }
}
```

### Time Complexity:

- **O(N)**: We traverse all the nodes in the tree exactly once.

### Space Complexity:
- **O(N)**: In the worst case (if the tree is skewed), the queue can hold up to `N` nodes.

---

### **Approach 2: Depth-First Search (DFS)**

In this approach, we perform a **pre-order traversal** (root-right-left) so that we prioritize visiting the right subtree before the left. As soon as we encounter a node at a new level for the first time, we add it to the result. This ensures that we capture the rightmost node at each level.

### Code (DFS):

```java
import java.util.*;

public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        // We initialize a list `result` to store the rightmost nodes of each level.
        List<Integer> result = new ArrayList<>();
        // The `dfs` helper function is used to perform a depth-first traversal. We call it starting from the root at depth `0`.
        dfs(root, result, 0);
        return result;
    }

    // Helper function for DFS traversal
    private void dfs(TreeNode node, List<Integer> result, int depth) {
        // In each call to `dfs`, we first check if the current node is `null`. If it is, we return immediately (base case).
        if (node == null) {
            return;
        }
		// If this is the first time visiting this depth (i.e., `depth == result.size()`), we add the current node to the result list.
        if (depth == result.size()) {
            result.add(node.val);
        }
        // We prioritize visiting the right subtree first by calling `dfs` method. This ensures that the first node we encounter at each depth is the rightmost node.
        // Recursion for the right subtree first to prioritize right-side nodes
        dfs(node.right, result, depth + 1);
        // Recursion for the left subtree to visit the remaining nodes at the same depth
        dfs(node.left, result, depth + 1);
        //After the DFS completes, `result` will contain the rightmost node of each level.
    }
}
```

### Time Complexity:
- **O(N)**: We visit each node once during the DFS traversal.

### Space Complexity:
- **O(H)**: The recursive call stack can go as deep as the height of the tree (`H`). In the worst case (skewed tree), this could be `O(N)`.

---

### **Approach 3: DFS with Backtracking**

We can further optimize DFS using **backtracking**, where we don't store the results during recursion but instead keep track of the maximum depth reached so far and update the result list accordingly.

### Code (DFS with Backtracking):

```java
import java.util.*;

public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        // List to store the right-side view of the tree
        List<Integer> result = new ArrayList<>();
        // Variable to track the maximum depth reached so far
        dfsWithBacktrack(root, result, 0, -1);
        return result;
    }

    // Helper function for DFS traversal with backtracking
    private int dfsWithBacktrack(TreeNode node, List<Integer> result, int depth, int maxDepth) {
        // Base case: if the current node is null, return the current max depth
        if (node == null) {
            return maxDepth;
        }

        // If visiting this level for the first time, add the current node to the result list
        if (depth > maxDepth) {
            result.add(node.val);
            maxDepth = depth; // Update the maximum depth reached
        }

        // Recur for the right subtree first to prioritize right-side nodes
        maxDepth = dfsWithBacktrack(node.right, result, depth + 1, maxDepth);
        // Recur for the left subtree to visit the remaining nodes at the same depth
        maxDepth = dfsWithBacktrack(node.left, result, depth + 1, maxDepth);

        return maxDepth;
    }
}
```

### Explanation:

- In this version, we maintain a `maxDepth` variable to keep track of the deepest level we've processed so far. Each time we visit a new depth, we add the node to the result list and update `maxDepth`.
- This approach is very similar to the previous DFS method but has a more explicit way of tracking the maximum depth, which may make it easier to understand during debugging.

### Time Complexity:
- **O(N)**: We visit each node once.

### Space Complexity:
- **O(H)**: The space required for the recursive call stack depends on the height of the tree.

---

### Conclusion:

1. **BFS**:
   - **Time Complexity**: O(N)
   - **Space Complexity**: O(N)
   - Best when you want a clear level-order traversal and focus on the rightmost node at each level.

2. **DFS**:
   - **Time Complexity**: O(N)
   - **Space Complexity**: O(H)
   - Good for depth-first exploration, especially when prioritizing the right subtree.

3. **DFS with Backtracking**:
   - **Time Complexity**: O(N)
   - **Space Complexity**: O(H)
   - Similar to DFS, but with explicit tracking of the depth and maximum depth reached.

All approaches are valid, and the choice depends on the situation and interviewer preference. The BFS approach may be more intuitive for level-order problems, while DFS offers a clean recursive solution.
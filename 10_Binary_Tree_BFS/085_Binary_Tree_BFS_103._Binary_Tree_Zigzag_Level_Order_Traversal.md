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




To solve LeetCode Problem 103, **Binary Tree Zigzag Level Order Traversal**, I’ll break down the solution step by step, explain my intuition, and provide different approaches with detailed code annotations. I will also analyze the time and space complexity for each approach.

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
import java.util.*;

public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        boolean leftToRight = true; // Flag to toggle the direction of traversal
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> level = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                // Add the current node's value to the list for this level
                if (leftToRight) {
                    level.add(currentNode.val);
                } else {
                    level.add(0, currentNode.val); // Insert at the beginning for right-to-left
                }
                
                // Add children to the queue
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

### Code Explanation:

1. **Queue Initialization**: We use a queue (`LinkedList`) to perform the level-order traversal. The root node is initially added to the queue.
2. **Direction Flag**: We use a boolean flag `leftToRight` to toggle between left-to-right and right-to-left traversal at each level.
3. **Level Traversal**: 
   - For each level, we first determine the number of nodes (`levelSize`) at that level.
   - We process each node in the queue, and depending on the value of `leftToRight`, we either append the node value to the end or insert it at the beginning of the `level` list.
4. **Queue Update**: As we process each node, we enqueue its left and right children (if they exist) for the next level.
5. **Toggle Direction**: After each level, we toggle the `leftToRight` flag to alternate the direction for the next level.
6. **Result**: The `result` list is updated after processing each level.

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
        
        // Recursively process left and right children
        dfs(node.left, depth + 1, result);
        dfs(node.right, depth + 1, result);
    }
}
```

### Code Explanation:

1. **DFS Function**: The `dfs` function takes the current node, the current depth, and the result list as arguments.
2. **Level Handling**: If the current depth exceeds the size of the result list, a new list is created for that depth.
3. **Alternating Insertion**: Based on the parity of the depth (`depth % 2`), the node's value is either added to the end of the list (left to right) or inserted at the beginning (right to left).
4. **Recursive Calls**: The function then recursively calls `dfs` on the left and right children, incrementing the depth.

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
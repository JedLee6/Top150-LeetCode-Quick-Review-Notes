# 077_Binary_Tree_General_129. Sum Root to Leaf Numbers

You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.

- For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return *the total sum of all root-to-leaf numbers*. Test cases are generated so that the answer will fit in a **32-bit** integer.

A **leaf** node is a node with no children.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/num1tree.jpg)

```
Input: root = [1,2,3]
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/num2tree-20240927204142611.jpg)

```
Input: root = [4,9,0,5,1]
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 9`
- The depth of the tree will not exceed `10`.



### **Intuition and Main Idea:**

- We need to traverse the tree to visit all root-to-leaf paths. And there are multiple ways to traverse a binary tree:
    - **DFS (Depth-First Search)**: We can perform a depth-first traversal (either recursively or iteratively using a stack).
    - **BFS (Breadth-First Search)**: We can also solve this problem using BFS by traversing the tree level by level, where each node stores the current path number.
- When we traverse from the root to a leaf, we compute the new number by multiplying the current number by 10 and adding the value of the current node.
- When we reach a leaf node, the number formed by the path from the root to that leaf is added to the total sum.

### **Solution 1: Recursive DFS Approach**

We will first explore the solution using a recursive DFS approach, where we traverse the tree, updating the number as we go deeper into the tree.

#### **Java Code for Recursive DFS Solution**:

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) {
        val = x;
    }
}

public class Solution {
    // Function to calculate the total sum of all root-to-leaf numbers
    public int sumNumbers(TreeNode root) {
        // Call the helper function with the initial sum (currentSum) as 0
        return dfs(root, 0);
    }
    // Helper function to perform DFS, which takes a `node` and the `currentSum` formed by the path from the root to this node
    private int dfs(TreeNode node, int currentSum) {
        // If the node is null, return 0, as there's no path to sum
        if (node == null) {
            return 0;
        }
        // Update the current sum by multiplying the previous sum by 10 and adding the node's value
        currentSum = currentSum * 10 + node.val;
        // If we reach a leaf node, return the current sum this is the path sum
        if (node.left == null && node.right == null) {
            return currentSum;
        }
        // Recursively calculate the sum for left and right subtrees and return the total sum
        return dfs(node.left, currentSum) + dfs(node.right, currentSum);
    }
}
```

### **Time Complexity and Space Complexity Analysis**:

- **Time Complexity**: 
  - The time complexity is **O(n)**, where **n** is the number of nodes in the tree. This is because we visit each node exactly once.
  
- **Space Complexity**:
  - The space complexity depends on the depth of the tree, which is **O(h)** where **h** is the height of the tree. In the worst case (skewed tree), the space complexity can be **O(n)**. In a balanced tree, it would be **O(log n)** due to recursion depth.

### **Solution 2: Iterative DFS Using a Stack**

In this approach, we use an explicit stack to simulate the DFS traversal. And each element in the stack contains both a reference to a node and the current path sum formed up to that node.

#### **Java Code for Iterative DFS Solution**:

```java
import java.util.Stack;
class Solution {
    // Main function to calculate the total sum of root-to-leaf numbers
    public int sumNumbers(TreeNode root) {
        if (root == null) return 0;  // If tree is empty, return 0  
        // Stack to simulate DFS, each element contains a pair (node, currentPathSum)
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, 0));
        int totalSum = 0;
        
        while (!stack.isEmpty()) {
            // For each iteration, pop a pair from the stack to check if its node is a leaf node and update the current path sum
            Pair<TreeNode, Integer> current = stack.pop();
            TreeNode node = current.getKey();
            int currentSum = current.getValue();
            // Update the current path sum
            currentSum = currentSum * 10 + node.val;
            // If the node is a leaf, add the current sum to the total sum
            if (node.left == null && node.right == null) {
                totalSum += currentSum;
            }
            // Push the right and left child into the stack if they exist
            // Push right subtree first so that left subtree is processed first in the stack (LIFO), which obeys the sequence of pre-order traversal (root->left->right)
            if (node.right != null) {
                stack.push(new Pair<>(node.right, currentSum));
            }
            if (node.left != null) {
                stack.push(new Pair<>(node.left, currentSum));
            }
        }
        return totalSum;  // Return the total sum of all root-to-leaf paths
    }
}
```

### **Time and Space Complexity**:

- **Time Complexity**: **O(n)** since we visit each node exactly once.
- **Space Complexity**: **O(n)** in the worst case due to the stack holding all nodes during traversal.

#### **Solution 3: BFS(Breadth-First Search) Using a Queue**

We can also solve this problem using BFS. The idea is to use a queue, where each element in the queue stores both a node and the current sum formed by the path from the root to that node. We process each level of the tree, updating the sum at each node, and when we reach a leaf node, we add the current sum to the total sum.

**Java Code for BFS solution**

```java
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    // Main function to calculate the total sum of root-to-leaf numbers
    public int sumNumbers(TreeNode root) {
        if (root == null) return 0;  // If tree is empty, return 0
        // Queue to store pairs of (node, currentSum)
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair<>(root, 0));
        int totalSum = 0;
        while (!queue.isEmpty()) {
            // Dequeue the front element
            Pair<TreeNode, Integer> current = queue.poll();
            // Process the current node
            TreeNode node = current.getKey();
            int currentSum = current.getValue();
            // Update the current sum
            currentSum = currentSum * 10 + node.val;
            // If the node is a leaf, add the current sum to the total sum
            if (node.left == null && node.right == null) {
                totalSum += currentSum;
            }
            // Enqueue the left and right subtreeif they exist
            // Enqueue the left subtree first to obey the sequence of pre-order traversal (root->left->right)
            if (node.left != null) {
                queue.offer(new Pair<>(node.left, currentSum));
            }
            if (node.right != null) {
                queue.offer(new Pair<>(node.right, currentSum));
            }
        }
        return totalSum;  // Return the total sum of all root-to-leaf paths
    }
}
```

### **Time and Space Complexity Analysis**:

- Time Complexity
    - **O(n)**, as we process each node exactly once.
- Space Complexity
    - **O(n)**, In a worst case, a balanced binary tree, we could store up to (n/2) nodes in the queue during the traversal.

> "n/2" is spelled as "n over two", which refers to https://en.wikipedia.org/wiki/Fraction.

### **Conclusion:**

Each approach has the same time complexity, but the space complexity differs based on how deep or wide the tree is. The best approach depends on the specific tree structure and whether recursion depth is a concern.

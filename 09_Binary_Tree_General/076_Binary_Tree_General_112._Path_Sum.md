

# 076_Binary_Tree_General_112. Path Sum

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/pathsum1-20240922223430319.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.
```

**Example 3:**

```
Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`



#### **Problem Understanding**

We are given the root of a binary tree and an integer `targetSum`. The task is to determine if there exists a **root-to-leaf** path in the tree where the sum of the node values is equal to `targetSum`. 

- A **leaf node** is defined as a node that has no children (both left and right child are `null`).
  
#### **Intuition and Approach**

The core idea is to perform a **tree traversal** (DFS or BFS) where we explore all possible root-to-leaf paths. For each path, we track the cumulative sum of node values as we traverse down the tree. Once we reach a leaf node, we check if the cumulative sum matches the target sum.

There are a few methods to approach this problem:

1. **Recursive Depth-First Search (DFS)**:
    - Traverse from the root to each leaf node recursively.
    - Keep track of the remaining `targetSum` as we move down the tree.
    - Subtract the current node’s value from `targetSum` at each step.
    - When we reach a leaf node, check if the remaining sum is zero.

2. **Iterative Depth-First Search (DFS) Using a Stack**:
    - This simulates the recursive approach using an explicit stack.
    - We store each node along with the remaining sum as we traverse the tree.

3. **Breadth-First Search (BFS) Using a Queue**:
    - Instead of going deep into the tree first, BFS explores level by level.
    - Use a queue to track nodes and their cumulative sums as we progress.

Let's first discuss the **recursive DFS approach** and follow it with alternative solutions.

---

### **Solution 1: Recursive Depth-First Search (DFS)**

**Approach:**

1. We recursively traverse the tree starting from the root.
2. At each node, subtract its value from `targetSum`.
3. If we reach a leaf node, check if the remaining sum is zero. If yes, return `true`.
4. If we haven't reached a leaf node, recursively check the left and right subtrees.

**Code Implementation:**

```java
// TreeNode definition (already provided in the problem)
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int val) {
        this.val = val;
    }
}
public class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // Base case: if the root is null, return false since there's no path
        if (root == null) return false;
        // If we reach a leaf node, check if the remaining targetSum equals the node's value
        if (root.left == null && root.right == null) {
            return targetSum == root.val;
        }
        // We subtract the current node's value from the targetSum and call `hasPathSum` recursively on the left and right subtrees
        int remainingSum = targetSum - root.val;
        // Use the logical OR (`||`) to check if either the left or right subtree contains a valid path.
        return hasPathSum(root.left, remainingSum) || hasPathSum(root.right, remainingSum);
    }
}
```

> In Java, the symbols `|` and `||` have distinct meanings and can be spelled out as follows:
>
> 1. **Single pipe (`|`)**: This is known as the **bitwise OR operator**.
>
>     - It performs a bitwise OR operation between two integer values.
>     - You can spell it as **"bitwise OR"**.
>
>     Example:
>
>     ```java
>     int result = 5 | 3;  // Bitwise OR
>     ```
>
> 2. **Double pipe (`||`)**: This is known as the **logical OR operator**.
>
>     - It is used to evaluate two boolean expressions. If either of the two expressions is `true`, the result is `true`.
>     - You can spell it as **"logical OR"**.
>
>     Example:
>
>     ```java
>     boolean result = (a > 5) || (b < 3);  // Logical OR
>     ```
>
> ### Summary:
>
> - **`|`** is spelled as **"bitwise OR"**.
> - **`||`** is spelled as **"logical OR"**.

#### **Time Complexity Analysis**:

- **Time Complexity**: `O(N)` where `N` is the number of nodes in the tree. We visit each node once during the traversal.
- **Space Complexity**: 
  - In the worst case, the recursion stack will have a depth of `O(H)`, where `H` is the height of the tree.
  - For a balanced tree, `H = O(log N)`. For a skewed tree, `H = O(N)`.

---

### **Solution 2: Iterative DFS Using a Stack**

In this approach, we use a stack to simulate the recursive DFS. For each node, we store the remaining target sum along with the node itself. Then we'll create a while loop to traverse all nodes. For each iteration of the while loop, we pop a node from the stack and check if it is a leaf. If we reach a leaf node, check if the currentSum equals 0. If so, we return true. Otherwize we push the not-null right subtree and not-null left subtree into the stack with updated sums. We break the while loop when the stack becomes empty.

**Code Implementation:**

```java
import java.util.Stack;
public class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        // Stack to store pairs of (current node, remaining sum) at that node
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, targetSum - root.val));
        while (!stack.isEmpty()) {
            // In the while loop, pop a node from the stack and check if it is a leaf
            Pair<TreeNode, Integer> current = stack.pop();
            TreeNode node = current.getKey();
            int currentSum = current.getValue();
            // If we reach a leaf node, check if the currentSum equals 0
            if (node.left == null && node.right == null && currentSum == 0) {
                return true;
            }
            // Push the right and left children (if exist) to the stack with updated sums
            if (node.right != null) {
                stack.push(new Pair<>(node.right, currentSum - node.right.val));
            }
            if (node.left != null) {
                stack.push(new Pair<>(node.left, currentSum - node.left.val));
            }
        }
        return false;
    }
}
```

#### **Time and Space Complexity**:

- **Time Complexity**: `O(N)` because we visit each node once.
- **Space Complexity**: `O(N)` for the stack in the worst case (skewed tree).

---

### **Solution 3: Breadth-First Search (BFS) Using a Queue**

In BFS, we use a queue to traverse the tree level by level. Similar to DFS, we maintain the current node and the remaining sum at each step. For each iteration, we check if reached a leaf node with sum 0, if so we return true. Otherwize, we enqueque the not-null right subtree and not-null left subtree with updated sums. And we break the loop when the queue becomes empty.

**Code Implementation:**

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.add(new Pair<>(root, targetSum - root.val));
        while (!queue.isEmpty()) {
            Pair<TreeNode, Integer> current = queue.poll();
            TreeNode node = current.getKey();
            int currentSum = current.getValue();
            // Check if we have reached a leaf with sum 0
            if (node.left == null && node.right == null && currentSum == 0) {
                return true;
            }
            // Add children to the queue with updated sums
            if (node.left != null) {
                queue.add(new Pair<>(node.left, currentSum - node.left.val));
            }
            if (node.right != null) {
                queue.add(new Pair<>(node.right, currentSum - node.right.val));
            }
        }
        return false;
    }
}
```

---

#### **Time and Space Complexity**:
- **Time Complexity**: `O(N)` as we traverse each node once.
- **Space Complexity**: `O(N)` for the queue in the worst case (skewed tree).

---

### **Summary**

- **Recursive DFS**: Simple and intuitive, uses recursion stack.
- **Iterative DFS**: Simulates recursion using an explicit stack.
- **BFS**: Explores the tree level by level using a queue.

All approaches have a time complexity of `O(N)` and vary in space complexity based on whether the tree is balanced or skewed.
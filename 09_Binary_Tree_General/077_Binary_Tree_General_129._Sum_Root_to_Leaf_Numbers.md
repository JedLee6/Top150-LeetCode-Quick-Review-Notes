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



### **Problem Understanding:**

We are given a binary tree where each root-to-leaf path represents a number formed by the digits at the nodes in that path. The task is to sum all such numbers formed by different root-to-leaf paths. For example, if the tree has paths `1 -> 2 -> 3`, this forms the number `123`, and similar numbers should be computed for all root-to-leaf paths. Finally, we return the sum of these numbers.

### **Intuition and Main Idea:**

1. **Recursive DFS (Depth-First Search):**
   - The core idea is to traverse the tree from the root to all leaf nodes.
   - At each node, we keep track of the number formed by the path from the root to that node.
   - If we reach a leaf node (a node with no children), the number formed by the path is added to the total sum.
   - We can achieve this efficiently with a DFS approach where, as we move from a parent node to its children, we propagate the current number downwards, multiplying the previous number by 10 and adding the current node's value.

2. **Iterative DFS (Using a Stack):**
   - Another approach is to simulate the DFS using a stack. We can store the node along with the current number formed by the path. This helps us avoid recursion and use an explicit stack.

### **Solution 1: Recursive DFS Approach**

#### **Step-by-Step Explanation:**

1. **Base Case**: If the node is `null`, return 0 because there's no number to form from a `null` node.
2. **Recursive Case**: If we are at a leaf node (both left and right children are `null`), return the number formed by the path from the root to this leaf node.
3. **Recursive Traversal**: For each node, we recursively compute the sum for the left and right subtrees, passing the current number formed so far.
4. **Sum Accumulation**: At each step, we multiply the current path number by 10 and add the current node's value, then pass it down to its children.

#### **Java Code for Recursive DFS Solution**:

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) {
        val = x;
    }
}

public class SumRootToLeaf {
    // Function to calculate the total sum of all root-to-leaf numbers
    public int sumNumbers(TreeNode root) {
        // Call the helper function with the initial sum (currentSum) as 0
        return dfs(root, 0);
    }
    // Helper function to perform DFS
    private int dfs(TreeNode node, int currentSum) {
        // If the node is null, return 0
        if (node == null) {
            return 0;
        }
        // Update the current sum by multiplying the previous sum by 10 and adding the node's value
        currentSum = currentSum * 10 + node.val;
        // If we reach a leaf node, return the current sum as this is a root-to-leaf number
        if (node.left == null && node.right == null) {
            return currentSum;
        }
        // Recursively calculate the sum for left and right subtrees and return the total sum
        return dfs(node.left, currentSum) + dfs(node.right, currentSum);
    }
}
```

### **Explanation of Code:**

- **Line 1-6**: The `TreeNode` class defines the structure of the binary tree nodes, where each node contains an integer value and references to left and right child nodes.
- **Line 9**: The `sumNumbers` function is the main function that initiates the DFS traversal.
- **Line 14**: The `dfs` function takes a `node` and the `currentSum` formed by the path from the root to this node.
- **Line 17**: If the node is `null`, return `0` (no path to sum).
- **Line 21**: Update the `currentSum` by shifting it by one decimal place (multiplying by 10) and adding the current node's value.
- **Line 24**: If the node is a leaf node, return the `currentSum` since this is a root-to-leaf number.
- **Line 28**: Recursively call the `dfs` function for both left and right subtrees and sum the results.

### **Time Complexity and Space Complexity Analysis**:

- **Time Complexity**: 
  - The time complexity is **O(n)**, where **n** is the number of nodes in the tree. This is because we visit each node exactly once.
  
- **Space Complexity**:
  - The space complexity depends on the depth of the tree, which is **O(h)** where **h** is the height of the tree. In the worst case (skewed tree), the space complexity can be **O(n)**. In a balanced tree, it would be **O(log n)** due to recursion depth.

### **Solution 2: Iterative DFS Using a Stack**

In this approach, we use an explicit stack to simulate the DFS traversal. Along with each node, we store the current sum formed by the path up to that node.

#### **Java Code for Iterative DFS Solution**:

```java
import java.util.Stack;

class Pair {
    TreeNode node;
    int currentSum;
    
    Pair(TreeNode node, int currentSum) {
        this.node = node;
        this.currentSum = currentSum;
    }
}

public class SumRootToLeafIterative {
    
    public int sumNumbers(TreeNode root) {
        if (root == null) return 0;
        
        // Stack to store pairs of (node, currentSum)
        Stack<Pair> stack = new Stack<>();
        stack.push(new Pair(root, 0));
        int totalSum = 0;
        
        while (!stack.isEmpty()) {
            Pair current = stack.pop();
            TreeNode node = current.node;
            int currentSum = current.currentSum;
            
            // Update the current sum by multiplying by 10 and adding the node's value
            currentSum = currentSum * 10 + node.val;
            
            // If it's a leaf node, add the current sum to the total sum
            if (node.left == null && node.right == null) {
                totalSum += currentSum;
            }
            
            // Push right and left children into the stack if they exist
            if (node.right != null) {
                stack.push(new Pair(node.right, currentSum));
            }
            if (node.left != null) {
                stack.push(new Pair(node.left, currentSum));
            }
        }
        
        return totalSum;
    }
}
```

### **Explanation of Code**:

- **Pair Class**: A helper class `Pair` is created to store a `TreeNode` and the `currentSum` formed along the path to this node.
- **Stack**: We use a stack to simulate DFS. Each element of the stack is a pair of a node and the `currentSum` formed up to that node.
- **Leaf Node Check**: Whenever we encounter a leaf node, we add the `currentSum` to the `totalSum`.

### **Time and Space Complexity**:

- **Time Complexity**: **O(n)** since we visit each node exactly once.
- **Space Complexity**: **O(n)** in the worst case due to the stack holding all nodes during traversal.

### **Conclusion:**

- Both the recursive DFS and the iterative DFS (using a stack) have the same time complexity of **O(n)**, but the iterative approach avoids recursion and may be preferred when recursion depth could be large.
- The recursive approach is elegant and easy to implement, while the iterative approach may be more suitable for avoiding potential stack overflow errors in deep trees.

# 080_Binary_Tree_General_222. Count Complete Tree Nodes

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2^h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

> `2^h` is spelled as `2 raised to the power of h`.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/complete.jpg)

```
Input: root = [1,2,3,4,5,6]
Output: 6
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [1]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5 * 104]`.
- `0 <= Node.val <= 5 * 104`
- The tree is guaranteed to be **complete**.



### Problem Understanding & Intuition:

First, we can easily use the Depth-First Search or Breadth-First Search method to solve the problem with O(n) time complexity. But we need to do it more efficiently than **O(n)**. A complete binary tree has specific properties that all levels except the last level may not be completely filled, and all nodes on the last level are as far left as possible. So we can leverage to improve the algorithm’s time complexity.

### Approach 1: **Recursive Solution**

Let's start with a recursive approach that utilizes the tree's height to calculate the number of nodes. The idea is to compute the height of the left and right subtrees:

- If the left and right heights are the same, the left subtree is a full tree, we can calculate its size directly: 2^h - 1 nodes, h is the height of the left subtree, and recursively count nodes in the right subtree.
- If the left and right heights are different, the right subtree is a full tree, we can calculate its size directly: 2^h - 1 nodes, h is the height of the left subtree, and recursively count nodes in the left subtree.

```java
// TreeNode class definition
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
public class Solution {
    // Helper function to calculate the height of the tree (number of node from root node to the leftmost leaf node)
    private int getHeight(TreeNode node) {
        int height = 0;
        while (node != null) {
            height++;
            node = node.left; // Move down the leftmost node to calculate height
        }
        return height;
    }
    // Main function to count nodes
    public int countNodes(TreeNode root) {
        // Base case: if the tree is empty, return 0
        if (root == null) return 0; 
        // Calculate the height of the leftmost paths from the left and right subtree 
        int leftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        // If the left and right heights are the same, the left subtree is a full tree
        if (leftHeight == rightHeight) {
            // Left subtree is full, we can calculate its size directly: 2^h - 1 nodes, h is the height of the left subtree
            // 2^h is computed by one left-shifted by leftHeight with bitwise left shift operator, a double open angle brackets
            // Plus 1 for the root node, and recursively count nodes in the right subtree, because we don't know if the right subtree is a full binary tree or not.
            return (1 << leftHeight) - 1 + 1 + countNodes(root.right);
        } else {
            // Otherwise, the right subtree is full and left subtree is not
            // Calculate nodes in the right subtree: 2^h - 1 nodes, h is the height of the right subtree
            // 2^h is computed by one left-shifted by rightHeight with bitwise left shift operator, a double open angle brackets
            // Plus 1 for the root node, and recursively count nodes in the left subtree
            return (1 << rightHeight) - 1 + 1 + countNodes(root.left);
        }
    }
}
```

### **Time Complexity Analysis**:

- **Time Complexity**: 
   - The height of the tree is **O(log n)**.
   - For each recursive call in each level, we compute the height, which takes another **O(log n)**, and make recursive calls for half of the tree.
   - Therefore, the overall time complexity is **O(log n^2)**, which is much better than a linear traversal **O(N)**.
- **Space Complexity**:
   - The space complexity is O(log n) due to the recursive call stack in a balanced binary tree.

### Approach 2: **Iterative Solution**

Here’s another version using an iterative approach, which avoids recursion.

```java
public class Solution {
    // Helper function to calculate the height of the tree (number of node from root node to the leftmost leaf node)
    private int getHeight(TreeNode node) {
        int height = 0;
        while (node != null) {
            height++;
            node = node.left; // Move down the leftmost node to calculate height
        }
        return height;
    }
    public int countNodes(TreeNode root) {
        // Base case: if the tree is empty, return 0
        if (root == null) return 0;
        int nodeCount = 0;
        TreeNode current = root;
        // We use a while loop to traverse the tree instead of recursion
        while (current != null) {
            // Calculate the height of the leftmost paths from the left and right subtree 
            int leftHeight = getHeight(current.left);
            int rightHeight = getHeight(current.right);
			// If the left and right heights are the same, the left subtree is a full tree
            if (leftHeight == rightHeight) {
                // Left subtree is full, we can calculate its size directly: 2^h - 1 nodes, h is the height of the left subtree, and plus 1 for the root node
                // 2^h is computed by one left-shifted by leftHeight with bitwise left shift operator, a double open angle brackets
                nodeCount += (1 << leftHeight);
                // Set current variable to the right subtree to compute its number of nodes in the next iteration
                current = current.right; 
            } else {
                // Otherwise, the right subtree is full and left subtree is not
            	// Calculate nodes in the right subtree: 2^h - 1 nodes, h is the height of the right subtree, and plus 1 for the root node
                // 2^h is computed by one left-shifted by rightHeight with bitwise left shift operator, a double open angle brackets
                nodeCount += (1 << rightHeight);
                // Set current variable to the right subtree to compute its number of nodes in the next iteration
                current = current.left;
            }
        }
        return nodeCount;
    }
}
```

### **Time Complexity**:
- **Time Complexity**: The height of the tree is **O(log n)**. For each recursive call in each level, we compute the height, which takes another **O(log n)**, and make recursive calls for half of the tree. Therefore, the overall time complexity is **O(log n^2)**
- **Space Complexity**: O(1), since we avoid the recursion stack.

### Summary:

- The key idea is to leverage the properties of a complete binary tree to avoid visiting every node.
- The recursive and iterative approaches both achieve a time complexity of O((log n)²) by dividing the problem in half at each step.



### How to spell and read `1 << rightHeight` in oral english

The expression `1 << rightHeight` in Java code is read aloud as **"one left-shifted by rightHeight"** or **"one shifted left by rightHeight"**. This expression uses the bitwise left shift operator `<<`, which shifts the bits of the number 1 to the left by the specified number of positions (`rightHeight` in this case).

### Why `1 << rightHeight` Equals \( 2^rightHeight\)

The left shift operator `<<` shifts the binary representation of the number to the left, effectively multiplying the number by powers of 2. Specifically:

- Shifting `1` to the left by `1` position (i.e., `1 << 1`) results in binary `10`, which is \( 2 \) in decimal.
- Shifting `1` to the left by `2` positions (i.e., `1 << 2`) results in binary `100`, which is \( 4 \) in decimal.

For each shift to the left, the result is multiplied by \( 2 \). Therefore:

1 << n = 1 * 2^n

### What's The Result of `x << y`

The result of `x << y` is equal to `( x * 2^y )`.

#### Explanation:

- The `<<` operator shifts the bits of `x` to the left by `y` positions. 
- Each left shift by 1 position doubles the value of the number, which is equivalent to multiplying by 2.
- Therefore, shifting `x` left by `y` positions is equivalent to multiplying `x` by \( 2^y \).

Example: If `x = 3` and `y = 2`, then: 3 << 2 = 3 * 2^2 = 3 * 4 = 12


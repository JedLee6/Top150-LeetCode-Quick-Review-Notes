# 075_Binary_Tree_General_114. Flatten Binary Tree to Linked List

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

 

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?



### Intuition and Main Idea

The main idea to solve this problem are 2 steps:

1. **Pre-order traversal** means that for every node, we first process the current node, then the left subtree, and finally the right subtree.
2. During the flattening process, the left subtree should be flattened and connected to the right of the current node, and the original right subtree should be moved to the end of this flattened left subtree.

### Detailed Solution:

We'll discuss three different approaches to solve this problem:

#### 1. Recursive Pre-order Traversal

We can recursively flatten the binary tree by:

1. Flattening the left and right subtrees.
2. Moving the flattened left subtree to the right of the current node and attaching the original right subtree at the end of the flattened left subtree.

This is a recursive approach, where each node will ensure that its subtree is flattened.

```java
// Define the TreeNode class as per the problem statement
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {
    // Helper function to perform the flatten operation recursively
    public void flatten(TreeNode root) {
        // Base case: If the root is null, there's nothing to flatten
        if (root == null) return;
        // Recursively flatten the left subtree, then the right subtree
        flatten(root.left);
        flatten(root.right);
        // After the recrusion, we'll store the original right subtree because we will overwrite root.right when moving the left subtree to the right
        TreeNode rightSubtree = root.right;
        // If there is a left subtree, we need to move it to the right
        if (root.left != null) {
        // Move the left subtree to the right
        root.right = root.left;
        root.left = null; // Set left to null as required
        // Find the rightmost node of the newly moved subtree so we can attach the original right subtree to it
        TreeNode current = root.right;
        while (current.right != null) {
            current = current.right;
        }
        // Attach the original right subtree to the rightmost node of the new right subtree
        current.right = rightSubtree;
        }
    }
}
```

- **Time complexity**: Each node is visited exactly once, and for each node, we traverse through its right subtree to find the rightmost node. In the worst case (a skewed tree), this will take O(N), where N is the number of nodes. Therefore, the overall time complexity is **O(N)**.
- **Space complexity**: The recursion depth can go up to the height of the tree. In the worst case, the height of the tree can be N (for a skewed tree), so the space complexity is **O(N)** due to the recursive call stack.

> skew /skjuː/ v. to move or lie at an angle, especially in a position that is not normal
>
> predecessor /ˈpredəsesər/  n The predecessor of an object or machine is the object or machine that came before it in a sequence or process of development.

#### 2. Iterative Pre-order Traversal with a Stack

An alternative approach would be to use an iterative method, leveraging a stack to simulate the pre-order traversal.

1. We perform a pre-order traversal using a stack.
2. We manually link each node as we would in a linked list way, setting the left child to `null` and connecting the right child.

```java
class Solution {
    public void flatten(TreeNode root) {
        // Edge case: If the tree is empty, just return
        if (root == null) return;
        // Stack for storing the nodes during traversal to simulate the pre-order traversal
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            // Pop the top node
            TreeNode currentNode = stack.pop();
            // Push the right and left children to the stack (right first, so it will be processed after the left child)
            if (currentNode.right != null) {
                stack.push(currentNode.right);
            }
            if (currentNode.left != null) {
                stack.push(currentNode.left);
            }
            // If the stack is not empty, the next node in pre-order traversal should be the top of the stack
            if (!stack.isEmpty()) {
                currentNode.right = stack.peek();
            }
            // Set the left child to null as per problem requirement
            currentNode.left = null;
        }
    }
}
```

- **Time complexity**: We visit each node exactly once, and every node is pushed and popped from the stack once. Therefore, the time complexity is **O(N)**.
- **Space complexity**: In the worst case, the stack will contain all the nodes in the tree. So, in the worst-case scenario (a completely unbalanced tree), the space complexity is **O(N)**.

#### 3. Morris Traversal (In-place)

The idea behind Morris Traversal is to modify the tree in such a way that we use the left subtree as a thread to traverse without using extra space for recursion or a stack.

```java
public class Solution {
    public void flatten(TreeNode root) {
        // We traverse the tree without extra space using a pointer current
        TreeNode current = root;
        while (current != null) {
            // For each node, if the left subtree exists, we find the rightmost node of the left subtree
            if (current.left != null) {
                // Find the rightmost node of the left subtree
                TreeNode predecessor = current.left;
                while (predecessor.right != null) {
                    predecessor = predecessor.right;
                }
                // Connect the original right subtree of the current node to the rightmost node of the left subtree
                predecessor.right = current.right;
                // Move the left subtree to the right and set the left child to null
                current.right = current.left;
                current.left = null;
            }
            // Move to the next node
            current = current.right;
        }
    }
}
```

- **Time complexity**: Each node is visited twice—once when finding the rightmost node and once during the main loop. Therefore, the time complexity is **O(N)**.
- **Space complexity**: This approach does not use a stack or recursion, so the space complexity is **O(1)**.

### Conclusion:

Each of the methods described above has its own advantages:

1. The recursive approach is simple and intuitive but uses additional space due to the call stack.
2. The iterative approach using a stack avoids recursion but still uses additional space proportional to the number of nodes.
3. The Morris Traversal method is the most space-efficient, using O(1) extra space, but it is a bit more complex.

When explaining these approaches, it's crucial to emphasize the trade-offs between time complexity and space complexity, and why we might choose one approach over the others based on the constraints of the problem.


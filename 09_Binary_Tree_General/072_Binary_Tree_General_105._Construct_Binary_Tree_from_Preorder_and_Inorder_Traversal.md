# 072_Binary_Tree_General_105. Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.



### Intuition and Main Idea

To solve the problem of constructing a binary tree from its preorder and inorder traversal arrays, we need to understand the properties of these traversals:

- **Preorder Traversal (Root, Left, Right)**:
  The first element of the preorder array is always the root of the tree. And all the elements of the left subtree are behind the root node, and all the elements of the right subtree are behind all the elements of the left subtree.

- **Inorder Traversal (Left, Root, Right)**:
  The elements to the left of the root in the inorder array represent the left subtree, and the elements to the right represent the right subtree.

Using these properties, we can recursively construct the tree by identifying the root from the preorder array and splitting the inorder array and preorder array into left and right subtrees.

### Detailed Step-by-Step Solution

1. **Identify the Root**:
   The root of the tree is the first element in the preorder array.

2. **Find the Root in Inorder Array**:
   Locate the root in the inorder array. The elements to the left of the root in the inorder array form the left subtree, and the elements to the right form the right subtree.

3. **Recursively Construct Subtrees**:
   Use the preorder and inorder arrays to recursively construct the left and right subtrees.

### Solution 1: Recursive Approach

```java
// Definition for a binary tree node.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {

    // Main method to construct the binary tree from preorder and inorder arrays
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // Call the helper method with initial indices
        return buildTreeHelper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    // Helper method to construct the binary tree recursively
    private TreeNode buildTreeHelper(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        // Base case: if there are no elements to construct the tree
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }

        // The first element in preorder array is the root node
        TreeNode root = new TreeNode(preorder[preStart]);

        // Find the index of the root in inorder array
        int inRootIndex = inStart;
        while (inRootIndex <= inEnd && inorder[inRootIndex] != root.val) {
            inRootIndex++;
        }

        // Number of elements in the left subtree
        int leftTreeSize = inRootIndex - inStart;

        // Recursively construct the left subtree
        root.left = buildTreeHelper(preorder, preStart + 1, preStart + leftTreeSize, inorder, inStart, inRootIndex - 1);

        // Recursively construct the right subtree
        root.right = buildTreeHelper(preorder, preStart + leftTreeSize + 1, preEnd, inorder, inRootIndex + 1, inEnd);

        // Return the root node
        return root;
    }
}
```

### Time Complexity
- Each element in the preorder array is processed once.
- Finding the root index in the inorder array takes O(n) time in the worst case.
- Therefore, the overall time complexity is O(n^2) in the worst case due to the repeated searching.

### Space Complexity
- The space complexity is O(n) due to the recursion stack used for tree construction.

### Solution 2: Optimized Recursive Approach with HashMap

To improve the performance of finding the root index in the inorder array, we can use a HashMap to store the indices of elements in the inorder array.

> subarray /'sʌbərei/

```java
public class Solution {
    // HashMap to store the indices of elements in inorder array, which can improve the performance of finding the root index in the inorder array
    private Map<Integer, Integer> inorderMap;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        inorderMap = new HashMap<>();
        // Fill the HashMap with the mapping rules from elements in inorder array to their indices
        for (int i = 0; i < inorder.length; i++) {
            inorderMap.put(inorder[i], i);
        }
        // Call the helper method with 6 input parameters, the first 3 parameters are preorder array with its start and end indices, and the last 3 parameters are inorder array with its start and end indices
        return buildTreeHelper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }
    private TreeNode buildTreeHelper(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        // Base case: if there are no elements to construct the tree
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }
        // The first element in preorder array is the root node, and nodes of left subtree and right subtree come after it
        TreeNode root = new TreeNode(preorder[preStart]);
        // Find the index of the root in inorder array using HashMap
        int inRootIndex = inorderMap.get(root.val);
        // Number of elements in the left subtree
        int leftTreeSize = inRootIndex - inStart;
        // Using these properties, we can split the inorder subarray and preorder subarray  into smaller left and right subtrees.
        // Recursively construct the left subtree, the 6 parameters are preorder and inorder pointer, with their corresponding start and end indices of the left subtree
        root.left = buildTreeHelper(preorder, preStart + 1, preStart + leftTreeSize, inorder, inStart, inRootIndex - 1);
        // Recursively construct the right subtree, the 6 parameters are preorder and inorder pointer, with their corresponding start and end indices of the right subtree
        root.right = buildTreeHelper(preorder, preStart + leftTreeSize + 1, preEnd, inorder, inRootIndex + 1, inEnd);
        // Return the root node
        return root;
    }
}
```

### Time Complexity
- Each element in the preorder array is processed once.
- Finding the root index in the inorder array takes O(1) time due to the HashMap.
- Therefore, the overall time complexity is O(n).

### Space Complexity

- The space complexity is O(n) due to the HashMap storing the indices and the recursion stack used for tree construction.

By using a HashMap, we optimize the search for the root index in the inorder array, thus reducing the time complexity from O(n^2) to O(n).



### Preorder, Inorder and Postorder traversals of a binary tree Recursively and Iteratively

#### Definition

**Preorder Traversal (Root, Left, Right)**:

The preorder traversal of a binary tree recursively visits the the root node first, then visits left subtree, followed by the right subtree.

1. Visit the root node.
2. Recursively visit the left subtree.
3. Recursively visit the right subtree.

The preorder traversal of a binary tree iteratively uses a stack to simulate the recursion.

1. Push the root node to the stack.
2. Pop the top of the stack, process the node, and push its right child (if it exists), followed by its left child (if it exists), so that left is processed first (LIFO)
3. Continue this process until the stack is empty.

**Inorder Traversal (Left, Root, Right)**:

The inorder traversal of a binary tree recursively visits the left subtree first, then visits the root node, followed by the right subtree.

1. Recursively visit the left subtree.
2. Visit the root node.
3. Recursively visit the right subtree.

The inorder traversal of a binary tree iteratively uses a stack to simulate the recursion.

1. Start from the root, and push all left children to the stack until we reach the leftmost node.
2. pop the top node from the stack, process the node, and move to its right child.
3. The loop continues until both the current node is null and the stack is empty.

**Postorder Traversal (Left, Right, Root)**:

The postorder traversal of a binary tree recursively visits the left subtree first, then visits the right subtree, followed by the root node.

1. Recursively visit the left subtree.
2. Recursively visit the right subtree.
3. Visit the root node.

The postorder traversal of a binary tree iteratively uses two stacks. One for performing a modified pre-order traversal(Root -> Right -> Left), and one for reversing the modified pre-order traversal.

1. Use stack1 to perform a modified pre-order traversal (Root -> Right -> Left), where we process the root first, followed by the right child, and then the left child.
2. Each time we pop a node from stack1, we push it onto stack2. This is reversing the modified pre-order traversal, so we get the nodes in post-order (left -> right -> root) when we pop from stack2.
3. After stack1 is empty, we pop all elements from stack2 to get the post-order traversal.

#### Real Example

Consider the following binary tree:

```
       1
      / \
     2   3
    / \   \
   4   5   6
```

- **Preorder Traversal**:
    - Start at the root: 1
    - Traverse the left subtree: 2, 4, 5
    - Traverse the right subtree: 3, 6
    - Preorder: [1, 2, 4, 5, 3, 6]
- **Inorder Traversal**:
    - Traverse the left subtree: 4, 2, 5
    - Visit the root: 1
    - Traverse the right subtree: 3, 6
    - Inorder: [4, 2, 5, 1, 3, 6]
- **Postorder Traversal**:
    - Traverse the left subtree: 4, 5, 2
    - Traverse the right subtree: 6, 3
    - Visit the root: 1
    - Postorder: [4, 5, 2, 6, 3, 1]

#### Java code implementations for performing Preorder, Inorder and Postorder traversals of a binary tree

```java
import java.util.Stack;

// Definition for a binary tree node. Defines the structure of a node in the binary tree, with integer value `val`, and references to left and right child nodes.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x;}
}

public class BinaryTreeTraversals {
    // Preorder traversal method visits the root node first, then recursively visits the left subtree, followed by the right subtree.
    public static void preorderTraversalRecursively(TreeNode root) {
        if (root == null) { return; }
        // Visit the root node
        System.out.print(root.val + " ");
        // Traverse the left subtree
        preorderTraversalRecursively(root.left);
        // Traverse the right subtree
        preorderTraversalRecursively(root.right);
    }

    public static void preorderTraversalIteratively(TreeNode root) {
        if (root == null) { return; }
        // Use a stack to simulate the recursion
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            // Pop the top of the stack, process the node, and push its right child (if it exists), followed by its left child (if it exists). Continue this process until the stack is empty.
            TreeNode current = stack.pop();
            System.out.print(current.val + " ");
            // Push right first so that left is processed first (LIFO)
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
    }
    // Inorder traversal method
    public static void inorderTraversalRecursively(TreeNode root) {
        if (root == null) { return; }
        // Traverse the left subtree
        inorderTraversalRecursively(root.left);
        // Visit the root node
        System.out.print(root.val + " ");
        // Traverse the right subtree
        inorderTraversalRecursively(root.right);
    }
    public static void inorderTraversalIteratively(TreeNode root) {
        // Use a stack to simulate recursion
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        // The loop continues until both the current node is null and the stack is empty
        while (current != null || !stack.isEmpty()) {
            // Push all the left children onto the stack
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            // When we reach the leftmost node, we pop the top node from the stack, process the node, and move to its right child
            current = stack.pop();
            System.out.print(current.val + " ");
            // Move to the right subtree
            current = current.right;
        }
    }
    // Postorder traversal method
    public static void postorderTraversalRecursively(TreeNode root) {
        if (root == null) { return; }
        // Traverse the left subtree
        postorderTraversalRecursively(root.left);
        // Traverse the right subtree
        postorderTraversalRecursively(root.right);
        // Visit the root node
        System.out.print(root.val + " ");
    }
    public static void postorderTraversalIteratively(TreeNode root) {
        if (root == null) { return; }
        // Use stack1 to perform a modified pre-order traversal (Root -> Right -> Left), where we process the root first, followed by the right child, and then the left child.
        Stack<TreeNode> stack1 = new Stack<>();
        // Each time we pop a node from stack1, we push it onto stack2. This is reversing the modified pre-order traversal, so we get the nodes in post-order (left -> right -> root) when we pop from stack2.
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);
        // Perform a modified pre-order traversal: Root -> Right -> Left
        while (!stack1.isEmpty()) {
            TreeNode current = stack1.pop();
            stack2.push(current); // Push the node to stack2
            // Push the left and right children to stack1
            if (current.left != null) {
                stack1.push(current.left);
            }
            if (current.right != null) {
                stack1.push(current.right);
            }
        }
        // After stack1 is empty, pop all the elements from stack2 to get the post-order
        while (!stack2.isEmpty()) {
            System.out.print(stack2.pop().val + " ");
        }
    }
    public static void main(String[] args) {
        // Constructing the example binary tree
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);
        // Preorder Traversal: 1 2 4 5 3 6
        System.out.print("Preorder Traversal Recursively: ");
        BinaryTreeTraversals.preorderTraversalRecursively(root);
        System.out.print("Preorder Traversal Iteratively: ");
        BinaryTreeTraversals.preorderTraversalIteratively(root);
        // Inorder Traversal: 4 2 5 1 3 6
        System.out.print("Inorder Traversal Recursively: ");
        BinaryTreeTraversals.inorderTraversalRecursively(root);
        System.out.print("Inorder Traversal Iteratively: ");
        BinaryTreeTraversals.inorderTraversalIteratively(root);
        // Postorder Traversal: 4 5 2 6 3 1
        System.out.print("Postorder Traversal Recursively: ");
        BinaryTreeTraversals.postorderTraversalRecursively(root);
        System.out.print("Postorder Traversal Iteratively: ");
        BinaryTreeTraversals.postorderTraversalIteratively(root);
    }
}
```

### Understanding Why a Binary Tree Cannot Be Reconstructed From Pre-order and Post-order Traversals  Alone

#### 1. **Pre-order and Post-order Traversals**

- **Pre-order traversal** visits the nodes in the order: **Root → Left → Right**.
- **Post-order traversal** visits the nodes in the order: **Left → Right → Root**.

When we have **pre-order** and **post-order** traversal results, the problem is that these two sequences do not provide enough information to **unambiguously determine the tree structure**, especially if the tree is not strictly binary (i.e., nodes can have either no children, one child, or two children).

Consider a simple example:

- A pre-order traversal of a tree gives: `A B C`
- A post-order traversal of the same tree gives: `B C A`

From these two traversals alone, you can't definitively say whether the tree has a structure like:

```
    A
   /
  B
   \
    C
```

or like:

```
    A
   /
  C
 /
B
```

The reason is that **both structures** would generate the same pre-order and post-order sequences. Thus, **pre-order and post-order traversals do not provide enough information to reconstruct the tree uniquely**.

#### 2. **Pre-order and In-order Traversals**

In contrast, when we combine **pre-order** and **in-order** traversal results, we can recover the tree unambiguously.

- **Pre-order traversal** always begins with the **root node**.
- **In-order traversal** allows us to determine the **structure of the left and right subtrees**.

For example, if the pre-order traversal is `A B D E C F` and the in-order traversal is `D B E A F C`, you can deduce the following:

- The root (`A`) comes first in pre-order.
- In the in-order sequence, everything to the left of `A` (i.e., `D B E`) is part of the left subtree, and everything to the right of `A` (i.e., `F C`) is part of the right subtree.
- Recursively applying this logic to the left and right subtrees helps reconstruct the entire tree uniquely.

#### 3. **Post-order and In-order Traversals**

Similarly, **post-order** and **in-order** traversals allow us to reconstruct the binary tree. Post-order gives the root last, and the in-order sequence helps in identifying the subtrees, making it possible to rebuild the tree in a unique way.
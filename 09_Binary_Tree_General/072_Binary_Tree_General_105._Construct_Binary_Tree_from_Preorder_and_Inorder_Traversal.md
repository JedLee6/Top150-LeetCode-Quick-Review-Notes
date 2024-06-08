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



### Preorder, Inorder and Postorder traversals of a binary tree

#### Definition

**Preorder Traversal (Root, Left, Right)**:

The preorder traversal of a binary tree recursively visits the the root node first, then visits left subtree, followed by the right subtree.

1. Visit the root node.
2. Recursively visit the left subtree.
3. Recursively visit the right subtree.

**Inorder Traversal (Left, Root, Right)**:

The inorder traversal of a binary tree recursively visits the left subtree first, then visits the root node, followed by the right subtree.

1. Recursively visit the left subtree.
2. Visit the root node.
3. Recursively visit the right subtree.

**Postorder Traversal (Left, Right, Root)**:

The postorder traversal of a binary tree recursively visits the left subtree first, then visits the right subtree, followed by the root node.

1. Recursively visit the left subtree.
2. Recursively visit the right subtree.
3. Visit the root node.

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
// Definition for a binary tree node. Defines the structure of a node in the binary tree, with integer value `val`, and references to left and right child nodes.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

public class BinaryTreeTraversals {
    // Preorder traversal method visits the root node first, then recursively visits
    // the left subtree, followed by the right subtree.
    public static void preorderTraversal(TreeNode root) {
        if (root == null) { return; }
        // Visit the root node
        System.out.print(root.val + " ");
        // Traverse the left subtree
        preorderTraversal(root.left);
        // Traverse the right subtree
        preorderTraversal(root.right);
    }
    // Inorder traversal method
    public static void inorderTraversal(TreeNode root) {
        if (root == null) { return; }
        // Traverse the left subtree
        inorderTraversal(root.left);
        // Visit the root node
        System.out.print(root.val + " ");
        // Traverse the right subtree
        inorderTraversal(root.right);
    }
    // Postorder traversal method
    public static void postorderTraversal(TreeNode root) {
        if (root == null) { return; }
        // Traverse the left subtree
        postorderTraversal(root.left);
        // Traverse the right subtree
        postorderTraversal(root.right);
        // Visit the root node
        System.out.print(root.val + " ");
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
        System.out.print("Preorder Traversal: ");
        BinaryTreeTraversals.preorderTraversal(root);
        // Inorder Traversal: 4 2 5 1 3 6
        System.out.print("Inorder Traversal: ");
        BinaryTreeTraversals.inorderTraversal(root);
        // Postorder Traversal: 4 5 2 6 3 1
        System.out.print("Postorder Traversal: ");
        BinaryTreeTraversals.postorderTraversal(root);
    }
}
```
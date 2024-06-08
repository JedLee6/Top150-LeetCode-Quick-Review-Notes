# 073_Binary_Tree_General_106. Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tree-20240608161519987.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.



### Intuition and Main Idea

To solve the problem of constructing a binary tree from given inorder and postorder traversal arrays, we need to understand the properties of these traversals:
- **Inorder Traversal** (Left, Root, Right): The inorder traversal of a binary tree recursively visits the left subtree first, then visits the root node, followed by the right subtree.
    - The elements to the left of the root in the inorder array represent the left subtree, and the elements to the right represent the right subtree.

- **Postorder Traversal** (Left, Right, Root): The postorder traversal of a binary tree recursively visits the left subtree first, then visits the right subtree, followed by the root node.
    - All the nodes from the left subtree are the first part of the postorde array, and followed by all the nodes from the right subtree, and elements of the left subtree, and the last node in the postorder array is the root node of the current subtree.


Using these properties, we can recursively construct the tree and its left and right subtrees using the corresponding parts of the inorder and postorder arrays.

### Approach 1: Recursive Approach with HashMap

1. Identify the root of the current subtree using the last element of the current postorder array.
2. Locate this root node in the inorder array. The elements to the left of this root in the inorder array belong to the left subtree, and the elements to the right belong to the right subtree.
3. Recursively construct the tree and its left and right subtrees using the corresponding parts of the inorder and postorder arrays.

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
public class Solution {
    // HashMap to store the indices of elements in inorder array, which can improve the performance of finding the root index in the inorder array
    private Map<Integer, Integer> inorderIndexMap = new HashMap<>();
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // Fill the HashMap with the mapping rules from elements in inorder array to their indices
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        // Call the helper method with 6 input parameters, the first 3 parameters are postorder array with its start and end indices, and the last 3 parameters are inorder array with its start and end indices
        return buildTreeHelper(postorder, 0, postorder.length - 1, 0, inorder.length - 1);
    }
    
    private TreeNode buildTreeHelper(int[] postorder, int postStart, int postEnd, int inStart, int inEnd) {
        // Base case: if there are no elements to construct the subtree
        if (postStart > postEnd || inStart > inEnd) {
            return null;
        }
        // The root node value of the current subtree is the last element in the current postorder subarray, so we create a new TreeNode with this value to build the tree
        TreeNode root = new TreeNode(postorder[postEnd]);
        // Get the index of the root node of the current subtree in the inorder array
        int rootIndex = inorderIndexMap.get(rootValue);
        // Calculate the size of the left subtree
        int leftSize = rootIndex - inStart;
        // By using the above properties, we can split the inorder subarray and postorder subarray into smaller left and right subtrees and continue the recursion.
        // Recursively construct the smaller left subtree, the 6 parameters are postorder and inorder pointer, with their corresponding start and end indices of the smaller left subtree
        root.left = buildTreeHelper(postorder, postStart, postStart + leftSize - 1, inStart, rootIndex - 1);
        // Recursively construct the smaller right subtree, the 6 parameters are postorder and inorder pointer, with their corresponding start and end indices of the smaller right subtree
        root.right = buildTreeHelper(postorder, postStart + leftSize, postEnd - 1, rootIndex + 1, inEnd);
        // Until the end of the recursion, we return the root node
        return root;
    }
}
```

### Time Complexity

The time complexity of this approach is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because we process each node once and perform constant-time operations (like looking up indices in the hashmap) for each node.

### Space Complexity

The space complexity is \(O(n)\) due to the additional space used by the hashmap to store the indices of the inorder array. The recursion stack also uses \(O(n)\) space in the worst case, resulting in a total space complexity of \(O(n)\).

### Approach 2: Iterative Approach

While the recursive approach is straightforward and easier to implement, an iterative approach can be considered to avoid potential stack overflow issues in very deep trees. However, the iterative approach for this specific problem is more complex and less intuitive compared to the recursive approach.

```java
public class Solution {
    private Map<Integer, Integer> inorderIndexMap = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder.length == 0 || postorder.length == 0) return null;
        // Fill the map with the indices of each value in the inorder array
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        // Initialize the root node with the last element of postorder array
        TreeNode root = new TreeNode(postorder[postorder.length - 1]);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        // Iterate over the postorder array in reverse (excluding the last element)
        for (int i = postorder.length - 2; i >= 0; i--) {
            TreeNode currentNode = new TreeNode(postorder[i]);
            TreeNode topNode = stack.peek();
            // If the index of the current node in inorder array is greater than the index of the top node
            // it means the current node is the right child of the top node
            if (inorderIndexMap.get(currentNode.val) > inorderIndexMap.get(topNode.val)) {
                topNode.right = currentNode;
            } else {
                TreeNode parent = null;
                // Find the parent of the current node by popping from the stack until we find a node whose index is less than the current node's index
                while (!stack.isEmpty() && inorderIndexMap.get(currentNode.val) < inorderIndexMap.get(stack.peek().val)) {
                    parent = stack.pop();
                }
                parent.left = currentNode;
            }
            stack.push(currentNode);
        }
        return root;
    }
}
```

### Time Complexity

The time complexity of this approach is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because we process each node once and perform constant-time operations for each node.

### Space Complexity

The space complexity is \(O(n)\) due to the additional space used by the hashmap to store the indices of the inorder array. The stack also uses \(O(n)\) space in the worst case, resulting in a total space complexity of \(O(n)\).

By providing both the recursive and iterative approaches, we ensure that we have a robust solution for constructing a binary tree from inorder and postorder traversal arrays.



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


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
    - All the nodes from the left subtree are the first part of the postorder array, and followed by all the nodes from the right subtree, and the last node in the postorder array is the root node of the binary tree.


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
        // Call the helper method with 5 input parameters, the first 3 parameters are postorder array pointer with its start and end indices, and the last 2 parameters are the start and end indices of the inorder array
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
        int rootIndex = inorderIndexMap.get(root.val);
        // Calculate the size of the left subtree
        int leftSize = rootIndex - inStart;
        // By using the above properties, we can split the inorder subarray and postorder subarray into smaller left and right subtrees and continue the recursion.
        // Recursively construct the smaller left subtree, the 5 parameters are postorder array pointer with the corresponding start and end indices of the inorder and postorder array
        // As we got the leftSize, so the index range in postorder of the smaller left subtree is from `postStart` to `postStart + leftSize - 1`. We also got the rootIndex in inorder, therefore the the index range in inorder of the smaller left subtree is from `inStart` to `rootIndex - 1`
        root.left = buildTreeHelper(postorder, postStart, postStart + leftSize - 1, inStart, rootIndex - 1);
        // Recursively construct the smaller right subtree, the 5 parameters are postorder array pointer with the corresponding start and end indices of the inorder and postorder array
        // According to the above information, the 5 parameters are writtern below
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





### Preorder, Inorder and Postorder traversals of a binary tree Recursively and Iteratively

**Preorder**, **Inorder**, and **Postorder** tree traversals are all forms of **Depth-First Search (DFS)**. They are different ways of visiting the nodes of a binary tree by exploring as deep as possible along each branch before backtracking, which is the core principle of DFS.

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

#### Java code implementations for performing Preorder, Inorder, Postorder traversals and Breadth-First Search of a binary tree

```java
import java.util.LinkedList;
import java.util.Queue;
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
            // Push right subtree first so that left subtree is processed first in the stack (LIFO), which obeys the sequence of pre-order traversal (root->left->right)
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
    public static void breadthFirstSearch(TreeNode root) {
        if (root == null) return;
        // A queue is used to keep track of nodes at each level. We enqueue the root node first and then, for every node we dequeue, we enqueue its left and right children (if they exist). The queue ensures that nodes are processed level by level.
        Queue<TreeNode> queue = new LinkedList<>();
        // Start at the root of the tree, and enqueue the root node
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode currentNode = queue.poll();  // Dequeue the front node
            System.out.print(currentNode.val + " ");  // Process the current node
            // Enqueue left child of the current node if it exists
            if (currentNode.left != null) {
                queue.add(currentNode.left);
            }
            // Enqueue right child of the current node if it exists
            if (currentNode.right != null) {
                queue.add(currentNode.right);
            }
        }
    }
    public static void main(String[] args) {
        // Constructing the example binary tree
        //       1
        //      / \
        //     2   3
        //    / \   \
        //   4   5   6
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
        // Breadth-First Search(BFS, Level-Order Traversal): 1 2 3 4 5 6
        System.out.print("Breadth-First Search(BFS, Level-Order Traversal): ");
        BinaryTreeTraversals.breadthFirstSearch(root);
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

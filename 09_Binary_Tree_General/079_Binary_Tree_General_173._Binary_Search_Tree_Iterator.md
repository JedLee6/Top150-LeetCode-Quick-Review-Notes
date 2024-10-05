# 079_Binary_Tree_General_173._Binary_Search_Tree_Iterator

Implement the `BSTIterator` class that represents an iterator over the **[in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR))** of a binary search tree (BST):

- `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
- `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

**
Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/bst-tree.png)

```
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 105]`.
- `0 <= Node.val <= 106`
- At most `105` calls will be made to `hasNext`, and `next`.

 **Follow up:**

- Could you implement `next()` and `hasNext()` to run in average `O(1)` time and use `O(h)` memory, where `h` is the height of the tree?



### **Intuition and Main Idea**

The problem asks us to implement an iterator for a Binary Search Tree (BST) that follows an **in-order traversal**. The key idea behind in-order traversal of a BST is that it visits the nodes in sorted (ascending) order. To solve this problem, the following steps can be followed:

1. **In-order Traversal**: In-order traversal visits nodes in the following order: left subtree, root, right subtree. For a BST, this guarantees that nodes are visited in increasing order.
   
2. **Iterator Design**: The iterator needs to provide two functionalities:
   - `next()`: Returns the next smallest element in the BST.
   - `hasNext()`: Checks if there is a next element in the traversal.

We want the `next()` method to be efficient, ideally `O(1)` or close to that, without having to compute the entire in-order traversal upfront every time `next()` is called.

### **Plan**

To achieve this, we can maintain a stack:
- The stack helps us simulate the in-order traversal iteratively. Instead of recursion, we push the leftmost nodes onto the stack, as these are the ones that should be visited first.
- When we call `next()`, we pop from the stack (which gives us the next smallest node) and, if that node has a right child, we repeat the process for its right subtree.

### **Approach 1: Stack-based Approach**

The solution uses a stack to keep track at most the next `"h"` (height of tree) elements for `"next()"` calls. The top node of the stack is the current minimum element. At every `"next()"` call, we need to refresh the stack by populating the stack with all leftmost nodes of the inputted node.

#### **Detailed Solution**

```java
// Define the TreeNode class
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) { val = x; }
}

// BSTIterator class
class BSTIterator {
    // Stack to store the nodes for in-order traversal
    private Stack<TreeNode> stack = new Stack<>();
    // Helper method to push all leftmost nodes of the inputted node to the stack, as these nodes are visited first in an in-order traversal
    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
    // Constructor that initializes the iterator with the root of the tree
    public BSTIterator(TreeNode root) {
        // Push all leftmost nodes of the BST to the stack
        pushLeft(root);
    }
    // hasNext() method to check if there is another node to be visited
    public boolean hasNext() {
        // If stack is not empty, we have a next node
        return !stack.isEmpty();
    }
    // next() method that returns the next smallest element in the BST
    public int next() {
        // Pop the node from the stack
        TreeNode node = stack.pop();
        // If the node has a right child, push all its leftmost children to the stack
        if (node.right != null) {
            pushLeft(node.right);
        }
        // Return the value of the node
        return node.val;
    }
}
/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

> inputted /ˈɪnpʊtɪd/ adj

### **Time Complexity Analysis**

- **next()**: The `next()` function involves popping a node from the stack, which takes `O(1)`. If the node has a right child, we call `pushLeft()` on that child, which involves traversing the leftmost nodes of the right subtree. In the worst case, we visit each node of the tree once. So, amortized, the `next()` operation takes **O(1)** time.
  
- **hasNext()**: This operation just checks if the stack is empty, so it is **O(1)**.

### **Space Complexity**

The space complexity depends on the height of the tree. In the worst case, the height of the tree could be `O(N)` (in case of a skewed tree), but in a balanced tree, the height would be `O(log N)`. Therefore, the space complexity is **O(h)**, where `h` is the height of the tree.

### **Approach 2: Flattening the BST (Alternative)**

Another approach could be to flatten the entire tree into a sorted list during initialization, and then iterate over this list.

Here’s how this alternative approach might look:

```java
class BSTIterator {
    private List<Integer> inorderList;
    private int index;

    public BSTIterator(TreeNode root) {
        inorderList = new ArrayList<>();
        index = 0;
        // Perform in-order traversal and store the result in the list
        inorderTraversal(root);
    }

    private void inorderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }
        // Traverse left subtree
        inorderTraversal(node.left);
        // Add node value to the list
        inorderList.add(node.val);
        // Traverse right subtree
        inorderTraversal(node.right);
    }

    public boolean hasNext() {
        // Check if the current index is within the bounds of the list
        return index < inorderList.size();
    }

    public int next() {
        // Return the current element and increment the index
        return inorderList.get(index++);
    }
}
```

However, this approach has some drawbacks:

1. **Space Complexity**: Storing the entire tree in a list would take **O(N)** space, where `N` is the number of nodes in the tree.

2. **Time Complexity**: Initializing the iterator would take **O(N)** time since we would perform an in-order traversal to create the list.

### **Comparison**

- **Time Complexity**:
  - **Initialization**: O(N) for the list approach (in-order traversal).
  - **next()**: O(1) for both approaches.
  - **hasNext()**: O(1) for both approaches.

- **Space Complexity**:
  - **Stack-based Approach**: O(h), where `h` is the height of the tree.
  - **List-based Approach**: O(N), where `N` is the number of nodes in the tree.

In summary, the **stack-based approach** is more efficient in terms of space complexity, especially for large, balanced trees. However, both solutions ensure an **O(1)** time complexity for `next()` and `hasNext()` operations.
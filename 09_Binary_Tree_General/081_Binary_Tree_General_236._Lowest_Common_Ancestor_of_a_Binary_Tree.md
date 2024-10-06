# 081_Binary_Tree_General_236. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

 

### Intuition and Main Idea

To solve the **Lowest Common Ancestor (LCA)** problem, we need to find the lowest node in the binary tree that has both given nodes `p` and `q` as descendants. This is a classic problem that can be approached using **tree traversal** techniques. 

The key insight here is that if we perform a **depth-first search (DFS)**, we can explore the tree from the root and look for the nodes `p` and `q`. Once both nodes are found in the left or right subtree, we can determine the lowest common ancestor by identifying the node where both subtrees diverge. This node will be the LCA, as it is the deepest point where both nodes are found.

The main challenge is to handle cases where:
- One node is a descendant of the other.
- Both nodes lie in different subtrees of the same ancestor.

### Step-by-Step Approach (Recursive Solution)

The idea is to use a **recursive DFS**. The function will return:
- **The node itself** if it matches either `p` or `q`.
- **Null** if neither node is found in the subtree.
- **LCA** if both `p` and `q` are found in the left and right subtrees of the current node.

#### Base Case:
- If the current node is `null`, return `null` (no LCA).
- If the current node is `p` or `q`, return the current node (it may be an ancestor).

#### Recursive Case:
- Recurse on both the left and right subtrees.
- If both left and right subtrees return non-null values, the current node is the LCA.
- If only one subtree returns a non-null value, propagate that value upwards (it might still be the LCA).

Let’s implement this approach step-by-step.

### Recursive Solution in Java

```java
// Definition for a binary tree node
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

public class Solution {
    // Main function to find the lowest common ancestor
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case: if the current node is null or matches either p or q
        if (root == null || root == p || root == q) {
            return root;  // Return the current node to the upper recursion if possible, as it might be the LCA
        }
        // Recurse on the left and right subtrees
        TreeNode left = lowestCommonAncestor(root.left, p, q);  // Find LCA in the left subtree
        TreeNode right = lowestCommonAncestor(root.right, p, q); // Find LCA in the right subtree
        // If both `left` and `right` are non-null, it means both `p` and `q` are found in different subtrees separately. Therefore, the current node (`root`) is the LCA.
        if (left != null && right != null) {
            return root; // Both p and q found in different subtrees, so root is the LCA
        }
        // If the left or right subtree recursive call gives a null value that means we haven’t found LCA in the this subtree, which means we found LCA on the another subtree. So we will return the non-null value from left or right.
        return left != null ? left : right;
    }
}
```

### Time Complexity:
- The time complexity is **O(N)**, where `N` is the number of nodes in the binary tree. This is because, in the worst case, we need to traverse all nodes in the tree to find the LCA.

### Space Complexity:
- The space complexity is **O(H)**, where `H` is the height of the tree. This is due to the recursive call stack. In the worst case, the height of the tree could be `N` (in a skewed tree), making the space complexity **O(N)**. In a balanced tree, the height would be **O(log N)**.

---

### Iterative Solution (Using Parent Pointers)

An alternative approach is to solve the problem **iteratively** by using a **hashmap to store parent pointers** during a DFS or BFS traversal. Once we store the parent of each node, we can trace the path from `p` and `q` back to the root and find the first common ancestor.

#### Approach:
1. Perform a DFS or BFS traversal to store the parent of each node in a hashmap.
2. Once we have the parent pointers, trace the ancestors of `p` and `q` upwards.
3. The first common node in both ancestor paths is the LCA.

### Iterative Solution in Java

```java
import java.util.*;
public class Solution {
    // Main function to find the lowest common ancestor using parent pointers
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Map to store parent pointers of each node, the key is the node, the value is the node's parent
        Map<TreeNode, TreeNode> parentMap = new HashMap<>();
        // Set to store ancestors of node p, because we'll check each ancestor of q with this set to find the common ancestor of q and p
        Set<TreeNode> ancestors = new HashSet<>();
        // Stack to perform DFS
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        parentMap.put(root, null); // Root has no parent
        // DFS to populate the parent map until both p and q are found
        while (!parentMap.containsKey(p) || !parentMap.containsKey(q)) {
            TreeNode node = stack.pop();
            // Add right subtree and its parent to the map
            // Push right subtree first so that left subtree is processed first in the stack (LIFO), which obeys the sequence of pre-order traversal (root->left->right)
            if (node.right != null) {
                parentMap.put(node.right, node);
                stack.push(node.right);
            }
            // Add left subtree and its parent to the map
            if (node.left != null) {
                parentMap.put(node.left, node);
                stack.push(node.left);
            }
        }
        // Add all ancestors of p to the set
        while (p != null) {
            ancestors.add(p);
            p = parentMap.get(p); // Move to the parent of p
        }
        // Check each ancestor of q upwards to find the common ancestor of q and p
        while (!ancestors.contains(q)) {
            q = parentMap.get(q); // Move to the parent of q
        }
        return q; // q is now the LCA
    }
}
```

### Time Complexity:
- The time complexity is **O(N)** because we traverse all nodes once to build the parent map.

### Space Complexity:
- The space complexity is **O(N)** because we store parent pointers for each node and use additional space for the stack and ancestor set.

---

### Conclusion:

- **Recursive Approach**: Simple, with a time complexity of **O(N)** and space complexity of **O(H)**.
- **Iterative Approach**: Also **O(N)** time complexity, but with a space complexity of **O(N)** due to the parent map.

Both approaches are valid, and the choice between them depends on the specific use case or interviewer preference.
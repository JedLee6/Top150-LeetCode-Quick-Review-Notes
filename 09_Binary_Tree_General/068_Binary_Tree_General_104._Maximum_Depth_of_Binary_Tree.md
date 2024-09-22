# 068_Binary_Tree_General_104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`



### Intuition and Main Idea

The maximum depth of a binary tree can be defined as the number of nodes along the longest path from the root node to the farthest leaf node. To determine this, we can use two main strategies:

1. **Recursive Depth-First Search (DFS)**: This involves exploring each node and its subtrees recursively, calculating the depth of each subtree, and then taking the maximum of these depths.

2. **Iterative Breadth-First Search (BFS)**: This approach uses a queue to traverse the tree level by level. The maximum depth is determined by the number of levels in the tree.

### Solution 1: Recursive Depth-First Search (DFS)

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
    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        // Recursively calculate the depth of the left subtree
        int leftDepth = maxDepth(root.left);
        // Recursively calculate the depth of the right subtree
        int rightDepth = maxDepth(root.right);
        // The depth of the current node is the maximum of the depths of its subtrees plus one
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

### Solution 2: Iterative Breadth-First Search (BFS)

```java
import java.util.LinkedList;
import java.util.Queue;
public class Solution {
    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        // Initialize a queue to hold nodes at each level and traverse the tree level by level
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        // While there are nodes to process
        while (!queue.isEmpty()) {
            // Get the number of nodes at the current level
            int levelSize = queue.size();
            depth++;
            // Process each node at the current level
            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                // Add the left child to the queue if it exists
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                // Add the right child to the queue if it exists
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }
        }
        // Return the maximum depth
        return depth;
    }
}
```

### Alternative Solution: Using Stack for Iterative DFS

```java
import java.util.Stack;

public class Solution {

    // Main method to calculate the maximum depth of a binary tree
    public int maxDepth(TreeNode root) {
        // Base case: if the root is null, the depth is 0
        if (root == null) {
            return 0;
        }
        
        // Initialize a stack to hold pairs of (node, current depth)
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, 1));
        int maxDepth = 0;
        
        // While there are nodes to process
        while (!stack.isEmpty()) {
            Pair<TreeNode, Integer> current = stack.pop();
            TreeNode currentNode = current.getKey();
            int currentDepth = current.getValue();
            
            // Update the maximum depth if needed
            maxDepth = Math.max(maxDepth, currentDepth);
            
            // Push the left child to the stack if it exists
            if (currentNode.left != null) {
                stack.push(new Pair<>(currentNode.left, currentDepth + 1));
            }
            
            // Push the right child to the stack if it exists
            if (currentNode.right != null) {
                stack.push(new Pair<>(currentNode.right, currentDepth + 1));
            }
        }
        
        // Return the maximum depth
        return maxDepth;
    }
}

// Utility class to hold pairs of (node, current depth)
class Pair<K, V> {
    private K key;
    private V value;
    
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    
    public K getKey() {
        return key;
    }
    
    public V getValue() {
        return value;
    }
}
```

### Time Complexity and Space Complexity

- **Recursive DFS**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(h) , where h is the height of the tree, due to the recursion stack. In the worst case (unbalanced tree), the height can be n ; in the best case (balanced tree), the height is log n .

- **Iterative BFS**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(n) , due to the queue that holds nodes at each level. In the worst case, the queue can hold up to half of the nodes at the last level (which is O(n) ).

- **Iterative DFS using Stack**:
  - **Time Complexity**: O(n) , where n is the number of nodes in the tree, because each node is visited once.
  - **Space Complexity**: O(h) , where h is the height of the tree, due to the stack. In the worst case (unbalanced tree), the height can be n ; in the best case (balanced tree), the height is log n .

### Conclusion

All three solutions effectively determine the maximum depth of a binary tree. The recursive DFS and iterative BFS methods are more commonly used, with iterative BFS being particularly suitable for balanced trees due to its level-order traversal. The iterative DFS using a stack is another robust option that avoids recursion, making it a viable alternative depending on the specific constraints and requirements of the problem.



### Breadth-First Search (BFS) in Binary Tree

**Breadth-First Search (BFS)** in a binary tree has a **single standard approach** for traversing the tree: **level-order traversal**. This means BFS visits each node level by level, starting from the root and moving down to the leaves. Unlike **DFS** (which includes **Preorder**, **Inorder**, and **Postorder** traversals), **BFS** does not have different variations in terms of traversal order for binary trees.

### **BFS Algorithm Code for Binary Tree (Level-Order Traversal)**

The most common implementation of BFS in a binary tree uses a **queue** to explore each level one at a time. Here's how the algorithm works:

1. **Start** at the root of the tree.
2. **Enqueue** the root node.
3. While the queue is not empty:
    - **Dequeue** a node.
    - **Process** the node (e.g., print or collect its value).
    - **Enqueue** its left and right children (if they exist).
4. Repeat until all nodes are processed.

#### **BFS (Level-Order Traversal) Java Implementation:**

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
        // Breadth-First Search(BFS, Level-Order Traversal): 1 2 3 4 5 6
        System.out.print("Breadth-First Search(BFS, Level-Order Traversal): ");
        BinaryTreeTraversals.breadthFirstSearch(root);
    }
}
```

### **Explanation of BFS Code**:

- **Queue**: A queue is used to keep track of nodes at each level. We enqueue the root node first and then, for every node we dequeue, we enqueue its left and right children (if they exist). The queue ensures that nodes are processed level by level.
- **Time Complexity**: The time complexity of BFS is **O(n)**, where **n** is the number of nodes in the tree. This is because every node is processed once.
- **Space Complexity**: The space complexity is **O(w)**, where **w** is the maximum width of the tree (the maximum number of nodes at any level). In the worst case (for example, in a full binary tree), the queue might need to store half of the nodes at the last level, so the space complexity can be **O(n/2) = O(n)**.

### **Does BFS Have Variations Like Preorder, Inorder, and Postorder?**

No, BFS (in the context of a binary tree) does not have different variations like DFS does with **Preorder**, **Inorder**, and **Postorder**. BFS is always **level-order traversal**, where nodes are visited level by level, starting from the root. The traversal order is determined by the structure of the tree, and there is no recursive variation like in DFS.



### DFS(Depth-First Search) and BFS(Breadth-First Search) for Binary Trees: Overview, Use Cases, and Comparisons

#### **1. What are DFS and BFS?**

- **DFS (Depth-First Search)** and **BFS (Breadth-First Search)** are two fundamental algorithms used for traversing or searching through data structures such as **trees** and **graphs**.
    - **DFS** explores a tree or graph **by going as deep as possible** along each branch before backtracking. In the case of a binary tree, DFS visits each node by first exploring one subtree entirely before moving to another subtree.

    - **BFS** explores the tree or graph level by level, visiting all nodes at the present depth before moving on to nodes at the next depth level.

---

#### **2. What is DFS and BFS used for?**

DFS and BFS are foundational algorithms used in various computer science problems, particularly in **searching**, **traversal**, and **solving optimization problems** in trees and graphs. They are instrumental in solving problems that require visiting or processing nodes in a specific order.

**Preorder**, **Inorder**, and **Postorder** tree traversals are all forms of **Depth-First Search (DFS)**. They are different ways of visiting the nodes of a binary tree by exploring as deep as possible along each branch before backtracking, which is the core principle of DFS.

**Breadth-First Search (BFS)** in a binary tree has a **single standard approach** for traversing the tree: **level-order traversal**. This means BFS visits each node level by level, starting from the root and moving down to the leaves. Unlike **DFS** (which includes **Preorder**, **Inorder**, and **Postorder** traversals), **BFS** does not have different variations in terms of traversal order for binary trees.

- **DFS Use Cases**:
    - **Pathfinding in mazes**: DFS is used in exploring all possible paths until the correct path is found.
    - **Detecting cycles**: DFS can be used in graphs to detect cycles by keeping track of visited nodes.
    - **Topological sorting**: DFS is used to order tasks or jobs (especially in DAGs - Directed Acyclic Graphs) based on dependencies.
    - **Tree traversal (Preorder, Inorder, Postorder)**: DFS forms the backbone of recursive tree traversal methods.

- **BFS Use Cases**:
    - **Shortest path in an unweighted graph**: BFS is used to find the shortest path between two nodes, as it processes all nodes level by level.
    - **Finding connected components**: BFS can help identify all nodes that are part of the same component in a graph.
    - **Solving puzzles like sliding puzzles or finding the shortest path in a grid**: BFS explores all possible moves in the shortest number of steps, which makes it suitable for shortest-path problems.

---

#### **3. Example of DFS and BFS in Binary Tree in Java**

**DFS Implementation (Preorder Traversal)**:

```java
// TreeNode definition
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
public class BinaryTreeDFS {
    // DFS using recursion (preorder traversal)
    public void dfs(TreeNode node) {
        if (node == null) return;
        System.out.print(node.val + " "); // process the node
        dfs(node.left);  // recursively process the left subtree
        dfs(node.right); // recursively process the right subtree
    }
}
```

**BFS Implementation (Level-Order Traversal)**:

```java
import java.util.LinkedList;
import java.util.Queue;

public class BinaryTreeBFS {
    // BFS using a queue (level-order traversal)
    public void bfs(TreeNode root) {
        if (root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll(); // get and remove the front node
            System.out.print(node.val + " "); // process the node
            if (node.left != null) queue.add(node.left);  // enqueue left child
            if (node.right != null) queue.add(node.right); // enqueue right child
        }
    }
}
```

---

#### **4. When Should or Shouldn't DFS or BFS be Used?**

- **DFS** is better suited for problems where:
    - You need to **explore all possible paths** (e.g., finding paths in a maze).
    - You're dealing with **deep trees** or **graphs** with fewer nodes in a single branch (long chains of nodes).
    - **Memory usage** is a concern, as DFS can operate with less memory (space complexity of `O(h)`, where `h` is the height of the tree).

- **BFS** is better suited for problems where:
    - You need to find the **shortest path** in an unweighted tree or graph.
    - You’re dealing with shallow but **wide trees** or graphs, where each level may have many nodes.
    - **Memory isn't a constraint**. BFS requires more memory (space complexity of `O(w)`, where `w` is the maximum width of the tree).

#### **When to Avoid DFS:**

- **Extremely deep trees/graphs**: DFS might lead to **stack overflow** errors in recursive implementations when the tree is highly imbalanced.
- **Shortest path problems**: DFS is not ideal for finding the shortest path in an unweighted graph because it might go deep into a suboptimal path before exploring others.

#### **When to Avoid BFS:**

- **Memory constraints**: BFS uses more memory due to storing all nodes at a particular level in the queue. In large graphs, this can be inefficient.

---

#### **5. Advantages and Disadvantages**

| **Algorithm** | **Advantages**                                               | **Disadvantages**                                            |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **DFS**       | - Uses less memory than BFS.<br>- Simple recursive implementation. | - May not find the shortest path.<br>- Can get stuck in deep trees. |
| **BFS**       | - Guarantees shortest path in unweighted graphs.<br>- Finds all solutions in the shortest steps. | - Requires more memory.<br>- Performance decreases with wide trees. |

---

#### **6. Related or Similar Algorithms**

- **A-star Algorithm**:
    - **Similarities**: Both BFS and A-star can be used for pathfinding, and A-star also explores nodes systematically.
    - **Differences**: A-star uses a heuristic function to prioritize nodes, which helps in finding the shortest path more efficiently than BFS in weighted graphs.
    - **Advantages of A-star**: More efficient in finding the optimal solution in weighted graphs due to its heuristic-based approach.
    - **Disadvantages of A-star**: Requires a good heuristic function to perform well.

- **Dijkstra’s Algorithm**:
    - **Similarities**: Like BFS, Dijkstra’s finds the shortest path. However, it works for **weighted graphs**.
    - **Differences**: BFS only works for unweighted graphs, whereas Dijkstra’s works for weighted graphs.
    - **Advantages of Dijkstra's**: Can find the shortest path in graphs with varying weights.
    - **Disadvantages of Dijkstra's**: It has a higher time complexity than BFS due to the need to manage priority queues.

> heuristic /hjuˈrɪstɪk/ adj. computer program uses rules based on previous experience in order to solve a problem, rather than using a mathematical procedure
>
> Dijkstra /ˈdaɪkstrə/ Dijkstra's algorithm (/ˈdaɪkstrəz/ DYKE-strəz) is an algorithm for finding the shortest paths between nodes in a weighted graph, which may represent, for example, road networks. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later.

---

#### **7. Industry-Standard Solutions and Opinions**

- **DFS** is often preferred in situations where **memory is limited** and the structure is known to be deep but not overly wide. For example, it's common in applications like solving **sudoku**, **puzzles**, or when searching for paths in **mazes**.

- **BFS** is preferred when the goal is to find the **shortest path**, especially in **unweighted graphs** or trees, such as in **social network connections** or **network routing algorithms**.

---

#### **8. Official Guidelines and Industry Discussions**

- According to discussions on **StackOverflow**, BFS is generally the choice for **shortest path** problems in unweighted graphs, as it guarantees that the first time it finds the solution, it is the optimal one. Reference: [StackOverflow discussion on BFS vs DFS](https://stackoverflow.com/questions/33353159/when-should-i-use-bfs-vs-dfs).

- In the case of **A-star** and **Dijkstra's Algorithm**, it is widely accepted in industry that BFS is inefficient for **weighted graphs** where **Dijkstra’s Algorithm** or **A\*** are more suitable, especially in complex applications like **Google Maps** or **GPS-based pathfinding** systems.
    - Reference: [Wikipedia on Dijkstra’s Algorithm](https://en.wikipedia.org/wiki/Dijkstra's_algorithm).

- The **Big O Cheat Sheet** (widely used in interviews and companies like **Google** and **Facebook**) highlights the different space and time complexities for DFS and BFS, emphasizing that **BFS is more memory-intensive** for wide trees, whereas **DFS can cause stack overflow** in deeply recursive trees.
    - Reference: [Big O Cheat Sheet](https://www.bigocheatsheet.com/).

---

### **Conclusion**

- **DFS** is preferred in deep but narrow trees where memory efficiency is key.
- **BFS** is the go-to for finding the shortest path in wide but shallow graphs/trees.
- In complex pathfinding problems, **A-star** and **Dijkstra’s** are more suitable due to their heuristic-based optimizations.

The choice between DFS and BFS should be informed by the specific problem constraints, such as memory usage, optimality of the solution, and the structure of the data (e.g., tree depth vs. width).


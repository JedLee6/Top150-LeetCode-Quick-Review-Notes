## 091_Graph_General_133. Clone Graph

Given a reference of a node in a **[connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph)** undirected graph.

Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

 

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

**An adjacency list** is a collection of unordered **lists** used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/133_clone_graph_question-20250310225237054.png)

```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/graph-20250310225225438.png)

```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

**Example 3:**

```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

 

**Constraints:**

- The number of nodes in the graph is in the range `[0, 100]`.
- `1 <= Node.val <= 100`
- `Node.val` is unique for each node.
- There are no repeated edges and no self-loops in the graph.
- The Graph is connected and all nodes can be visited starting from the given node.



────────────────────────────  
### Intuition and Main Idea

Now, we start with the intuition of this problem. The goal is to clone every node and its relationships in the graph, and we can't clone duplicate nodes even if the graph has cycles. To do this, we maintain a HashMap that maps each original node to its clone. When we traverse the graph using DFS or BFS, we check if a node has already been cloned. If it has, we reuse the cloned node; if it hasn't, we create a new cloned node and then copy its neighbors. Let's start with DFS.

### **Solution 1: DFS Recursive Approach**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

public class SolutionDFS {
    // In the code, we start by creating a HashMap. This HashMap maps every original node to its cloned node, ensuring that we do not clone a node more than once. It helps us handle cycles and duplicate references in the graph.
    private Map<Node, Node> visited = new HashMap<>();
    public Node cloneGraph(Node node) {
        // Inside the cloneGraph method, we check if the input node is null, if so we return null as there's nothing to process.
        if (node == null) return null;
        // Then, we check if the input node exists in our visited map. If the input node was already cloned, return the corresponding cloned node from the map to avoid duplicate references.
        if (visited.containsKey(node)) {
            return visited.get(node);
        }
        // Otherwise, we create a new cloned node with the same value as the the input node. We also initialize its neighbors list as empty because we haven’t processed its neighbors yet.
        Node clone = new Node(node.val, new ArrayList<>());
        // Next, we store the input node and its clone node in the map. So when we meet the same node again, we could retrieve its cloned node immediately.
        visited.put(node, clone);
        // After that, we iterate over all the neighbors of the input node.
        for (Node neighbor : node.neighbors) {
            // For each iteration, we recursively call cloneGraph on each neighbor node of the input node to clone it. Then we add the cloned neighbor node into the neighbors list of our cloned node.
            clone.neighbors.add(cloneGraph(neighbor));
        }
        // Finally, we return the fully cloned node, which represents the deep copy of the original graph starting from the given node.
        return clone;
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

- ### **Complexity Analysis**

  - **Time Complexity:**
      *This solution visits every node and traverses every edge in the graph exactly once. Thus, the time complexity is O(V + E), where V is the number of vertices (nodes) and E is the number of edges in the graph. Every node is cloned, and every neighbor is processed, ensuring we cover the whole graph.*
  - **Space Complexity:**
      *The space complexity is also O(V) primarily because of two factors: the recursion stack in the worst-case scenario (for a graph with many nodes, especially in a chain-like structure) and the `visited` HashMap which stores a mapping for every node. Hence, both aspects contribute linearly to the space requirements.*

> vertex /ˈvɜːrteks/ vertices /ˈvɜːrtəˌsiz/ n. In the context of graph theory, a vertex (plural: vertices) is a fundamental unit that forms a graph. It is also often called a node or a point of a graph.
>
> replicate /ˈrɛplɪkeɪt/ vt. If you replicate someone's experiment, work, or research, you do it yourself in exactly the same way.

────────────────────────────  
### **Solution 2: BFS Iterative Approach**

Next, Let's dive into the Breadth-First Search (BFS) solution, which uses a queue to traverse the graph level by level rather than recursion.

```java
public class SolutionBFS {
    public Node cloneGraph(Node node) {
        // We start with checking the base case, if the input node is null, we return null.
        if (node == null) return null;
        // Then, we create a HashMap to map every original node to its cloned node. So if a node has already been cloned, we can quickly retrieve its cloned node from the HashMap to avoid duplicate clone.
        Map<Node, Node> visited = new HashMap<>();
        // Initialize the queue for BFS.
        // Next, we create a queue using LinkedList for BFS. This queue will help us traverse the graph level by level by storing nodes as we meet them.
        Queue<Node> queue = new LinkedList<>();
        // After that, we clone the starting node and initialize its neighbors list as empty, then add it to the visited map.
        Node clone = new Node(node.val, new ArrayList<>());
        visited.put(node, clone);
        // Here, we enqueue the starting node to start the BFS traversal.
        queue.offer(node);
        // We start a while-loop that continues as long as there are nodes in the queue. This ensures we visit every node in the graph.
        while (!queue.isEmpty()) {
            // For each iteration , we dequeue the current node.
            Node current = queue.poll();
            // Then we create an enhanced for-loop to iterate through all the neighbors of the current node.
            for (Node neighbor : current.neighbors) {
                // For each neighbor, if the neighbor hasn't been cloned yet
                if (!visited.containsKey(neighbor)) {
                    // We clone the neighbor.
                    Node neighborClone = new Node(neighbor.val, new ArrayList<>());
                    // Add the neighbor and its cloned node to the visited map to avoid duplicate clone.
                    visited.put(neighbor, neighborClone);
                    // Next, we enqueue the neighbor for future processing.
                    queue.offer(neighbor);
                }
                // After that, we retrieve the cloned node of the current node from the visited map, then add it to its neighbors list. This step is what connects the cloned nodes in the same way as the original graph.
                visited.get(current).neighbors.add(visited.get(neighbor));
            }
        }
		// Finally, we return the cloned node of the starting node, which represents the entire cloned
        return clone;
    }
}
```

Alright. Let's submit the code and check if the solution works. Awesome, our submission got accepted. Then, here comes the time and space complexity analysis.

### Complexity Analysis

- **Time Complexity: ** This BFS solution visits every node in the graph once and processes all the edges. Therefore, the overall time complexity is O(V + E), where V is the number of vertices (nodes) and E is the number of edges. Each node is enqueued and each edge is examined once during the traversal.
- **Space Complexity:** The space complexity is primarily due to the `visited` HashMap and the BFS queue. In the worst case, HashMap and BFS queue will hold all the nodes, which gives us a space complexity of O(V).

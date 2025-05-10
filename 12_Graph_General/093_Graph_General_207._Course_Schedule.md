## 093_Graph_General_207._Course_Schedule

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

 

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

> Kahn /ka:n/
>
> adjacency /əˈdʒeɪsənsi/ Adjacency refers to the state or quality of being adjacent , meaning being near, next to, or sharing a boundary or edge with something else . It is a concept used in various fields such as geography, computer science, mathematics, and network theory.
>
> A back edge in graph theory, particularly during a Depth-First Search (DFS) traversal, is an edge that connects a vertex to one of its ancestors in the DFS tree. This type of edge indicates the presence of a cycle in a directed graph.
>
> prerequisite /ˌpriːˈrekwəzɪt/ If one thing is a <b>prerequisite</b> <b>for</b> another, it must happen or exist before the other thing is possible.
>
> “In‑degree” is a graph theory term that, for a given node in a directed graph, counts how many edges are coming into that node. In other words: If you imagine each course as a node, and each prerequisite pair [a, b] as a directed arrow from b → a, then the in‑degree of course a is the number of prerequisites it has—how many arrows end at a. Conversely, the out‑degree of a node is how many arrows start from it.
>

**Intuition & Main Idea**

Now, we start with the main idea of this problem. Our goal is to check if there's any **cycle** in this directed graph. If there is a cycle, we can’t finish all courses.

First, we can use **DFS (Depth-First Search)** with **3-state node tracking** to detect cycles:

- `0 = unvisited`
- `1 = visiting` (currently in DFS path)
- `2 = visited` (fully processed)

If during DFS we revisit a node that’s already being visited (`state[v] == 1`), we’ve found a **cycle** .

Instead of DFS, we can use **Kahn’s algorithm** , which is based on **in-degree counting** and **BFS traversal**.  Let's start with DFS first.

---

## Solution 1: DFS with 3-state node tracking

```java
class Solution {
    // In the code, we start by declaring an array to track each node’s visitation state
    private int[] state;
    // And we declare our adjacency list to represent the directed graph. Each index represents a course, and its value is a list of dependent courses.
    private List<List<Integer>> graph;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // And we initialize the adjacency list container
        graph = new ArrayList<>();
        // Next, we implement a for loop to traverse each course and initialize an empty list of neighbors for it.
        for (int i = 0; i < numCourses; i++) {
            // For each course, we add an empty neighbor list so we can store outgoing edges
            graph.add(new ArrayList<>());
        }
        // And then we create an enhanced for-loop to traverse each prerequisite pair in the input array
        for (int[] pre : prerequisites) {
            // Inside the loop, we unpack the dependent course and its prerequisite
            int course = pre[0], before = pre[1];
            // Next, we add the directed edge from before to course into our graph
            graph.get(before).add(course);
        }
        // And we initialize the state array to keep track of each node's visitation status. Initially, all states are 0 (unvisited).
        state = new int[numCourses];
        // Now, we loop through every course
        for (int i = 0; i < numCourses; i++) {
            // For each iteration, if course i has not been visited yet
            if (state[i] == 0) {
                // We'll start a DFS from node i to check for cycles
                if (hasCycle(i))
                    // And if a cycle is detected, we immediately return false
                    return false;
            }
        }
        // Or, if no cycles are found after exploring all nodes, we return true
        return true;
    }
	// Then we implement the hasCycle helper method to performs a DFS from a given node and returns true if a cycle is detected.
    private boolean hasCycle(int u) {
        // First, we mark node u as Visiting before exploring its neighbors
        state[u] = 1;
        // And for each neighbor v of u in the adjacency list
        for (int v : graph.get(u)) {
            // Inside the loop, if v is still Unvisited
            if (state[v] == 0) {
                // We recursively call hasCycle on v
                if (hasCycle(v))
                    // And if that recursion finds a cycle, we return true
                    return true;
            } 
            // Then we check if v is already in the Visiting state or not
            else if (state[v] == 1) {
                // If so, we’ve detected a back-edge, so we return true
                return true;
            }
        }
        // Once all neighbors are safe, we mark u as Visited
        state[u] = 2;
        // Finallu, we return false because no cycle was found starting from the node u
        return false;
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### Complexity

- **Time Complexity**
    - Building the adjacency list takes **O(P)**, where **P is the length of prerequisites**.
    - The DFS visits each node once and traverses each edge once, giving **O(N + P)**, where **N is the numCourses**.
    - Overall, the space complexity is **O(N + P)**.
- **Space Complexity**
    - The adjacency list stores **O(N + P)** entries.
    - The `state` array uses **O(N)** space.
    - The recursion stack can grow up to **O(N)** in the worst case.
    - Overall, the space complexity is **O(N + P)**.

---

## Solution 2: BFS (Kahn’s Algorithm)

Next, Let's dive into the second solution, Kahn’s Algorithm. The key idea is:

1. Compute the **in‑degree** of every node (how many prerequisites each course has).
2. Start with all nodes of in‑degree 0 (courses you can take immediately).
3. “Take” each of those courses, decrement the in‑degree of its neighbors, and enqueue any that drop to in‑degree 0.
4. If we end up taking all courses, there was no cycle; otherwise, a cycle exists.

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // And we declare an array to count prerequisites for each course
        int[] inDegree = new int[numCourses];
        // And we declare our adjacency list to represent the graph
        List<List<Integer>> graph = new ArrayList<>();
        // And for each course index from 0 to numCourses−1
        for (int i = 0; i < numCourses; i++)
            // And we add an empty list so we can record its dependent courses
            graph.add(new ArrayList<>());
        // And then for each prerequisite pair in the input
        for (int[] pre : prerequisites) {
            // And we unpack the dependent course and its prerequisite
            int course = pre[0], before = pre[1];
            // And we add an edge before → course to our graph
            graph.get(before).add(course);
            // And we increment the in‑degree of that dependent course
            inDegree[course]++;
        }
        // And we create a queue to hold courses ready to take (in‑degree 0)
        Queue<Integer> q = new ArrayDeque<>();
        // And for each course index i
        for (int i = 0; i < numCourses; i++) {
            // And if it has no prerequisites
            if (inDegree[i] == 0)
                // And we enqueue it as immediately available
                q.offer(i);
        }
        // And we initialize a counter for how many courses we’ve taken
        int taken = 0;
        // And while there are courses ready to process
        while (!q.isEmpty()) {
            // And we dequeue the next available course
            int cur = q.poll();
            // And we mark it as taken
            taken++;
            // And for each course that depends on this course
            for (int nxt : graph.get(cur)) {
                // And we decrement its in‑degree because one prerequisite is satisfied
                if (--inDegree[nxt] == 0) {
                    // And if it now has no remaining prerequisites, we enqueue it
                    q.offer(nxt);
                }
            }
        }
        // And we return true if and only if we managed to take every course
        return taken == numCourses;
    }
}
```

Alright. Let's submit the code and check if the solution works. Bravo, our submission got accepted. Then, here comes the time and space complexity analysis.

### Complexity

- **Time Complexity**
    - Computing in‑degrees and building the adjacency list takes **O(P)**, where **P = prerequisites.length**.
    - Enqueuing initial nodes is **O(N)**, where **N = numCourses**.
    - The BFS loop processes each node once and each edge once, for **O(N + P)** total.
- **Space Complexity**
    - The adjacency list holds **O(N + P)** entries.
    - The `inDegree` array uses **O(N)** space.
    - The queue can hold up to **O(N)** nodes in the worst case.
    - Overall, we use **O(N + P)** additional space.

---

## (Brief) Alternative Approach: Union-Find

You can also detect cycles by unioning prerequisites; every time you add an edge that unites two nodes already in the same set, you found a cycle. This is less common for directed graphs but can be adapted with care.



## Why are they named u and v in the sentence "And for each neighbor v of u in the adjacency list"?

The use of **"u"** and **"v"** in the phrase **"And for each neighbor v of u in the adjacency list"** is rooted in standard **graph theory notation**, where these letters are commonly used to denote **vertices (nodes)** in a graph.

### Full Explanation:

- **"u"** typically represents the **current vertex** being processed.
- **"v"** represents one of the **adjacent vertices (neighbors)** of **u**, meaning there is an edge connecting **u** and **v** .

This naming convention is widely adopted in computer science and mathematics literature, especially in discussions around graph representations like **adjacency lists** or **adjacency matrices**. For example:

- In **adjacency list** representation, for each vertex **u ∈ V**, we store a list of its neighboring vertices **v**, which are connected by an edge **(u, v) ∈ E** .
- The terms **origin and destination** may also be used in directed graphs, where **u** is the origin (tail), and **v** is the destination (head) of a directed edge .

### Why "u" and "v"?

There is no strict historical reason why the letters **u** and **v** are used — it's largely a **convention** that has evolved from mathematical graph theory:

- These letters come after **x** and **y**, which are often used for coordinates, and before other letters, making them convenient placeholders for abstract entities like graph nodes.
- They are also easy to distinguish visually when writing proofs or algorithms involving multiple vertices (e.g., **u, v, w**) .

### Example:

Given an **adjacency list** like this:
```
A: [B, C]
B: [C]
C: []
```

- For **u = A**, its neighbors **v** are **B** and **C**.
- This means edges **(A, B)** and **(A, C)** exist in the graph .

So in summary:

> The names **"u"** and **"v"** are conventional labels used in graph theory to represent vertices, particularly when describing relationships such as adjacency or edges between nodes in a graph .
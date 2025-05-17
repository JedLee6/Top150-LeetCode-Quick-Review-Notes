## 094_Graph_General_210._Course_Schedule_II

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input: numCourses = 1, prerequisites = []
Output: [0]
```

 

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- All the pairs `[ai, bi]` are **distinct**.

> Kahn /ka:n/
>
> adjacency /əˈdʒeɪsənsi/ Adjacency refers to the state or quality of being adjacent , meaning being near, next to, or sharing a boundary or edge with something else . It is a concept used in various fields such as geography, computer science, mathematics, and network theory.
>
> A back edge in graph theory, particularly during a Depth-First Search (DFS) traversal, is an edge that connects a vertex to one of its ancestors in the DFS tree. This type of edge indicates the presence of a cycle in a directed graph.
>
> prerequisite /ˌpriːˈrekwəzɪt/ If one thing is a <b>prerequisite</b> <b>for</b> another, it must happen or exist before the other thing is possible.
>
> “In‑degree” is a graph theory term that, for a given node in a directed graph, counts how many edges are coming into that node. In other words: If you imagine each course as a node, and each prerequisite pair [a, b] as a directed arrow from b → a, then the in‑degree of course a is the number of prerequisites it has—how many arrows end at a. Conversely, the out‑degree of a node is how many arrows start from it.



### Intuition & Main Idea

Now, Let's start with the main idea of this problem. When I first saw this problem, I immediately recognized it as a classic application of topological sorting in directed graphs. Here's how I think about it:

1. Each course is a vertex in a directed graph.
2. Each prerequisite [ai, bi] means there's a directed edge from bi to ai (course bi must be taken before ai).
3. Our goal is to find a valid order to take all courses, which is essentially asking for a topological sort of this graph.
4. If there's a cycle in the graph (e.g., course A requires B, which requires C, which requires A), then it's impossible to complete all courses, and we should return an empty array.

I'll present two different approaches:

1. **BFS-based approach (Kahn's algorithm)** - Using in-degree counting
2. **DFS-based approach** - Using depth-first search with cycle detection

Both approaches effectively solve the problem, and let's start with Kahn's Algorithm.

## Solution 1: BFS Approach (Kahn's Algorithm)

We'll use **Kahn’s algorithm** , which uses **in-degree counting** and **BFS traversal** to generate a topological sort or detect cycles.

Here’s the idea:

- Build adjacency list and in-degree array.
- Enqueue all nodes with zero in-degree.
- While queue is not empty:
    - Dequeue a node.
    - Reduce in-degree of its neighbors.
    - If in-degree becomes zero, enqueue the neighbor.
- If all nodes were processed, return the order. Otherwise, return empty array.

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // First, I need to build the graph representation and calculate in-degrees.
        // I'll create an adjacency list where adjList[i] contains all courses that have course i as a prerequisite
        List<List<Integer>> adjList = new ArrayList<>();
        // Initialize the adjacency list with empty lists for each course
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        // Create an array to track the in-degree (number of prerequisites) for each course
        int[] inDegree = new int[numCourses];
        // Process each prerequisite to build the graph and calculate in-degrees
        for (int[] prereq : prerequisites) {
            int course = prereq[0];        // The course we want to take
            int prerequisite = prereq[1];  // Its prerequisite
            // Add an edge from prerequisite to course in our graph
            adjList.get(prerequisite).add(course);
            // Increment the in-degree of the course
            inDegree[course]++;
        }
        // Create a queue to perform BFS, starting with all courses that have no prerequisites
        Queue<Integer> queue = new LinkedList<>();
        // Add all courses with no prerequisites (in-degree = 0) to the queue
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        // Create an array to store the topological ordering of courses
        int[] result = new int[numCourses];
        // Keep track of how many courses we've processed
        int count = 0;
        // Process courses in topological order
        while (!queue.isEmpty()) {
            // Take the next course with no prerequisites remaining
            int current = queue.poll();
            // Add this course to our result array
            result[count++] = current;
            // For each course that has this course as a prerequisite
            for (int next : adjList.get(current)) {
                // Reduce its in-degree since we've completed one of its prerequisites
                inDegree[next]--;
                // If all prerequisites for this course are now completed, add it to the queue
                if (inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
        // After processing courses in topological order. If we couldn't process all courses, there must be a cycle
        if (count != numCourses) {
            return new int[0]; // Return empty array if there's a cycle
        }
        // Return the valid course ordering
        return result;
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### Time and Space Complexity

**Time Complexity**: O(V + E) where V is the number of vertices (courses) and E is the number of edges (prerequisites). We process each vertex and edge exactly once during the BFS.

**Space Complexity**: O(V + E) for storing the adjacency list, in-degree array, queue, and result array.

## Solution 2: DFS Approach

We'll use **DFS with 3-state marking** (`unvisited`, `visiting`, `visited`) to detect cycles and generate a post-order stack of nodes. This gives us the reverse of the correct topological order.

```java
import java.util.*;

class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // First, I need to build the graph representation
        // I'll create an adjacency list where adjList[i] contains all courses that have course i as a prerequisite
        List<List<Integer>> adjList = new ArrayList<>();
        
        // Initialize the adjacency list with empty lists for each course
        for (int i = 0; i < numCourses; i++) {
            adjList.add(new ArrayList<>());
        }
        
        // Process each prerequisite to build the graph
        for (int[] prereq : prerequisites) {
            int course = prereq[0];        // The course we want to take
            int prerequisite = prereq[1];  // Its prerequisite
            
            // Add an edge from prerequisite to course in our graph
            adjList.get(prerequisite).add(course);
        }
        
        // Track the visited status of each course during DFS:
        // 0 = not visited, 1 = in current DFS path (potential cycle), 2 = completely processed
        int[] visited = new int[numCourses];
        
        // Create a stack to store the courses in reverse topological order
        // (When we pop from this stack, we'll get the correct order)
        Stack<Integer> stack = new Stack<>();
        
        // Perform DFS for each unvisited course
        for (int i = 0; i < numCourses; i++) {
            // If this course is not yet visited and we detect a cycle starting from it
            if (visited[i] == 0 && hasCycle(i, adjList, visited, stack)) {
                return new int[0]; // Return empty array if there's a cycle
            }
        }
        
        // Create an array to store the topological ordering of courses
        int[] result = new int[numCourses];
        
        // Pop from the stack to get the courses in topological order
        for (int i = 0; i < numCourses; i++) {
            result[i] = stack.pop();
        }
        
        // Return the valid course ordering
        return result;
    }
    
    // Helper method to perform DFS and detect cycles
    private boolean hasCycle(int course, List<List<Integer>> adjList, int[] visited, Stack<Integer> stack) {
        // If the course is currently in our DFS path, we've found a cycle
        if (visited[course] == 1) {
            return true; // Cycle detected
        }
        
        // If we've already completely processed this course, we can skip it
        if (visited[course] == 2) {
            return false; // No cycle
        }
        
        // Mark this course as being visited in the current DFS path
        visited[course] = 1;
        
        // Recursively visit all courses that have this course as a prerequisite
        for (int next : adjList.get(course)) {
            // If there's a cycle in the recursive call, propagate that information up
            if (hasCycle(next, adjList, visited, stack)) {
                return true; // Cycle detected
            }
        }
        
        // Mark this course as completely processed
        visited[course] = 2;
        
        // Add this course to the stack (it will be in reverse topological order)
        stack.push(course);
        
        // No cycle detected
        return false;
    }
}
```

### Time and Space Complexity Analysis

**Time Complexity**: O(V + E) where V is the number of vertices (courses) and E is the number of edges (prerequisites). We process each vertex and edge exactly once during the DFS.

**Space Complexity**: O(V + E) for storing the adjacency list, visited array, and stack. Additionally, the recursion stack can take up to O(V) space in the worst case.

## Solution 3: Alternative BFS Approach with Optimized Data Structures



```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Create arrays to represent our graph more efficiently
        // For each course, store a list of courses that depend on it
        int[][] graph = new int[numCourses][];
        
        // Track the in-degree (number of prerequisites) for each course
        int[] inDegree = new int[numCourses];
        
        // First, count the number of prerequisites for each course
        for (int[] prereq : prerequisites) {
            inDegree[prereq[0]]++;
        }
        
        // Count outgoing edges for each course to allocate proper array sizes
        int[] outCounts = new int[numCourses];
        for (int[] prereq : prerequisites) {
            outCounts[prereq[1]]++;
        }
        
        // Initialize the adjacency arrays with proper sizes
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new int[outCounts[i]];
        }
        
        // Reset outCounts to use as index trackers when building the graph
        int[] outIndex = new int[numCourses];
        
        // Build the graph by populating the adjacency arrays
        for (int[] prereq : prerequisites) {
            int course = prereq[0];
            int prerequisite = prereq[1];
            graph[prerequisite][outIndex[prerequisite]++] = course;
        }
        
        // Create a queue to perform BFS, starting with all courses that have no prerequisites
        // Use a simple array-based queue for efficiency
        int[] queue = new int[numCourses];
        int front = 0, rear = 0;
        
        // Add all courses with no prerequisites (in-degree = 0) to the queue
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue[rear++] = i;
            }
        }
        
        // Create an array to store the topological ordering of courses
        int[] result = new int[numCourses];
        int index = 0;
        
        // Process courses in topological order
        while (front < rear) {
            // Take the next course with no prerequisites remaining
            int curr = queue[front++];
            
            // Add this course to our result array
            result[index++] = curr;
            
            // For each course that has this course as a prerequisite
            for (int i = 0; i < graph[curr].length; i++) {
                int next = graph[curr][i];
                
                // Reduce its in-degree since we've completed one of its prerequisites
                inDegree[next]--;
                
                // If all prerequisites for this course are now completed, add it to the queue
                if (inDegree[next] == 0) {
                    queue[rear++] = next;
                }
            }
        }
        
        // If we couldn't process all courses, there must be a cycle
        if (index != numCourses) {
            return new int[0]; // Return empty array if there's a cycle
        }
        
        // Return the valid course ordering
        return result;
    }
}
```

### Time and Space Complexity Analysis

**Time Complexity**: O(V + E) where V is the number of vertices (courses) and E is the number of edges (prerequisites). We process each vertex and edge exactly once during the BFS.

**Space Complexity**: O(V + E) for storing the graph, in-degree array, queue, and result array. This implementation uses arrays more efficiently than dynamic collections, which might provide performance benefits for large inputs.

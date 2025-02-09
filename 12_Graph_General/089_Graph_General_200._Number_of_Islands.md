## 089_Graph_General_200. Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.



## Problem Recap

We are given a 2D binary grid representing a map of `'1'`s (land) and `'0'`s (water). An island is formed by connecting adjacent lands horizontally or vertically. The goal is to return the number of islands in the grid.

---

## Intuition and Main Ideas

### **Approach 1: Depth-First Search (DFS)**
- **Idea:**  
  Traverse the grid cell by cell. When you encounter a `'1'`, you have found an island. Then, use DFS to “sink” the entire island by marking all connected lands as visited (for example, by changing them to `'0'`). This prevents counting the same island more than once.
- **Why DFS?**  
  DFS recursively explores all connected cells (neighbors up, down, left, right) and marks them. It’s intuitive and easy to implement.

### **Approach 2: Breadth-First Search (BFS)**
- **Idea:**  
  Similar to DFS, iterate through the grid. When you find a `'1'`, use BFS to explore all connected land cells. Instead of recursion, BFS uses a queue to visit cells in a level-by-level fashion.

### **Approach 3: Union-Find (Disjoint Set Union)**
- **Idea:**  
  Treat each land cell as a node. Connect adjacent nodes (cells) if both are land. After processing, count the number of connected components (islands). The Union-Find data structure efficiently merges connected cells and tracks the number of distinct roots.

---

## Detailed Solutions

### **Solution 1: DFS Approach**

Traverse the grid cell by cell. When you encounter a `'1'`, you have found an island. Then, use DFS to “sink” the entire island by marking all connected lands as visited (for example, by changing them to `'0'`). This prevents counting the same island more than once.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // Edge-case: if the grid is null or empty, return 0 islands.
        if (grid == null || grid.length == 0) return 0;
        // Initialize the island count.
        int numIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;
        
        // Loop over every cell in the grid.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // If the current cell is land ('1'),
                // we have found an island.
                if (grid[i][j] == '1') {
                    numIslands++;  // Increment island count.
                    // Call DFS to mark the entire island as visited.
                    dfs(grid, i, j);
                }
            }
        }
        return numIslands;
    }
    
    /**
     * DFS helper function that marks all connected land cells as water ('0').
     * This "sinks" the island to avoid revisiting it.
     */
    private void dfs(char[][] grid, int i, int j) {
        int rows = grid.length;
        int cols = grid[0].length;
        // Base cases: if the cell is out-of-bound or already water.
        if (i < 0 || i >= rows || j < 0 || j >= cols || grid[i][j] == '0') {
            return;
        }
        // Mark the current cell as water to denote it is visited.
        grid[i][j] = '0';
        // Recursively visit all four adjacent cells.
        dfs(grid, i - 1, j); // Up
        dfs(grid, i + 1, j); // Down
        dfs(grid, i, j - 1); // Left
        dfs(grid, i, j + 1); // Right
    }
}
```

**Complexity Analysis - DFS Approach:**

- **Time Complexity:** O(m * n) – Each cell is visited once.
- **Space Complexity:** O(m * n) in the worst case due to recursion stack (in case of a very large island).

---

### **Solution 2: BFS Approach**

Similar to DFS, iterate through the grid. When you find a `'1'`, use BFS to explore all connected land cells. Instead of recursion, BFS uses a queue to visit cells in a level-by-level fashion.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // Edge-case: if grid is null or empty, return 0.
        if (grid == null || grid.length == 0) return 0;
        // Initialize island count.
        int numIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;
        // Loop over every cell in the grid.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // When we find land, process it.
                if (grid[i][j] == '1') {
                    numIslands++; // Increase the island count.
                    // Start BFS from this cell.
                    bfs(grid, i, j);
                }
            }
        }
        return numIslands;
    }
    
    /**
     * BFS helper function that marks all connected land cells as water.
     */
    private void bfs(char[][] grid, int i, int j) {
        int rows = grid.length;
        int cols = grid[0].length;
        // Set up an array of direction vectors.: up, down, left, right.
        int[][] directions = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
        // Initialize the queue for BFS.
        Queue<int[]> queue = new LinkedList<>();
        // Add the starting cell to the queue.
        queue.offer(new int[] { i, j });
        // Mark it as visited.
        grid[i][j] = '0';
        // While the queue is not empty, remove the front element and explore all four neighbors. Mark each found land as visited (`'0'`) and add it to the queue.
        // Process the queue until it's empty.
        while (!queue.isEmpty()) {
            // Retrieve and remove the head cell of the queue
            int[] cell = queue.poll();
            int row = cell[0];
            int col = cell[1];
            // Explore all 4 adjacent directions.
            for (int[] d : directions) {
                int newRow = row + d[0];
                int newCol = col + d[1];
                // Check bounds and whether the cell is land.
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols 
                    && grid[newRow][newCol] == '1') {
                    // Mark the cell as visited and add it to the queue.
                    grid[newRow][newCol] = '0';
                    queue.offer(new int[] { newRow, newCol });
                }
            }
        }
    }
}
```

**Complexity Analysis - BFS Approach:**

- **Time Complexity:** O(m * n) – Each cell is visited once.
- **Space Complexity:** O(min(m, n)) on average (or up to O(m * n) in worst-case scenarios with large islands).

---

### **Solution 3: Union-Find (Disjoint Set Union) Approach**

Treat each land cell as a node. Connect adjacent nodes (cells) if both are land. After processing, count the number of connected components (islands). The Union-Find data structure efficiently merges connected cells and tracks the number of distinct roots.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // Edge-case: if grid is null or empty.
        if (grid == null || grid.length == 0) return 0;
        int rows = grid.length;
        int cols = grid[0].length;
        // Create an instance of the UnionFind class that initializes each land cell as its own component.
        UnionFind uf = new UnionFind(grid);
        // Loop over each cell to traverse the grid. For every cell that is `'1'`, mark it as visited (set to `'0'`) and union it with its adjacent land neighbors.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    // Mark the current cell as visited by setting it to '0'
                    // to avoid processing it again.
                    grid[i][j] = '0';
                    // For each valid adjacent cell that is land, union the cells.
                    int[][] directions = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
                    for (int[] d : directions) {
                        int x = i + d[0];
                        int y = j + d[1];
                        // If the neighbor is within bounds and is land.
                        if (x >= 0 && x < rows && y >= 0 && y < cols && grid[x][y] == '1') {
                            uf.union(i * cols + j, x * cols + y);
                        }
                    }
                }
            }
        }
        // The number of disjoint sets in the union-find represents the number of islands.
        return uf.getCount();
    }
    
    // Union-Find (Disjoint Set Union) implementation.
    class UnionFind {
        private int count;      // Number of connected components (islands)
        private int[] parent;   // Parent array for each node
        
        public UnionFind(char[][] grid) {
            int rows = grid.length;
            int cols = grid[0].length;
            parent = new int[rows * cols];
            count = 0;
            // Initialize the union-find structure.
            for (int i = 0; i < rows; i++) {
                for (int j = 0; j < cols; j++) {
                    if (grid[i][j] == '1') {
                        // Map 2D index to 1D index.
                        int index = i * cols + j;
                        // Initialize the `parent` array where each land cell is its own parent.
                        parent[index] = index;
                        count++; // Increase count for each land cell.
                    }
                }
            }
        }
        // Uses path compression to find the root of a node.
        public int find(int i) {
            // Path compression.
            if (parent[i] != i) {
                parent[i] = find(parent[i]);
            }
            return parent[i];
        }
        // Merges two components and decreases the count if they were separate.
        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            // If roots are different, merge them.
            if (rootX != rootY) {
                parent[rootX] = rootY;
                count--; // Decrease the count of islands.
            }
        }
        // Returns the final number of disjoint sets, which corresponds to the number of islands.
        public int getCount() {
            return count;
        }
    }
}
```

**Complexity Analysis - Union-Find Approach:**

- **Time Complexity:** Nearly O(m * n) (amortized nearly constant time per union/find operation).
- **Space Complexity:** O(m * n) for the union-find data structures.

> amortize vt.  In finance, if you amortize a debt, you pay it back in regular payments.

---

## Conclusion

Each approach has its strengths. DFS and BFS are straightforward and are often preferred for their simplicity when modifying the input grid is allowed. Union-Find, on the other hand, is a powerful method when you want to maintain separate connected components without recursion or explicit queue management. Depending on the interview context and constraints, any of these solutions demonstrates a solid understanding of graph traversal and algorithm design.

This detailed explanation and the multiple solutions should help demonstrate your deep understanding of the problem and various ways to solve it in Java.
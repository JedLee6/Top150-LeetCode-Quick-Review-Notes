## 089_Graph_General_200. Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. we may assume all four edges of the grid are all surrounded by water.

 

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

The key idea is to scan the grid and, upon encountering a cell with a `'1'`, perform a traversal (using Depth-First Search (DFS) or Breadth-First Search (BFS)) to mark all connected land cells as visited. This way, each traversal will cover one island. Alternative approaches, such as using a Union-Find data structure, are also possible.

---

## Detailed Solutions

### **Solution 1: DFS Approach**

The idea is simple: we traverse the grid cell by cell. When we encounter a `'1'`, which means an island is found. Then, use DFS to “sink” the entire island by marking all connected lands as visited (for example, by changing them to `'0'`). This prevents counting the same island more than once.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // In the code, we start by checking if the grid is null or empty. If there's no grid at all or no rows, there's nothing to process, and we should immediately return 0.
        if (grid == null || grid.length == 0) return 0;
        // Next, we initialize our island counter to 0. We also determine the number of rows and columns in the grid. This helps us when iterating through the grid and in our DFS helper function.
        int numIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;
        // Then, we use nested loops to iterate over every cell in the grid. The outer loop goes through each row, and the inner loop goes through each column in that row.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // Inside the loop, we check if the current cell is '1'. If it is, that means we've found an unvisited piece of land.
                if (grid[i][j] == '1') {
                    // We then increment our island counter.
                    numIslands++;  
                    // After that, we call the DFS helper function to mark all connected land cells as visited to ensure we don't count them again later.
                    dfs(grid, i, j);
                }
            }
        }
        // After processing every cell, we return the final count of islands. Each DFS call ensures that one island is completely processed, so our counter is the total number of islands.
        return numIslands;
    }
    
    /**
   	 * Now, let's impleemnt the DFS helper function.
     * DFS helper function that marks all connected land cells as water ('0').
     * This "sinks" the island to avoid revisiting it.
     */
    private void dfs(char[][] grid, int i, int j) {
        // We start by getting the number of rows and columns in the grid
        int rows = grid.length;
        int cols = grid[0].length;
        // Then we check if the current cell is out of bounds or if it’s already water ('0').
        if (i < 0 || i >= rows || j < 0 || j >= cols || grid[i][j] == '0') {
            // If either condition is true, we simply return because there's nothing to process.
            return;
        }
        // Mark the current cell as water to denote it is visited.
        // Or, if the current cell is valid land, we mark it as water to ensure that subsequent iterations don't count the same island again.
        grid[i][j] = '0';
        // Finally, we recursively call DFS for the four adjacent directions: up, down, left, and right. These calls will continue to mark all connected pieces of land.
        dfs(grid, i - 1, j); // Up
        dfs(grid, i + 1, j); // Down
        dfs(grid, i, j - 1); // Left
        dfs(grid, i, j + 1); // Right
        // Once all recursive calls complete, the DFS function will return control to the main function, where we continue scanning for the next unvisited piece of land.
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, let's dive into the time and space complexity analysis.

**Complexity Analysis - DFS Approach:**

- **Time Complexity:** O(m * n), where m is the number of rows and n is the number of columns. Beacuse Each cell is visited once.
- **Space Complexity:** O(m * n). In the worst-case scenario, where the grid is entirely land, the recursive DFS could go as deep as O(m * n) in the call stack. However, for most typical grids, the depth will be much less.

---

### **Solution 2: BFS Approach**

Next, I want to share a Breadth-First Search (BFS) solution. This approach is similar to DFS in that we start from each '1' and mark the entire island, but we use a queue to explore the grid level by level. We add the starting cell to a queue and while the queue is not empty, we poll a cell and check its neighbors. Each neighbor that is '1' is marked as visited and added to the queue. This continues until the entire island is processed.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // First, we check if the grid is null or empty. This is important because if there's no grid at all, there are no islands, so we return 0 immediately.
        if (grid == null || grid.length == 0) return 0;
        // Next, we initialize the island count and determine the number of rows and columns in the grid to prefare for iterating over every cell in the grid.
        int numIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;
        // We then loop over every cell in the grid. The nested loops ensure that we check every row and every column.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // When we find a cell that contains '1' (land), we increment our island count and call the BFS helper function, which will traverse all connected land cells and mark them as visited.
                if (grid[i][j] == '1') {
                    numIslands++; // Increase the island count.
                    // Start BFS from this cell.
                    bfs(grid, i, j);
                }
            }
        }
        // After processing every cell, we return the final count of islands. Each BFS call ensures that one island is completely processed, so our counter is the total number of islands.
        return numIslands;
    }
    
    /**
     * Let's move on to the BFS helper function. Its purpose is to traverse every connected '1' (land) starting from the given cell and mark them as '0' to avoid double-counting.
     */
    private void bfs(char[][] grid, int i, int j) {
        // We start by getting the number of rows and columns in the grid
        int rows = grid.length;
        int cols = grid[0].length;
        // Next, we set up our direction vectors array to easily explore neighboring cells in four directions.
        int[][] directions = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
        // We then initialize a queue for BFS, add the starting cell, and immediately mark it as visited by changing its value to '0'.
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] { i, j }); // Add the starting cell to the queue.
        grid[i][j] = '0'; // Mark it as visited.
        // After that, we process the queue until it's empty.
        while (!queue.isEmpty()) {
            // Inside the loop, we retrieve and remove the head cell of the queue
            int[] cell = queue.poll();
            // We get the row and column and create an enhanced for-loop to check its 4 neighbors
            int row = cell[0];
            int col = cell[1];
            // Create an enhanced for-loop to check its 4 neighbors
            for (int[] d : directions) {
                // Inside the loop, we calculate the row and column of the neightbor cell
                int newRow = row + d[0];
                int newCol = col + d[1];
                // Then check its bounds and whether it is land.
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols 
                    && grid[newRow][newCol] == '1') {
                    // If so, we mark the cell as visited and add it to the queue for next iteration.
                    grid[newRow][newCol] = '0';
                    queue.offer(new int[] { newRow, newCol });
                }
            }
        }
        // When the iteration completed, the BFS function will return control to the main function, where we continue scanning for the next unvisited piece of land.
    }
}
```

OK. We've completed the entire solution. Now it's time to submit our code and see if everything works as expected. OK, our solution passed all the test cases. Let's break down the time and space complexity.

**Complexity Analysis - BFS Approach:**

- **Time Complexity:** O(m * n). We iterate over every cell in the grid, which gives an O(m * n) time complexity, where 'm' is the number of rows and 'n' is the number of columns. The BFS ensures that each cell is processed only once.
- **Space Complexity:** The space complexity is also O(m * n) in the worst case. This happens when the grid is entirely land, the queue of BFS would hold all the cells. However, for most typical grids, the space cost will be much less.

---

### **Solution 3: Union-Find (Disjoint Set Union) Approach**

Lastly, I’ll briefly explain an alternative solution using the Union-Find data structure. Here, every land cell is treated as a node in a disjoint-set. Initially, each '1' is its own parent. We then scan the grid and union adjacent land cells. Every union operation effectively merges two islands, and the final count of disjoint sets gives the number of islands. This method is efficient and particularly elegant for handling connectivity problems.

> island /ˈaɪlənd/ n. An island is a piece of land that is completely surrounded by water.
>
> disjoint /dɪsˈdʒɔɪnt/ adj. (of two sets) having no members in common
>
> adjacent /əˈdʒeɪs(ə)nt/ adj. If one thing is adjacent to another, the two things are next to each other.
>
> For this problem, although it is easier and probably suggested to use BFS, but Union find also comes handy and can be easily extended to solve Island 2 and Surrounded regions.

```java
class Solution {
    public int numIslands(char[][] grid) {
        // We begin with a simple edge-case check. If the grid is null or has no rows, there's nothing to process, so we return 0.
        if (grid == null || grid.length == 0) return 0;
        // Next, we retrieve the rows and columns of the grid
        int rows = grid.length; // Initialize `rows` as `grid dot length`
        int cols = grid[0].length; // Initialize `columns` as `grid at index 0 dot length`
        // Then we create an instance of the UnionFind class that initializes each land cell as its own component.
        UnionFind uf = new UnionFind(grid);
        // We then iterate over every cell in the grid using nested loops.
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // When we encounter a '1' (a land cell)
                if (grid[i][j] == '1') {
                    // Mark the current cell as visited by setting it to '0' to avoid processing it again.
                    grid[i][j] = '0';
                    // Next, we set up our direction vectors array to easily explore neighboring cells in four directions.
                    int[][] directions = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
                    // Create an enhanced for-loop to check its 4 neighbors
                    for (int[] d : directions) {
                        // Inside the loop, we calculate the row and column of the neightbor cell
                        int x = i + d[0];
                        int y = j + d[1];
                        // For each neighbor that is within bounds and is land, we perform a union operation to merge the two pieces of land.
                        if (x >= 0 && x < rows && y >= 0 && y < cols && grid[x][y] == '1') {
                            uf.union(i * cols + j, x * cols + y);
                        }
                    }
                }
            }
        }
        // After processing all cells, the total number of disjoint sets in our Union-Find structure is number of islands. So we return it.
        return uf.getCount();
    }
    
    // Now let’s dive into our UnionFind class, which is the backbone of our solution.
    class UnionFind {
        private int count; // Number of connected components (islands)
        private int[] parent; // Parent array for each node
        // The constructor takes the grid as input.
        public UnionFind(char[][] grid) {
            // We calculate the total number of cells by multiplying the number of rows by the number of columns.
            int rows = grid.length;
            int cols = grid[0].length;
            // Then, we initialize the parent array and count.
            parent = new int[rows * cols];
            count = 0;
            // Next, create an enhanced for-loop to initialize the union-find structure.
            for (int i = 0; i < rows; i++) {
                for (int j = 0; j < cols; j++) {
                    // For every cell that contains a '1',  
                    if (grid[i][j] == '1') {
                        // We map its 2D position into a 1D index
                        int index = i * cols + j;
                        // And set the parent of that index to itself, as each land cell is its own parent.
                        parent[index] = index;
                        // At the same time, we increment our island count, as each '1' starts as its own island.
                        count++;
                    }
                }
            }
        }
        // The find method uses path compression to find the root of a node. So that every time we search for a root, we flatten the structure by pointing nodes directly to the root. This optimization helps speed up future queries.
        public int find(int i) {
            // If an element is the root of its own set, then parent[i] would equal i. If parent[i] is not equal to i, it means that i is not the root, and we need to look further up the chain.
            if (parent[i] != i) {
                // Since i is not the root, we make a recursive call to find(parent[i]) to find the root of i's parent.
                parent[i] = find(parent[i]);
            }
            // As we go through this recursive process, we set parent[i] to the result of find(parent[i]). This effectively "compresses" the path by directly linking i to the root. This optimization speeds up future find calls because it shortens the chain of nodes we need to traverse.
            return parent[i];
        }
        // Merges two components and decreases the count if they were separate.
        public void union(int x, int y) {
            // We find the roots of both elements. If the roots are different, we link one root to the other and decrement our island count because two islands have now merged into one.
            int rootX = find(x);
            int rootY = find(y);
            // If roots are different, merge them.
            if (rootX != rootY) {
                // we link one root to the other.
                parent[rootX] = rootY;
                count--; // Decrease the count of islands because two islands have now merged into one.
            }
            // In the Union-Find structure, each element belongs to a "set" or "component" that is represented by a root. When you call the union operation for two elements, you first find their respective roots. If the roots are the same, it means they already belong to the same connected component (i.e., they are already "merged"). There’s no need to perform another merge since doing so wouldn’t change the structure of our sets—it would simply be redundant. This is why, in our union method, we only merge if the roots differ.
        }
        // Finally, the getCount method simply returns the number of remaining disjoint sets, which represents the total number of islands.
        public int getCount() {
            return count;
        }
    }
}
```

Alright. Let's submit the code and check if the solution works. Awesome, our submission got accepted. Then, here comes the time and space complexity analysis.

**Complexity Analysis - Union-Find Approach:**

- **Time Complexity:** We traverse every cell in the grid once, leading to a time complexity of O(m * n), where m is the number of rows and n is the number of columns. The union and find operations are nearly constant time on average due to path compression.
- **Space Complexity:** The space complexity is also O(m * n) because we maintain an array to represent the parent of each cell. Additionally, the recursion in the find method uses little extra space due to path compression.

> amortize vt.  In finance, if we amortize a debt, we pay it back in regular payments.

---

## Conclusion

Each approach has its strengths. DFS and BFS are straightforward and are often preferred for their simplicity when modifying the input grid is allowed. Union-Find, on the other hand, is a powerful method when we want to maintain separate connected components without recursion or explicit queue management. Depending on the interview context and constraints, any of these solutions demonstrates a solid understanding of graph traversal and algorithm design.

This detailed explanation and the multiple solutions should help demonstrate wer deep understanding of the problem and various ways to solve it in Java.
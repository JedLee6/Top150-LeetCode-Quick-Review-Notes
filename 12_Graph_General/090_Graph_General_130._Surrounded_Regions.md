## 090_Graph_General_130. Surrounded Regions

You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

To capture a **surrounded region**, replace all `'O'`s with `'X'`s **in-place** within the original board. You do not need to return anything.

 

**Example 1:**

**Input:** board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/xogrid.jpg)

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = [["X"]]

**Output:** [["X"]]

 

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.



---

## **Problem Recap**

Given an m x n board with characters `'X'` and `'O'`, we want to “capture” all regions of `'O'` that are completely surrounded by `'X'`. A region is captured by flipping all its `'O'`s to `'X'`. However, any `'O'` connected to a border (directly or indirectly via adjacent cells) is safe and should remain unchanged.

---

## **Intuition and Main Idea**

OK. Let's get into the intuition of this problem. Our intuition is to first identify all the `'O'`s that are safe—that is, any `'O'` on the border or connected to one cannot be captured. We achieve this by performing a depth-first search (DFS) or breadth-first search starting from the borders to mark safe cells. Once that’s done, we'll flip all remaining surrounded `'O'` cells to `'X'`. Let's start with DFS.

---

## **Solution 1: Depth-First Search (DFS)**

We achieve this by performing a depth-first search (DFS) starting from the borders to mark safe cells. Once that’s done, we'll flip all remaining surrounded `'O'` cells to `'X'`.

### **DFS Code with Detailed Annotations:**

```java
public class Solution {
    public void solve(char[][] board) {
        // In the code, we begin by checking if the board is null or empty. If it is, there's nothing to process, so we simply return.
        if (board == null || board.length == 0) return;
        // Next, we store the number of rows (m) and columns (n) for later use in our loops.
        int m = board.length, n = board[0].length;
        // We then iterate over the first and last column for each row.
        for (int i = 0; i < m; i++) {
            // For each row, we check the first and last column. If we find an 'O', we call the dfs method starting from that cell. This ensures we mark all 'O's connected to the border on the left and right sides.
            if (board[i][0] == 'O') dfs(board, i, 0);
            if (board[i][n - 1] == 'O') dfs(board, i, n - 1);
        }
        // Similarly, we iterate over the first and last row for each column.
        for (int j = 0; j < n; j++) {
            // Any 'O' found here also triggers a DFS method to mark connected safe cells.
            if (board[0][j] == 'O') dfs(board, 0, j);
            if (board[m - 1][j] == 'O') dfs(board, m - 1, j);
        }
        // After all safe 'O's are marked, we scan through the entire board. 
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If we see an 'O', it means it was not marked safe and is fully surrounded by 'X's, so we flip it to 'X'.
                if (board[i][j] == 'O') {
                    board[i][j] = 'X'; // This 'O' is surrounded, so capture it.
                    // Or, if a cell is marked with 'S', we change it back to 'O' since it's part of a safe region.
                } else if (board[i][j] == 'S') {
                    board[i][j] = 'O'; // Revert safe-marked cells back to 'O'.
                }
            }
        }
    }
    
    // Now, let's impleemnt the DFS helper method.
    private void dfs(char[][] board, int i, int j) {
        // We start by getting the number of rows and columns in the grid
        int m = board.length, n = board[0].length;
        // Next, we check if the current position is valid and if the cell contains 'O'. If not, we stop.
        if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != 'O') return;
        // Otherwise, we mark the cell as safe by setting it to 'S'. 
        board[i][j] = 'S';
        // Finally, we recursively call dfs on the four neighboring cells: up, down, left, and right. This way, every connected 'O' from the border is marked safe.
        dfs(board, i - 1, j); // Up
        dfs(board, i + 1, j); // Down
        dfs(board, i, j - 1); // Left
        dfs(board, i, j + 1); // Right
        // Once all recursive calls complete, the DFS method will return control to the main method, where we continue scanning for the next unvisited piece of land.
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### **Time and Space Complexity (DFS):**

- **Time Complexity:**
  The algorithm scans through the board several times. The initial border checks and DFS calls ensure that each cell is visited only once. Therefore, the overall time complexity is O(m \* n), where m is the number of rows and n is the number of columns.
- **Space Complexity:**
    The space complexity depends on the recursion stack used by DFS. In the worst-case scenario, where almost all cells are 'O', the recursion depth could be O(m \* n) in a degenerate case. Hence, the space complexity is O(m \* n) in the worst case. However, under typical circumstances, it won't be that extreme.

---

## **Solution 2: Breadth-First Search (BFS)**

Next, Let's dive into the Breadth-First Search (BFS) solution, which is similar to DFS. For each `'O'` on the border, we perform a BFS. In our BFS method, we use a queue to explore all four directions from the current cell. As we visit an `'O'`, we mark it safe. After processing all border-connected cells, we traverse the entire board. If a cell is still an `'O'`, it means it’s fully surrounded and we flip it to `'X'`. Lastly, we restore the safe cells by converting `'S'` back to `'O'`.

### BFS Code with Detailed Annotations:

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public void solve(char[][] board) {
        // We begin by checking if the board is null or empty, and if so, we return early.
        if (board == null || board.length == 0) return;
        // After the validation, we determine the dimensions of the board. We store the number of rows in m and the number of columns in n.
        int m = board.length, n = board[0].length;
        
        // Process all border cells and perform BFS on 'O's found on the border.
        // Now we use a for-loop that iterates over each row
        for (int i = 0; i < m; i++) {
            // And for each row, we check the left border cell and the right border cell.
            // Left border cell
            // If we encounter an 'O' on either side, we call the bfs method to mark all 'O's connected to the border, so it prevents them from being incorrectly captured.
            if (board[i][0] == 'O') {
                
                bfs(board, i, 0);
            }
            // And the code for right border cell is similar to the previous one
            if (board[i][n - 1] == 'O') {
                bfs(board, i, n - 1);
            }
        }
        // Similarly, we use another for-loop to iterate over the columns. 
        for (int j = 0; j < n; j++) {
            // In this loop, we check the top border cell and the bottom border cell. Once again, if an 'O' is found, we call the bfs method to mark all border 'O's, and their connected regions are marked as safe.
            // Top border cell
            if (board[0][j] == 'O') {
                bfs(board, 0, j);
            }
            // Then we handle the bottom border cell
            if (board[m - 1][j] == 'O') {
                bfs(board, m - 1, j);
            }
        }
		// After marking the safe regions, we traverse the entire board.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // For each cell, if it is still an 'O', it means it was not connected to any border and is therefore surrounded; we flip it to 'X'.
                if (board[i][j] == 'O') {
                    board[i][j] = 'X'; // Capture the surrounded region.
                    // Conversely, if the cell contains an 'S', which is our temporary marker for safe regions, we revert it back to 'O'.
                } else if (board[i][j] == 'S') {
                    board[i][j] = 'O'; // Restore safe region.
                }
                // This two-step process ensures that only the surrounded regions are captured.
            }
        }
    }
    // We now move on to the BFS helper method. This method marks 'O's that are connected to the border as safe.
    private void bfs(char[][] board, int i, int j) {
        // Within the BFS method, we first re-establish the board dimensions m and n for easy reference.
        int m = board.length, n = board[0].length;
        // Then, we initialize a queue tfor breadth-first search, and this queue will keep track of cells to be processed.
        Queue<int[]> queue = new LinkedList<>();
        // Here, we mark the starting cell as safe and add it to the queue.
        board[i][j] = 'S';
        queue.offer(new int[]{i, j});
        // Next, we define an array of directions that represent the four possible moves: down, up, right, and left. This array is used to explore all adjacent cells
        int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        // We then create a while-loop that continues until the queue is empty
        while (!queue.isEmpty()) {
            // At the start of each iteration, we dequeue the current cell and extract its row and column indices.
            int[] cur = queue.poll();
            int curI = cur[0], curJ = cur[1];
            // Next, we loop over each direction in our directions array.
            for (int[] dir : directions) {
                // For every neighbor, we calculate its coordinates by adding the direction values to the current cell's coordinates.
                int newI = curI + dir[0];
                int newJ = curJ + dir[1];
                // We then check whether the new coordinates are within bounds and whether the cell contains an 'O'.
                // Check boundaries and if the neighbor is an 'O'
                if (newI >= 0 && newI < m && newJ >= 0 && newJ < n && board[newI][newJ] == 'O') {
                    // If both conditions are met, we mark the neighbor as safe, and we enqueue it for further exploration.
                    board[newI][newJ] = 'S'; // Mark as safe.
                    queue.offer(new int[]{newI, newJ}); // Enqueue the neighbor.
                }
            }
        }
    }
}
```

### **Time and Space Complexity (BFS):**

**Time Complexity:** The time complexity of this solution is **O(M \* N)**, where M is the number of rows and N is the number of columns in the board. In the worst-case scenario, we might visit every cell on the board once during the BFS traversal.  We iterate through the border in the `solve` method, and in `bfs` method, each cell is enqueued and dequeued at most once. The nested loops in the final step of `solve`  method also iterate through each cell once. Therefore, the overall time complexity is O(M * N).

**Space Complexity:** The space complexity is also **O(M \* N)**. In the worst case, if the entire board is filled with 'O's and connected to the border, our queue might end up containing almost all cells of the board. This is because, in such a scenario, the BFS would explore and enqueue almost every 'O' cell before marking them as safe.

---

## **Solution 3: Union-Find (Disjoint Set Union)**

Lastly, let's get into the union-find data structure, also known as disjoint set union .Here, We create a dummy node representing all safe cells. Union each `'O'` on the border (and their connected cells) with the dummy node.. Then, We traverse the inner cells to union connected 'O's. Finally, any 'O' that isn’t connected to the dummy node is flipped to 'X'.  

### **Union-Find Code with Detailed Annotations:**

```java
public class Solution {
    // We start by declaring a two-dimensional array called directions. It stores four possible directions to move from a cell to its neighbors: down ({1, 0}), up ({-1, 0}), right ({0, 1}), and left ({0, -1})
    private final int[][] directions = { {1, 0}, {-1, 0}, {0, 1}, {0, -1} };
    
    public void solve(char[][] board) {
        // Inside the 'solve' method, we check if the board is null or has zero length, and if so, we return immediately to avoid null pointer exceptions.
        if (board == null || board.length == 0) return;
        // Here, we get the dimensions of the board, m for rows and n for columns.
        int m = board.length, n = board[0].length;
        // Then, we create an instance of our UnionFind data structure, passing the board and its dimensions
        UnionFind uf = new UnionFind(board, m, n);
        // Additionally, we create a dummy node whose index is set to m * n, which is one position beyond the last valid cell index. This dummy node acts as a representative for all border 'O's, and by unioning border cells with this dummy, we can later check which cells are safe.
        int dummy = m * n; // Dummy node index.
        
        // Next, we create two loops to union border 'O's with the dummy node.
        for (int i = 0; i < m; i++) {
            // For every row, we check the leftmost cell (board[i][0]) and the rightmost cell (board[i][n - 1]). If either is an 'O', we union that cell's index with the dummy node.
            if (board[i][0] == 'O') uf.union(i * n, dummy);
            if (board[i][n - 1] == 'O') uf.union(i * n + n - 1, dummy);
        }
        // Then, we repeat the process for the top and bottom rows by iterating over every column.
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O') uf.union(j, dummy);
            if (board[m - 1][j] == 'O') uf.union((m - 1) * n + j, dummy);
        }
        // In this way, all border 'O's are connected to the dummy node, and consequently, all 'O's connected to these borders will be marked as safe.
        // After that, we create a nested loop to process the inner cells of the board
        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                // For each cell that contains an 'O', we check all four adjacent cells using our predefined directions array.
                if (board[i][j] == 'O') {
                    // Check in four directions.
                    for (int[] d : directions) {
                        // // Inside the loop, we calculate the row and column of the adjacent cell
                        int x = i + d[0], y = j + d[1];
                        // If an adjacent cell is also an 'O', we perform a union operation to connect these two cells in the Union-Find structure.
                        if (board[x][y] == 'O') {
                            uf.union(i * n + j, x * n + y);
                            // This step is crucial because it links all adjacent 'O's, allowing us to eventually know if a particular 'O' is safe
                        }
                    }
                }
            }
        }
        // After all unions, flip non-border-connected 'O's to 'X'.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If current 'O' is not connected to dummy, it's surrounded.
                if (board[i][j] == 'O' && !uf.isConnected(i * n + j, dummy)) {
                    board[i][j] = 'X';
                }
            }
        }
    }
    // Now, let's declare a nested class named UnionFind
    class UnionFind {
        // And it contains an integer array parent to keep track of each node's parent. 
        int[] parent;
        //   Moreover, we also set the dummy node's parent to itself, thus preparing it for union operations.
        public UnionFind(char[][] board, int m, int n) {
            // In the constructor, we initialize the parent array with a size of m * n + 1 to cover every board cell plus the dummy node.
            parent = new int[m * n + 1]; // Extra node for dummy.
            // We then iterate through each valid cell index and set its parent to itself, which establishes each cell as its own separate component initially.
            for (int i = 0; i < m * n; i++) {
                parent[i] = i;
            }
            // Set dummy's parent.
            parent[m * n] = m * n;
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
        // Finally, the isConnected method simply checks whether two nodes belong to the same component by comparing their roots using the find method. This method is critical in our final board traversal, where we decide which 'O's to flip based on whether they are connected to the dummy node.
        public boolean isConnected(int i, int j) {
            return find(i) == find(j);
        }
    }
}
```

### **Time and Space Complexity (Union-Find):**

- **Time Complexity:** The time complexity of this Union-Find solution is **nearly O(M \* N)**, where M is the number of rows and N is the number of columns.  While technically, with path compression and union by rank (which we have path compression here), the amortized time complexity of Union-Find operations (`find` and `union`) is almost constant, practically, for each cell, we perform a constant number of Union-Find operations. We iterate through the board a few times: to union border 'O's, union inner adjacent 'O's, and finally to check for surrounded regions. Each of these iterations takes O(M * N) in the worst case. Therefore, the overall time complexity is dominated by these iterations, resulting in approximately O(M * N).  The Union-Find operations themselves contribute a very small overhead due to path compression.
- **Space Complexity:** The space complexity is **O(M \* N)**. This comes from the `parent` array in our `UnionFind` class, which stores parent information for each cell on the board, plus the dummy node.  The size of the `parent` array is directly proportional to the number of cells in the board, hence O(M * N).

> conversely ADV. /kɒnˈvɜːslɪ/ You say conversely to indicate that the situation you are about to describe is the opposite or reverse of the one you have just described
>
> proportional ADJ. If one amount is proportional to another, the two amounts increase and decrease at the same rate so there is always the same relationship between them.
>
> representative /ˌrɛprɪˈzɛntətɪv/ N-COUNT. A representative is a person who has been chosen to act or make decisions on behalf of another person or a group of people.; ADJ. Someone who is typical of the group to which they belong can be described as representative.


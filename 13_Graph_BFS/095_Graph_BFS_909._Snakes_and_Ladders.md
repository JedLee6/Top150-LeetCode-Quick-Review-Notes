## 095_Graph_BFS_909._Snakes_and_Ladders

You are given an `n x n` integer matrix `board` where the cells are labeled from `1` to `n^2` in a [**Boustrophedon style**](https://en.wikipedia.org/wiki/Boustrophedon) starting from the bottom left of the board (i.e. `board[n - 1][0]`) and alternating direction each row.

You start on square `1` of the board. In each move, starting from square `curr`, do the following:

- Choose a destination square

     

    ```
    next
    ```

     

    with a label in the range

     

    ```
    [curr + 1, min(curr + 6, n2)]
    ```

    .

    - This choice simulates the result of a standard **6-sided die roll**: i.e., there are always at most 6 destinations, regardless of the size of the board.

- If `next` has a snake or ladder, you **must** move to the destination of that snake or ladder. Otherwise, you move to `next`.

- The game ends when you reach the square `n2`.

A board square on row `r` and column `c` has a snake or ladder if `board[r][c] != -1`. The destination of that snake or ladder is `board[r][c]`. Squares `1` and `n2` are not the starting points of any snake or ladder.

Note that you only take a snake or ladder at most once per dice roll. If the destination to a snake or ladder is the start of another snake or ladder, you do **not** follow the subsequent snake or ladder.

- For example, suppose the board is `[[-1,4],[-1,3]]`, and on the first move, your destination square is `2`. You follow the ladder to square `3`, but do **not** follow the subsequent ladder to `4`.

Return *the least number of dice rolls required to reach the square* `n2`*. If it is not possible to reach the square, return* `-1`.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/snakes.png)

```
Input: board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation: 
In the beginning, you start at square 1 (at row 5, column 0).
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 and must take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
This is the lowest possible number of moves to reach the last square, so return 4.
```

**Example 2:**

```
Input: board = [[-1,-1],[-1,3]]
Output: 1
```

 

**Constraints:**

- `n == board.length == board[i].length`
- `2 <= n <= 20`
- `board[i][j]` is either `-1` or in the range `[1, n2]`.
- The squares labeled `1` and `n2` are not the starting points of any snake or ladder.



> matrix /ˈmeɪtrɪks/ N. In mathematics, a matrix is an arrangement of numbers, symbols, or letters in rows and columns which is used in solving mathematical problems.
>
> Boustrophedon /ˌbuːstrəˈfiːdən/ N. Boustrophedon is a style of writing in which alternate lines of writing are reversed, with letters also written in reverse, mirror-style. This is in contrast to modern European languages, where lines always begin on the same side, usually the left.
>
> alternate /ˈɔːltərneɪt/ VI. When you <b>alternate</b> two things, you keep using one then the other. When one thing <b>alternates</b> <b>with</b> another, the first regularly occurs after the other. ADJ. <b>Alternate</b> actions, events, or processes regularly occur after each other.
>
> In programming, indexing is how we refer to positions in an array or list. "1-indexed" means labels start at 1, not 0. Java code uses arrays which are 0-indexed.
>
> ladder [ˈlædər] N.  <b>ladder</b> is a piece of equipment used for climbing up something or down from something. It consists of two long pieces of wood, metal, or rope with steps fixed between them.
>
> "Snakes and Ladders" originated in ancient India, where it was known as Moksha Patam.
> It was created to teach moral lessons: virtues (represented by ladders) could take you forward, while vices (represented by snakes) pulled you backward. A snake represents a negative force—if you land on a snake's head, you slide down its tail. It's a setback in the game, mimicking a fall in morality or progress. A ladder symbolizes a boost or shortcut—if you land at the bottom, you can climb up to a higher square.
>
> Dijkstra's algorithm (/ˈdaɪkstrəz/ DYKE-strəz) is an algorithm for finding the shortest paths between nodes in a weighted graph, which may represent, for example, a road network. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later.



## Intuition and Main Ideas

When we look at this problem, we see it as a shortest path problem in a graph. Each cell on the board is a node, and from each node, we can move to at most 6 other nodes (simulating a dice roll). If there's a snake or ladder, we must take it.

The key insights:
1. This is fundamentally a shortest path problem where we want to find the minimum number of moves (dice rolls) to reach the final square.
2. We need to handle the Boustrophedon style board numbering, which requires mapping between the cell number and the board coordinates.
3. BFS is ideal for finding the shortest path in an unweighted graph.

We have two different solutions:
1. **BFS with queue** - The classic approach for shortest path problems
2. **Dijkstra's algorithm** - Though overkill for this problem, it demonstrates another approach

Now, let's start with BFS with queue first.

## Solution 1: BFS with Queue

We'll use BFS to find the shortest path from cell 1 to cell n². We'll maintain a queue of positions to explore and a visited set to avoid cycles. For each position, we'll try all possible dice rolls (1-6) and use BFS to find the minimum number of moves.

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        // In the code, we start by determining the size of the board
        int n = board.length;
        // We'll use a queue for BFS to find the shortest path
        Queue<Integer> queue = new LinkedList<>();
        // we need to track which cells we've already visited to avoid cycles
        boolean[] visited = new boolean[n * n + 1];
        // Start BFS from cell 1 (the starting point)
        queue.offer(1);
        visited[1] = true;
        // Initialize the number of moves to 0
        int moves = 0;
        // Begin the BFS traversal
        while (!queue.isEmpty()) {
            // For each level of BFS, process all cells at the current distance
            int size = queue.size();
            // Process all cells at the current distance
            for (int i = 0; i < size; i++) {
                // Get the current cell
                int curr = queue.poll();
                // If we've reached the destination, return the number of moves
                if (curr == n * n) {
                    return moves;
                }
                // Try all possible dice rolls (1-6)
                for (int j = 1; j <= 6; j++) {
                    // Calculate the next position after the dice roll
                    int next = curr + j;
                    // If we've gone beyond the board, skip this roll
                    if (next > n * n) {
                        continue;
                    }
                    // Convert the cell number to board coordinates
                    int[] coordinates = getCoordinates(next, n);
                    int row = coordinates[0];
                    int col = coordinates[1];
                    // Check if there's a snake or ladder at this position
                    if (board[row][col] != -1) {
                        // If there is, we must move to the destination of the snake or ladder
                        next = board[row][col];
                    }
                    // If we haven't visited this cell yet, add it to the queue
                    if (!visited[next]) {
                        visited[next] = true;
                        queue.offer(next);
                    }
                }
            }
            // Increment the number of moves for this level of BFS
            moves++;
        }
        // If we can't reach the destination, return -1
        return -1;
    }
    // Helper method to convert cell number to board coordinates
    private int[] getCoordinates(int cell, int n) {
        // First, we'll adjust the cell number to 0-indexed
        cell = cell - 1;
        // Calculate the row and column in a normal grid
        int row = n - 1 - (cell / n);
        int col = cell % n;
        // Adjust the column for even rows (0-indexed) to handle the Boustrophedon style
        if ((n - 1 - row) % 2 == 1) {
            col = n - 1 - col;
        }
        // Return the coordinates as [row, col]
        return new int[]{row, col};
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### Time and Space Complexity Analysis:

- **Time Complexity**: O(n²)
  - We have n² cells, and each cell is visited at most once due to our visited array.
  - For each cell, we consider up to 6 next moves, which is O(1).
  - The coordinate conversion is also O(1).
  - Overall, we process each cell at most once, so the time complexity is O(n²).

- **Space Complexity**: O(n²)
  - The queue can grow up to O(n²) in the worst case.
  - The visited array is of size n² + 1.
  - The coordinate conversion and other variables use O(1) space.
  - Overall, the space complexity is O(n²).

## Solution 2: Dijkstra's Algorithm

### Main Idea
While Dijkstra's algorithm is typically used for weighted graphs, we can adapt it for this problem. We'll use a priority queue to always process the cell with the fewest moves first. This ensures we find the shortest path.

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        // First, we need to determine the size of the board
        int n = board.length;
        // we'll use a priority queue for Dijkstra's algorithm
        // Each entry is [cell, moves] where moves is the number of dice rolls to reach this cell
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        // we need to track the minimum moves to reach each cell
        int[] distance = new int[n * n + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);
        // Start from cell 1 with 0 moves
        pq.offer(new int[]{1, 0});
        distance[1] = 0;
        // Begin Dijkstra's algorithm
        while (!pq.isEmpty()) {
            // Get the cell with the fewest moves so far
            int[] current = pq.poll();
            int cell = current[0];
            int moves = current[1];
            // If we've reached the destination, return the number of moves
            if (cell == n * n) {
                return moves;
            }
            // If we've already found a better path to this cell, skip it
            if (moves > distance[cell]) {
                continue;
            }
            // Try all possible dice rolls (1-6)
            for (int i = 1; i <= 6; i++) {
                // Calculate the next position after the dice roll
                int next = cell + i;
                // If we've gone beyond the board, skip this roll
                if (next > n * n) {
                    continue;
                }
                // Convert the cell number to board coordinates
                int[] coordinates = getCoordinates(next, n);
                int row = coordinates[0];
                int col = coordinates[1];
                // Check if there's a snake or ladder at this position
                if (board[row][col] != -1) {
                    // If there is, we must move to the destination of the snake or ladder
                    next = board[row][col];
                }
                // If we've found a shorter path to the next cell, update it
                if (moves + 1 < distance[next]) {
                    distance[next] = moves + 1;
                    pq.offer(new int[]{next, moves + 1});
                }
            }
        }
        // If we can't reach the destination, return -1
        return -1;
    }
    // Helper method to convert cell number to board coordinates
    private int[] getCoordinates(int cell, int n) {
        // First, we'll adjust the cell number to 0-indexed
        cell = cell - 1;
        // Calculate the row and column in a normal grid
        int row = n - 1 - (cell / n);
        int col = cell % n;
        // Adjust the column for even rows (0-indexed) to handle the Boustrophedon style
        if ((n - 1 - row) % 2 == 1) {
            col = n - 1 - col;
        }
        // Return the coordinates as [row, col]
        return new int[]{row, col};
    }
}
```

### Time and Space Complexity Analysis:
- **Time Complexity**: O(n² log n²)
  - In the worst case, we might add each cell to the priority queue multiple times (up to 6 times per cell).
  - There are n² cells, so we might have up to O(n²) operations on the priority queue.
  - Each priority queue operation costs O(log m) where m is the size of the queue, which is at most O(n²).
  - Therefore, the time complexity is O(n² log n²).
  - The coordinate conversion for each cell is O(1).

- **Space Complexity**: O(n²)
  - The priority queue can grow up to O(n²) in the worst case.
  - The distance array is of size n² + 1.
  - The coordinate conversion and other variables use O(1) space.
  - Overall, the space complexity is O(n²).

## Solution 3: BFS with Optimization

### Main Idea
We can optimize our BFS solution by directly calculating the board position without using a helper function each time. Additionally, we can use a more compact way to track both the cell and the number of moves.

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        // First, we need to determine the size of the board
        int n = board.length;
        
        // we'll use a queue for BFS where each element is [cell, moves]
        Queue<int[]> queue = new LinkedList<>();
        
        // we need to track which cells we've already visited
        boolean[] visited = new boolean[n * n + 1];
        
        // Start BFS from cell 1 with 0 moves
        queue.offer(new int[]{1, 0});
        visited[1] = true;
        
        // Begin the BFS traversal
        while (!queue.isEmpty()) {
            // Get the current state
            int[] state = queue.poll();
            int cell = state[0];
            int moves = state[1];
            
            // If we've reached the destination, return the number of moves
            if (cell == n * n) {
                return moves;
            }
            
            // Try all possible dice rolls (1-6)
            for (int we = 1; i <= 6; i++) {
                // Calculate the next position after the dice roll
                int nextCell = cell + i;
                
                // If we've gone beyond the board, skip this roll
                if (nextCell > n * n) {
                    continue;
                }
                
                // Calculate the row and column for the next cell
                // First, we'll adjust to 0-indexed
                int cellIndex = nextCell - 1;
                // Calculate the row (0-indexed from top)
                int row = n - 1 - (cellIndex / n);
                // Calculate the column
                int col = (cellIndex / n) % 2 == 0 ? cellIndex % n : n - 1 - (cellIndex % n);
                
                // Check if there's a snake or ladder at this position
                if (board[row][col] != -1) {
                    // If there is, we must move to the destination of the snake or ladder
                    nextCell = board[row][col];
                }
                
                // If we haven't visited this cell yet, add it to the queue
                if (!visited[nextCell]) {
                    visited[nextCell] = true;
                    queue.offer(new int[]{nextCell, moves + 1});
                }
            }
        }
        
        // If we can't reach the destination, return -1
        return -1;
    }
}
```

### Time and Space Complexity Analysis:
- **Time Complexity**: O(n²)
  - We have n² cells, and each cell is visited at most once due to our visited array.
  - For each cell, we consider up to 6 next moves, which is O(1).
  - The coordinate calculation is inlined and is O(1).
  - Overall, we process each cell at most once, so the time complexity is O(n²).

- **Space Complexity**: O(n²)
  - The queue can grow up to O(n²) in the worst case.
  - The visited array is of size n² + 1.
  - Other variables use O(1) space.
  - Overall, the space complexity is O(n²).

## Conclusion

All three solutions effectively solve the Snakes and Ladders problem:

1. **BFS with Queue**: A clean, straightforward approach that's optimal for this type of shortest path problem.
2. **Dijkstra's Algorithm**: A more general approach that's slightly overkill for this problem but demonstrates flexibility.
3. **BFS with Optimization**: A refined BFS approach with inline coordinate calculation for better efficiency.

The BFS solution is the most appropriate for this problem because:
- The graph is unweighted (each dice roll is one move)
- We're looking for the shortest path
- BFS guarantees the shortest path in unweighted graphs

In practice, we would recommend the optimized BFS solution (Solution 3) for its clarity and efficiency.
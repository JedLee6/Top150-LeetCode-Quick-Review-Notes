## 105_Backtracking_52._N-Queens_II

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *the number of distinct solutions to the **n-queens puzzle***.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/queens.jpg)

```
Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
```

**Example 2:**

```
Input: n = 1
Output: 1
```

 

**Constraints:**

- `1 <= n <= 9`

> The **queen** (♕, ♛) is the most powerful [piece](https://en.wikipedia.org/wiki/Chess_piece) in the game of [chess](https://en.wikipedia.org/wiki/Chess). It can move any number of squares vertically, horizontally or [diagonally](https://en.wikipedia.org/wiki/Glossary_of_chess#diagonal), combining the powers of the [rook](https://en.wikipedia.org/wiki/Rook_(chess)) and [bishop](https://en.wikipedia.org/wiki/Bishop_(chess)). Each player starts the game with one queen, placed in the middle of the first [rank](https://en.wikipedia.org/wiki/Glossary_of_chess#rank) next to the [king](https://en.wikipedia.org/wiki/King_(chess)).
>
> asymptotic /ˌæsɪmˈtɒtɪk/ ADJ of or referring to an asymptote.
>
> asymptote/ˈæsɪmˌtəʊt/ N a straight line that is closely approached by a plane curve so that the perpendicular distance between them decreases to zero as the distance from the origin increases to infinity.
>
> indices /ˈɪndɪsiːz/ Indices is a plural form of the word index.



## Intuition

We need to find all valid placements where no two queens can be in the same row, same column, or same diagonal, reminds us of **backtracking**. We'll try placing queens row by row. at each row, we try placing a queen in a column only if that column and its diagonals are not attacked by previous queens. If it's not valid, we backtrack and try a different position. If we successfully place `n` queens, we count it as one solution.

Next, let's consider some approaches:

1) **Brute-Force (Worst):** We could try placing `n` queens in all possible (n^2)^n positions and then validating each board. This is extremely inefficient and computationally infeasible for anything but tiny `n`.

2) **Backtracking with Sets or boolean arrays** (clear & readable)  
   - Place queens row by row. In each row, try placing a queen in a column only if that column and its diagonals are not already attacked by queens in previous rows. We can optimize the attack checks using sets or boolean arrays to quickly determine if a column or diagonal is occupied.

3) **Backtracking with bitmasks** (high-performance)  
   - For even more optimization, we can use bit manipulation to track occupied columns and diagonals; use bitwise operations to get candidates in O(1) per step and extract moves with LSB(Least Significant Bit) tricks.  

Let's start with the second approach, which offers a good balance of clarity and efficiency.

---

## Solution A — Backtracking Solution with `HashSet` (Deep Dive)

The core idea remains the same: a recursive function placing queens row by row. The difference is using `HashSet<Integer>` instead of boolean arrays to track occupied lines.

- `cols`: A `HashSet` storing the indices of columns that have a queen.
- `posDiag`: A `HashSet` storing identifiers for occupied positive diagonals. The identifier `row - col` is constant along these diagonals.
- `negDiag`: A `HashSet` storing identifiers for occupied negative diagonals. The identifier `row + col` is constant along these diagonals.

The `solve(row)` function will iterate through `col` from 0 to `n-1`. For a square `(row, col)`, it checks:

1. Does `cols` contain `col`?
2. Does `posDiag` contain `row - col`?
3. Does `negDiag` contain `row + col`?

If none are present, the square is safe. We add `col`, `row - col`, and `row + col` to their respective sets, call `solve(row + 1)`, and upon return, we remove these elements from the sets (backtracking).

```java
import java.util.HashSet;
import java.util.Set;
class Solution {
    // First, we create a variable to store the final count of distinct solutions.
    private int count;
    // Next, we create a HashSet to store the indices of columns that are currently occupied by a queen.
    private Set<Integer> cols;
    // Then, we create a HashSet to store identifiers for occupied positive diagonals (top-left to bottom-right). The identifier we use is 'row - col', as it's constant along these diagonals.
    private Set<Integer> posDiag;
    // Following that, we create another HashSet to store identifiers for occupied negative diagonals (top-right to bottom-left). The identifier we use is 'row + col', as it's constant along these diagonals.
    private Set<Integer> negDiag;
    /**
     * This is the main public method. It sets up the required data structures and starts the recursive backtracking process.
     */
    public int totalNQueens(int n) {
        // First, we initialize the solution count to zero and the HashSets used for tracking occupied columns and diagonals.
        cols = new HashSet<>();
        posDiag = new HashSet<>();
        negDiag = new HashSet<>();
        // Then, we begin the recursive placement starting from the first row (row 0).
        solve(0, n);
        // After exploring all possibilities, return the final count.
        return count;
    }
    /**
     * Alright, let's implement the slove method. This is the core recursive backtracking function. It attempts to place a queen in the specified 'row'.
     */
    private void solve(int row, int n) {
        // First, we'll implement the base case for the recursion: if the current row index equals 'n', it means we have successfully placed queens in all rows from 0 to n-1.
        if (row == n) {
            // Since we found a valid solution, so we increment our counter.
            count++;
            // Then, we stop exploring this path further and return.
            return;
        }
        // If the current row index is less than 'n', we iterate through all possible columns in the current 'row'.
        for (int col = 0; col < n; col++) {
            // For each column, we calculate the identifiers for the positive and negative diagonals for the current square (row, col).
            int posDiagId = row - col;
            int negDiagId = row + col;
            // Now, we check if placing a queen at (row, col) is safe.
            // First, we check if the current column 'col' is already in the 'cols' set.
            // Second, we check if the current positive diagonal identifier 'posDiagId' is already in the 'posDiag' set.
            // Third, we check if the current negative diagonal identifier 'negDiagId' is already in the 'negDiag' set.
            if (cols.contains(col) || posDiag.contains(posDiagId) || negDiag.contains(negDiagId)) {
                // If any of these checks are true, the square is under attack, so we cannot place a queen here. We skip this column and continue to the next iteration of the loop.
                continue;
            }
            // If the square is safe, we "place" the queen by adding the column and diagonal identifiers to our sets.
            cols.add(col);
            posDiag.add(posDiagId);
            negDiag.add(negDiagId);
            // We then recursively call the 'solve' method to try placing a queen in the next row.
            solve(row + 1, n);
            // After the recursive call returns, it means we have finished exploring all possibilities that started with placing a queen at (row, col).
            // We must now "remove" the queen by removing the corresponding identifiers from our sets.
            // This backtracking step allows us to try placing the queen in the next column of the current 'row'.
            cols.remove(col);
            posDiag.remove(posDiagId);
            negDiag.remove(negDiagId);
        }
    }
}
```

### Complexity Analysis

Okay, that's our implementation. Let's run it against the test cases. Yes! It got accepted. let's break down the time and space complexity.

- **Time Complexity consist of 2 factors**
    - First, the number of potential valid placements explored is bounded by permutations. We have `N` choices for the first row, at most `N-1` for the second, and so on.
    - Second, the `contains`, `add`, and `remove` operations on `HashSet` take average O(1) time.
    - Therefore, the overall time complexity remains roughly **O(N!)**. While `HashSet` operations might have slightly higher constant factors than direct array access, the asymptotic complexity is the same.
- **Space Complexity is made up of 2 factors**
    - First, we use three `HashSet`s. In the worst case, each set might store up to `N` elements (though diagonals technically store up to `2N-1` distinct values, it's still O(N)).
    - Second, the recursion stack depth goes up to `N`.
    - Therefore, the auxiliary space complexity is dominated by the sets and the recursion stack, resulting in **O(N)**.

---

## Solution B — Backtracking with bitmasks (optimized & elegant)

Same DFS idea, but we compress the three constraint sets (columns, main diagonals, anti-diagonals) into **bitmasks**. At row `r`, a `1` bit means “blocked,” `0` means “free.” The neat trick is:

- `cols` has `1`s where columns are occupied.
- `diags` tracks main diagonals; when we go to the next row, we left-shift `diags` by 1 to align conflicts for the next row.
- `antiDiags` tracks anti-diagonals; when we go to the next row, we right-shift `antiDiags` by 1.

Then we compute the set of free positions for this row as a mask:  
`available = ~(cols | diags | antiDiags) & allOnes`  
where `allOnes = (1 << n) - 1` keeps only the low `n` bits.

we iterate over the `1` bits in `available` using the standard LSB extraction:  
`pick = available & -available; available -= pick;`  
Each `pick` corresponds to choosing a column. The next row’s masks update as:  
`cols | pick`, `(diags | pick) << 1`, `(antiDiags | pick) >> 1`.

```java
class SolutionBitmask {
    // We're storing the resulting count across recursive calls to avoid passing it around.
    private int count;

    // We're exposing the main API: compute the number of N-Queens solutions using bitmask backtracking.
    public int totalNQueens(int n) {
        // We're resetting the solution counter at the start.
        count = 0;
        // We're creating a mask with the lowest n bits set to 1, to constrain operations within the board width.
        int allOnes = (1 << n) - 1;
        // We're starting the DFS with empty masks for columns and diagonals.
        backtrack(n, 0, 0, 0, allOnes);
        // We're returning the total count accumulated by the DFS.
        return count;
    }

    // We're defining a recursive helper that carries three bitmasks: columns, main-diagonals, anti-diagonals.
    private void backtrack(int n, int cols, int diags, int antiDiags, int allOnes) {
        // We're checking if all columns are filled (i.e., n queens placed), which happens when cols == allOnes.
        if (cols == allOnes) {
            // We're incrementing the count because a full valid placement has been achieved.
            count++;
            // We're returning to explore other bit choices.
            return;
        }

        // We're computing the set of available spots in the current row by removing blocked columns/diagonals.
        int available = allOnes & ~(cols | diags | antiDiags);
        // We're looping over each available position by repeatedly extracting the least significant 1-bit.
        while (available != 0) {
            // We're isolating the lowest 1-bit to choose one column for the queen in this row.
            int pick = available & -available;
            // We're removing that bit from the available set so we can try the next candidate later.
            available -= pick;
            // We're recursing to the next row, updating the masks:
            //   - columns: mark this column as used
            //   - diags: mark and then shift left to align conflicts for the next row
            //   - antiDiags: mark and then shift right to align conflicts for the next row
            backtrack(
                n,
                cols | pick,
                (diags | pick) << 1,
                (antiDiags | pick) >> 1,
                allOnes
            );
        }
    }
}
```

### Complexity (spoken)
- **Time:** Still exponential in `n`, bounded by the number of valid partial boards explored. Bit operations make candidate generation and conflict checks **O(1)** per move, so in practice it’s much faster than the boolean-array version.
- **Space:** O(n) recursion depth; the masks are just integers (constant extra space). Overall still linear in `n` for the stack.

**Trade-off:** Slightly trickier to read at first, but very fast and elegant. Great for production-grade performance on this problem.


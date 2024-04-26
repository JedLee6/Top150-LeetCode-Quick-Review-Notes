# 034_Matrix_36. Valid Sudoku

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/250px-Sudoku-by-L2G-20050714.svg.png)

```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

**Example 2:**

```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

 

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`.



## Intuition and Main Idea

Using a HashSet to record the number that was already found in the row, column and box.

If there are any same number in the row, column or box is already in the HashSet, then we have found the identical number, which resulted in an invalid sudoku board.

There are multiple approaches for the HashSet, either use separate HashSet for the rows, columns and boxes, or using String to include all the information into a single HashSet.

## Java Code

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set<String> set = new HashSet<>();
        for (int row = 0; row < 9; row++) {
            for (int column = 0; column < 9; column++) {
                char number = board[row][column];
                // If the position is a number (!= '.'), then we can try adding the number and its information into the HashSet.
                if (number != '.') {
                    // The HashSet.add() method returns false if the element being added already exists in the HashSet, or true otherwise
                    // As such, we can just use this boolean return from add() to check if we successfully added.     
                    // 1. Store the number in the row.
                    if (!set.add(number + " in row " + row) ||
                        // 2. Store the number in the column.
                        !set.add(number + " in column " + column) ||
                        // 3. Store the number with coordinate in the box, row and column divided by 3 are the coordinate of the box for this number
                        !set.add(number + " in block " + (row / 3) + "," + (column / 3))) {
                        // If any of the 3 part (row, column and box) is not added successfully, then 'board' is not a valid sudoku board.
                        return false;
                    }
                }
            }
        }
        // If all checks succeed, then the 'board' is a valid sudoku.
        return true;
    }
}
```

## Complexity Analysis

### Time Complexity : O(n^2),

where 'n' is 9, the length and width of the 'board'.
This is because we iterate through every number in 'board'.
Do note that the string concatenation take use some additional time, but they are constant time and do not scale linearly with 'n'.

### Space Complexity : O(n^2),

where 'n' is 9, the length and width of the 'board'.
This is due to the HashSet used, which stores the information of the number in the row, column and box.
Every row with every 9 numbers, likewise for every column and every box, thus, to be more precise, it is O (n^2 + n^2 + n^2).
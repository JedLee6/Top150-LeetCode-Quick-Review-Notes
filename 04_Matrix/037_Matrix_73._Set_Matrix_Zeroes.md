# 037_Matrix_73. Set Matrix Zeroes

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/mat1-20240428210549498.jpg)

```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/mat2-20240428210549602.jpg)

```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

 

**Follow up:**

- A straightforward solution using `O(mn)` space is probably a bad idea.
- A simple improvement uses `O(m + n)` space, but still not the best solution.
- Could you devise a constant space solution?



## Intuition and Main Idea

Leverage the matrix's first row and first column to keep track of which rows and columns should be zeroed out. This approach allows us to work entirely within the matrix and avoid using extra space.

## Method: Using First Row and First Column as Storage

Steps:

1. **Determine if the first row and first column need to be zeroed:** Before using them as storage, we need to check if they originally contain zeros.
2. **Use the first row and column to mark zeros:** Iterate through the matrix, and when a zero is found, mark the corresponding position in the first row and column.
3. **Zero out cells based on marks in the first row and column:** Go through the matrix again and use the marks to set cells to zero.
4. **Correct the first row and column:** Based on the initial check, zero out the first row and/or column if needed.

#### Implementation:
```java
public void setZeroes(int[][] matrix) {
    boolean firstRowZero = false;
    boolean firstColZero = false;
    int m = matrix.length;
    int n = matrix[0].length;
    // Step 1.1: Determine if the first column should be zero
    for (int i = 0; i < m; i++) {
        if (matrix[i][0] == 0) {
            firstColZero = true;
            break;
        }
    }
    // Step 1.2: Determine if the first row should be zero
    for (int j = 0; j < n; j++) {
        if (matrix[0][j] == 0) {
            firstRowZero = true;
            break;
        }
    }
    // Step 2: Use first row and column to mark zeros
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][j] == 0) {
                // all the element at i-th row or j-th column should be zero
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    // Step 3: Zero out cells based on marks
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }
    // Step 4: Handle the first row and first column
    if (firstColZero) {
        for (int i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
    if (firstRowZero) {
        for (int j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }
}
```

## Complexity Analysis:

- **Time Complexity:** \(O(m * n)\). We iterate over the entire matrix multiple times, but only a constant number of times.
- **Space Complexity:** \(O(1)\). We use the matrix itself for storage, thereby not requiring extra space beyond a few variables.

## Conclusion

This method leverages the matrix's own structure to store information about which rows and columns need to be zeroed, allowing us to achieve the task in-place with constant extra space. It's efficient and cleverly uses the given data structure to reduce space usage while maintaining clarity and efficiency. This approach demonstrates a deep understanding of both the problem constraints and potential optimizations.
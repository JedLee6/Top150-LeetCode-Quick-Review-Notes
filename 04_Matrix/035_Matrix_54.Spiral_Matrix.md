## 035_Matrix_54.Spiral Matrix

Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*. 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/spiral1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`



### Intuition and Main Idea

The idea is to use boundaries to manage the traversal of the matrix in a spiral form. We will use four boundaries:
- `top` (starts from the first row and moves downward)
- `bottom` (starts from the last row and moves upward)
- `left` (starts from the first column and moves rightward)
- `right` (starts from the last column and moves leftward)

The traversal is done in cycles:
1. Move from left to right along the top boundary, then move the top boundary one row down.
2. Move from top to bottom along the right boundary, then move the right boundary one column left.
3. Move from right to left along the bottom boundary, if applicable, then move the bottom boundary one row up.
4. Move from bottom to top along the left boundary, if applicable, then move the left boundary one column right.

Repeat this cycle until all elements are traversed.

### Solution: Spiral Order Traversal

Let's write the code for this with detailed explanations:

```java
public List<Integer> spiralOrder(int[][] matrix) {
    //Initialize the result list and handle edge cases
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    //Set initial boundaries for the top, bottom, left, and right
    int top = 0;
    int bottom = matrix.length - 1;
    int left = 0;
    int right = matrix[0].length - 1;
    while (top <= bottom && left <= right) {
        // `top` accessed inside the 1st for loop must be valid, as the while condition has already checked if `top <= bottom`, so there's no further check
        // Traverse from left to right along the top row
        for (int i = left; i <= right; i++) {
            //inside this iteration, top and i are guaranteed to be valid
            result.add(matrix[top][i]);
        }
        top++;
        // `right` accessed inside the 2nd for loop must be valid, as the while condition has already checked if `left <= right`, so there's no further check
        // Traverse downwards along the right column
        for (int i = top; i <= bottom; i++) {
            result.add(matrix[i][right]);
        }
        right--;
        // `top` has been changed after the 1st for loop, `bottom` accessed inside the 3rd for loop could be invalid, so need to check it
        if (top <= bottom) {
            // Traverse from right to left along the bottom row
            for (int i = right; i >= left; i--) {
                result.add(matrix[bottom][i]);
            }
            bottom--;
        }
        // `right` has been changed after the 2nd for loop, `left` accessed inside the 4th for loop could be invalid, so need to check it
        if (left <= right) {
            // Traverse upwards along the left column
            for (int i = bottom; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            left++;
        }
    }
    return result;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(m * n)\), where \(m\) is the number of rows and \(n\) is the number of columns. Each element is processed once.
- **Space Complexity:** \(O(1)\) excluding the output list, as no additional space is required for computation. If the output is considered, then it's \(O(m * n)\) because all elements are stored in the result.



### Why Checks Are Needed for the Latter Two Steps

The reason we include the checks "top <= bottom" and "left <= right" in the latter two scenarios of the traversal (right-to-left and bottom-to-top) is due to the nature of how the spiral traversal proceeds and collapses in on itself.

#### Spiral Matrix Traversal Details

In a spiral traversal, after each full cycle (all four sides of the current boundary), the dimensions of the traversal area are reduced:

1. **Top to Bottom:** Initially, we traverse from the left to the right along the top boundary and then increase the `top` boundary, because we have finished with that row.

2. **Right to Left:** Next, we move down the right side. After completing this, we decrease the `right` boundary, as we've finished with that column.

Now, before moving in the reverse direction (right-to-left and then bottom-to-top), we need to check if these movements are still valid within the updated boundaries:

3. **Bottom to Top (Right-to-Left on Bottom Row):** Before executing this step, we must ensure that our updated `top` (which has been incremented) hasn't surpassed the `bottom`. If `top > bottom`, it means that after moving downward along the right edge, there's no horizontal row available to move back from right to left, as the top part of the spiral has already converged or is converging with the bottom part.

   ```java
   if (top <= bottom) {
       for (int i = right; i >= left; i--) {
           result.add(matrix[bottom][i]);
       }
       bottom--;  // Reduce the bottom boundary as we've processed that row
   }
   ```

4. **Left to Right (Bottom-to-Top on Left Column):** Similarly, before moving up the left side, we need to confirm that `left <= right`. If `left > right`, there's no vertical column remaining to traverse from bottom to top as the left boundary has converged with or is converging with the right boundary.

   ```java
   if (left <= right) {
       for (int i = bottom; i >= top; i--) {
           result.add(matrix[i][left]);
       }
       left++;  // Move the left boundary inwards as we've processed that column
   }
   ```

#### Why Checks Are Not Needed for the Former Two Steps:

- **Left to Right (Top Row):** When traversing the top row from left to right, we just began the cycle, and there are no adjustments yet that could invalidate the traversal.
- **Top to Bottom (Right Column):** Similarly, when moving down along the right edge, since we've only adjusted the `top` boundary upwards and not yet modified the `bottom` or `left`, the full right column is valid to traverse.

#### Summary

These checks are critical for ensuring that we don't attempt to traverse boundaries that have already been processed or are out of the valid range due to the narrowing spiral. By including "top <= bottom" and "left <= right" in the latter parts of each cycle, we avoid accessing matrix elements outside of our intended range and ensure correctness in the spiral order output. This careful management of traversal boundaries guarantees that each element is processed exactly once and in the correct order.

### Alternative Solutions

1. **Recursive Solution:** A recursive approach could theoretically be used where you peel off the outer layer in each recursive call, but this would generally be less efficient due to overhead and more complex due to handling of base cases.
2. **Layer-by-Layer:** This approach would involve taking each "ring" of the matrix from the outermost to the innermost and appending it to the result. This can simplify handling of indices but may require more complex logic to identify and process each layer correctly.

The presented iterative solution using boundary adjustments is typically most straightforward and efficient for this problem. It avoids the pitfalls of more complex index management and recursive overhead.
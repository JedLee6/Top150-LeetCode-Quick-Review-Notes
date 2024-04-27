# 036_Matrix_48. Rotate Image

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/mat2.jpg)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

 

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`





### Intuition and Main Idea

This is a common problem that tests our understanding of array manipulation and the ability to transform a matrix geometrically without additional space.

When you rotate a matrix by 90 degrees clockwise, each element at position `(i, j)` moves to a new position `(j, n-1-i)`. Here, `n` is the dimension of the matrix. This new position indicates where elements will end up after a 90-degree rotation.

To achieve this:
1. **Transpose the Matrix:** First, transpose the matrix by swapping elements along the main diagonal. Thus, the element at position `(i, j)` in the original matrix moves to position `(j, i)` in its transpose. And the pseudo code is `swap(matrix[i][j], matrix[j][i])`. 

2. **Reverse Each Row:** After transposing, reversing each row of the matrix corresponds to flipping the elements horizontally. Element at `(j, i)` moves to `(j, n-1-i)`. This means the first element of each row goes to the last, the second to the second last, and so on. And the pseudo code is `swap(matrix[i][j], matrix[i][matrix.length-1-j])`. 


This is precisely the movement needed for a 90-degree clockwise rotation. The row reversal step after transposition effectively performs a vertical mirror transformation which is required to complete this specific rotational alignment.

### Solution: In-place Matrix Rotation

Here's how to implement this with detailed explanations:

```java
public class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // Step 1: Transpose the matrix by swapping elements along the main diagonal to convert columns into rows.
        for (int i = 0; i < n; i++) {
            // Iterates through the upper triangle of the matrix, which start j from i to avoid re-transposing
            for (int j = i; j < n; j++) {  
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // Step 2: Reversing each row of the matrix corresponds to flipping the elements horizontally
        for (int i = 0; i < n; i++) {
            // Only go up to half the row
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
            }
        }
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n^2)\). Each element is accessed a constant number of times, leading to a quadratic time complexity relative to the dimensions of the matrix.
- **Space Complexity:** \(O(1)\). The rotation is done in-place, using only a small constant amount of extra space for swapping elements.

### Alternative Solution: Layer-by-Layer Rotation

Another way to solve this problem is to rotate the matrix layer by layer, moving the four corners and then the elements along the edges in a cycle, adjusting the four sides of each layer:

1. Calculate and swap groups of four related elements directly.
2. Move to the next group in the current layer.
3. Move inward to the next inner layer and repeat until all layers are processed.

This approach is more complex to implement but still follows the \(O(n^2)\) time complexity due to handling each element a constant number of times and maintains \(O(1)\) space complexity because it also modifies the matrix in-place.

### Conclusion

The transpose-then-reverse method is usually simpler and clearer for most developers. It avoids the complex indexing and error-prone handling of multiple groups of elements simultaneously, making it a preferred choice in interviews unless specific constraints or requirements dictate otherwise.




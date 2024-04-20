## 028_Two Pointers_11. Container With Most Water

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

**Notice** that you may not slant the container.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/question_11.jpg)

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2:**

```
Input: height = [1,1]
Output: 1
```

**Constraints:**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`



### Understanding the Problem

The problem describes a scenario where we have a series of vertical lines on a 2D plane, and we want to find two lines that, along with the x-axis, will form a container that can hold the maximum amount of water. The width of the container is the distance between the two lines, and the height is the minimum height of the two lines (since the water would overflow from the shorter line otherwise).

This problem is often called the "Container With Most Water" problem. Given that the lines are represented by an array where the value at each index indicates the height of a line at that position, our task is to find two indices `i` and `j` such that the area between them is maximized. The area between two lines can be computed as:
\[ \text{Area} = (j - i) \times \min(\text{height}[i], \text{height}[j]) \]

### Intuitive Approach: Two-Pointer Technique

Given that the array is not sorted and we are looking to maximize an area based on two values and their distance, a brute-force solution would require checking all pairs, resulting in a time complexity of \( O(n^2) \). However, we can optimize this using a two-pointer approach due to the direct relationship between the width (distance between two pointers) and the height (minimum of the two heights).

#### Java Implementation

```java
public class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0; // Variable to store the maximum area found
        int left = 0; // Starting at the beginning of the array
        int right = height.length - 1; // Starting at the end of the array

        while (left < right) { // Continue until the two pointers meet
            int currentMinHeight = Math.min(height[left], height[right]); // Minimum of the two heights
            int currentWidth = right - left; // Distance between the two pointers
            int currentArea = currentMinHeight * currentWidth; // Calculate the area of the current container

            maxArea = Math.max(maxArea, currentArea); // Update maxArea if the current area is larger

            // Move the pointer pointing to the shorter line inward
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return maxArea; // Return the maximum area found
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where `n` is the number of elements in the array. Each element is considered once as the pointers move inward.
- **Space Complexity**: O(1), as the solution uses only a few extra variables (pointers and area calculations), and does not depend on the size of the input array.

### Conclusion

The two-pointer technique is highly efficient for this problem because it systematically narrows down the possible maximum area by eliminating scenarios that cannot surpass a current maximum. This approach ensures that we only traverse the list once, thus maintaining a linear time complexity, which is optimal for this problem.
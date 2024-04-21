## 030_Sliding_Window_209. Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`, return the **minimal length** of a subarray whose sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

> subarray /'sʌbərei/

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

 

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

 

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.



### Intuitive Approach

The problem suggests a need for a method to explore contiguous subarrays. A brute-force method would involve calculating the sum for every possible subarray and keeping track of the smallest subarray that meets or exceeds the target sum, whose time complexity is O(n^3). However, this approach is inefficient.

### Optimized Approach: Sliding Window Technique

A more efficient approach would be to use the sliding window technique. This method involves using two pointers to create a window which can expand or contract based on the sum of its elements relative to the target. This technique is particularly suitable because it ensures that each element is processed once, thereby optimizing the operation.

### Solution Development in Java

```java
public class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLength = Integer.MAX_VALUE; // Initialize to max value to ensure any valid subarray found will be smaller
        int sum = 0; // This will store the sum of the current window
        int start = 0; // Starting index of our sliding window
        // Window Expansion: Iterate through the array with the end pointer, adding each element to sum.
        for (int end = 0; end < nums.length; end++) {
            sum += nums[end]; // Expand the window by adding the current element
            // Window Contraction: While the sum of the window is greater than or equal to the target, try to shrink the window from the left to see if we can get a smaller length
            while (sum >= target) {
                minLength = Math.min(minLength, end - start + 1); // Update the minimum length whenever a smaller valid window is found
                sum -= nums[start]; // Reduce the window size by moving the start pointer
                start++;
            }
        }
        // If minLength was updated, return it; otherwise, return 0
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

### Complexity Analysis

- **Time Complexity**: (O(n)), where (n) is the number of elements in `nums`. Each element is processed at most twice (once by the `end` pointer and once by the `start` pointer).
- **Space Complexity**: (O(1)), since only a constant amount of additional space is used beyond the input.

### Conclusion

The sliding window technique is highly effective for this problem, as it efficiently finds the smallest subarray that meets the condition without needing to reevaluate elements multiple times. This approach significantly reduces the computational complexity compared to a brute-force approach and elegantly handles the problem constraints.



### Follow up

If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

For an alternative solution with a time complexity of \(O(n \log n)\), we can employ a different approach based on binary search combined with prefix sums. This method is especially effective when we need to find optimal subarray lengths using accumulated sums and searching techniques.

### Approach: Binary Search on Length with Prefix Sums

This method revolves around using a binary search on the length of the subarray. We can generate prefix sums to quickly calculate the sum of any subarray in constant time. The binary search will help determine the smallest subarray length that meets the criteria.

### Step-by-Step Explanation and Java Implementation

1. **Generate Prefix Sums**: Calculate the prefix sum array such that `prefix[i]` is the sum of elements from `nums[0]` to `nums[i]`.
2. **Binary Search on Length**: Use binary search to find the minimal length of the subarray that has a sum greater than or equal to the target.

#### Java Code

```java
public class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] prefixSums = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSums[i] = prefixSums[i - 1] + nums[i - 1];
        }
        int minLength = Integer.MAX_VALUE;
        // Binary search on the length of the subarray
        int left = 1, right = n;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (windowExists(mid, prefixSums, target)) {
                minLength = mid;
                right = mid - 1;  // Try to find a smaller length
            } else {
                left = mid + 1;
            }
        }
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
    private boolean windowExists(int length, int[] prefixSums, int target) {
        for (int i = length; i < prefixSums.length; i++) {
            if (prefixSums[i] - prefixSums[i - length] >= target) {
                return true;
            }
        }
        return false;
    }
}
```

#### Code Explanation

- **Prefix Sum Array**: We first build a prefix sum array to help calculate any subarray sum in \(O(1)\).
- **Binary Search**: We binary search the length of the subarray from 1 to `n`. For each length `mid`, we check if a subarray of this length exists that meets or exceeds the target sum.
- **Window Existence Check**: `windowExists` checks if any subarray of length `length` has a sum greater than or equal to `target` by iterating through the prefix sums.

### Complexity Analysis

- **Time Complexity**: \(O(n log n)\). The binary search takes \(O(log n)\), and for each mid value, we potentially scan the array in \(O(n)\) to check if a valid window exists.
- **Space Complexity**: \(O(n)\) due to the storage of the prefix sums array.

### Conclusion

This approach is a bit more complex than the sliding window method but showcases a different technique involving prefix sums and binary search. It can be particularly educational for understanding how different algorithmic techniques can be applied to the same problem to achieve similar efficiencies. This method is also useful in scenarios where you might need to adjust the conditions dynamically or when working with immutable data structures where sliding window modifications are not feasible.




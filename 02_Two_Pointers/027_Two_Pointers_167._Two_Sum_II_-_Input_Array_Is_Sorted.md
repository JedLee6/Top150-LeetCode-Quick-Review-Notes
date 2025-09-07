## 027_Two Pointers_167. Two Sum II - Input Array Is Sorted

Given a **1-indexed** array of integers `numbers` that is already ***sorted in non-decreasing order\***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return *the indices of the two numbers,* `index1` *and* `index2`*, **added by one** as an integer array* `[index1, index2]` *of length 2.*

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space. 

**Example 1:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

**Example 2:**

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

**Example 3:**

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

**Constraints:**

- `2 <= numbers.length <= 3 * 104`
- `-1000 <= numbers[i] <= 1000`
- `numbers` is sorted in **non-decreasing order**.
- `-1000 <= target <= 1000`
- The tests are generated such that there is **exactly one solution**.



### Problem Understanding and Approach

The problem presented is a classic "two sum" problem but with a twist: the array is already sorted in non-decreasing order. This sorted nature of the array allows us to use efficient approaches that leverage the order of the elements.

Given the constraints and requirements:
1. We need to find two numbers in the array that sum up to a given target.
2. We need to return the indices of these numbers, adjusted to be 1-indexed.
3. The array is sorted, which allows the use of a two-pointer technique.

### Solution using the Two-pointer Technique

**Intuition**: Since the array is sorted, we can use two pointers—one starting from the beginning (left) and the other from the end (right) of the array. By comparing the sum of these two elements to the target, we can decide to move the pointers inward until we find the exact pair that sums up to the target.

> inward /ˈɪnwəd/ 
>
> adj. Your inward thoughts or feelings are the ones that you do not express or show to other people.
>
> adv. If something moves or faces inward, it moves or faces toward the inside or centre of something.

#### Step-by-Step Java Implementation:

```java
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0; // Start pointer at the beginning of the array
        int right = numbers.length - 1;  // Start pointer at the end of the array
        while (left < right) {  // Ensure pointers do not cross
            int sum = numbers[left] + numbers[right];  // Calculate sum of two pointed elements
            if (sum == target) {  // Check if the sum is equal to the target
                return new int[] {left + 1, right + 1};  // Return 1-indexed position
            } else if (sum < target) {  // If sum is less than target, move the left pointer to the right
                left++;
            } else {  // If sum is greater than target, move the right pointer to the left
                right--;
            }
        }
        return new int[] {-1, -1};  // Return a default value (not needed as per problem statement)
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where `n` is the number of elements in the array. Each element is visited at most once by either the `left` or the `right` pointer.
- **Space Complexity**: O(1), as the solution only uses two extra variables for the pointers, regardless of the input size.

### Conclusion

The two-pointer approach is ideal for this problem due to the sorted nature of the array. It efficiently finds the two numbers in a single pass through the array while maintaining a constant space complexity. This method leverages the order to eliminate the need for additional space for data structures like hash tables, which are typically used in the general "two sum" problem. This approach is optimal and meets all given constraints.
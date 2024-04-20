## 029_Two Pointers_15. 3Sum

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

 

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

 

**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

### Problem Understanding

The problem requires finding all unique triplets in an array that add up to zero. This is a common type of problem known as "3-sum", which is a specific case of the more general "n-sum" problem. The constraints that the triplets must be unique and that the indices must be distinct complicate the solution slightly, especially when trying to avoid duplicates efficiently.

### Solution Strategy

To solve this problem, I’ll discuss two approaches: a brute-force method and an optimized two-pointer approach.

#### 1. Brute Force Method

This involves checking all possible combinations of three different elements in the array to see if they sum to zero.

##### Java Implementation

```java
import java.util.*;
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>(); // To avoid duplicates
        int n = nums.length;
        Arrays.sort(nums); // Sorting to make it easier to avoid duplicates
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        result.add(triplet); // HashSet takes care of duplicates
                    }
                }
            }
        }
        
        return new ArrayList<>(result); // Convert set to list
    }
}
```

##### Complexity Analysis
- **Time Complexity**: \(O(n^3)\) due to the three nested loops.
- **Space Complexity**: \(O(n)\) for the hash set used to store the results (in the worst case, all elements could form valid triplets).

#### 2. Two-Pointer Technique

An optimized approach that uses sorting and two pointers to find triplets efficiently.

##### Java Implementation

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // Sort the array to help manage duplicates and use two-pointer technique
        Arrays.sort(nums); 
        List<List<Integer>> res = new ArrayList<>();
        //Loop through each number, but only consider numbers ≤ 0 as possible first elements of the triplet
        for (int i = 0; i < nums.length && nums[i] <= 0; ++i)
            // "i == 0" is for avoiding ArrayIndexOutOfBoundsException: Index -1 out of bounds
            if (i == 0 || nums[i - 1] != nums[i]) { // Skip duplicates for the first element
                twoSumII(nums, i, res);
            }
        return res;
    }

    void twoSumII(int[] nums, int i, List<List<Integer>> res) {
        int left = i + 1, right = nums.length - 1;
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum < 0) {
                left++;
            } else if (sum > 0) {
                right--;
            } else {
                res.add(Arrays.asList(nums[i], nums[left++], nums[right--]));
                // Avoid ArrayIndexOutOfBoundsException and Skip duplicates for the second and third element
                while (left < right && nums[left] == nums[left - 1])
                    ++left;
            }
        }
    }
}
```

##### Complexity Analysis
- **Time Complexity**: \(O(n^2)\), where sorting takes \(O(n * log n)\) and the two-pointer scan takes \(O(n^2)\).
- **Space Complexity**: \(O(n)\) for the result list, or \(O(log n)\) if considering only the space needed for the sorting algorithm.

### Conclusion

The two-pointer technique is significantly more efficient for this problem, especially with large datasets, due to the reduced time complexity and effective handling of duplicates. This method leverages sorting and a systematic approach to skip unnecessary elements, thus ensuring optimal performance.
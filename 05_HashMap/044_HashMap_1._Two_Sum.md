#  044_HashMap_1. Two Sum

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have **exactly one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

 

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

 

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

 

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?



**Intuition and Main Idea:**

To find two numbers in the array that add up to the target, we can use a hashmap to store the complement of each number needed to reach the target. As we iterate through the array, we check if the current number's complement (i.e., target - current number) exists in the hashmap. If it does, we have found the pair of numbers that add up to the target.

**Solution: Using HashMap**

```java
public int[] twoSum(int[] nums, int target) {
        // Initialize a hashmap to store the complement of each number and its index
        Map<Integer, Integer> map = new HashMap<>();
        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            // Check if the complement exists in the hashmap
            if (map.containsKey(complement)) {
                // If yes, return the indices of the current number and its complement
                return new int[] { map.get(complement), i };
            }
            // Otherwise, put the current number and its index into the hashmap
            map.put(nums[i], i);
        }  
        // If no solution is found, return an empty array
        return new int[0];
    }
```

**Time Complexity Analysis:** O(n)

Let \( n \) be the number of elements in the array.

- The time complexity of iterating through the array is \( O(n) \).
- Each lookup and insertion in the hashmap takes \( O(1) \) time on average.
- Therefore, the overall time complexity of this solution is \( O(n) \).

**Space Complexity Analysis:** O(n)

- The space complexity is \( O(n) \) because we may store up to \( n \) entries in the hashmap.
# 048_Intervals_228. Summary Ranges

You are given a **sorted unique** integer array `nums`.

A **range** `[a,b]` is the set of all integers from `a` to `b` (inclusive).

Return *the **smallest sorted** list of ranges that **cover all the numbers in the array exactly***. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.

Each range `[a,b]` in the list should be output as:

- `"a->b"` if `a != b`
- `"a"` if `a == b`

 

**Example 1:**

```
Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

**Example 2:**

```
Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

 

**Constraints:**

- `0 <= nums.length <= 20`
- `-231 <= nums[i] <= 231 - 1`
- All the values of `nums` are **unique**.
- `nums` is sorted in ascending order.



### Problem Understanding

The task is to generate a list of string representations for ranges of consecutive numbers found in a sorted and unique integer array `nums`. Each range can be represented in two ways:
- As a single number (`"a"`) if the start and end of the range are the same.
- As a range (`"a->b"`) if the start (`a`) and end (`b`) of the range are different.

### Intuition and Main Idea

Given that the array is sorted and contains unique integers, we can leverage these properties to efficiently determine the ranges:
1. Traverse the array while keeping track of the start of the current range.
2. As you progress, if the next number in the array is not consecutive to the current one, close off the current range and start a new one.
3. If it is consecutive, continue until it's not, then start the next range.

### Solution with Detailed Annotations

Let's walk through the Java solution, explaining each part:

```java
public List<String> summaryRanges(int[] nums) {
    List<String> result = new ArrayList<>(); // List to store the final range strings
    if (nums.length == 0) {
        return result; // Return empty list if input array is empty
    }
    // Initialize the start of the first range
    int start = nums[0]; 
    //Iterates over the array to find where ranges start and end.
    for (int i = 1; i < nums.length; i++) { // Start from the second element
        // If current element is not consecutive
        // Checks if the current number is not consecutive to the previous one. If so, close the current range and add it to the list
        if (nums[i] != nums[i - 1] + 1) {
            // Complete the current range and add it to result
            // If start is equal to `nums[i - 1]`, it's a single number range
            if (start == nums[i - 1]) {
                result.add(String.valueOf(start)); // Single number range
            } else {
                result.add(start + "->" + nums[i - 1]); // Multiple number range from start to the previous number
            }
            start = nums[i]; // Start a new range
        }
    }
    // Add the last range after finishing the loop
    if (start == nums[nums.length - 1]) {
        // If start is equal to `nums[i - 1]`, it's a single number range, or a multiple number range
        result.add(String.valueOf(start));
    } else {
        result.add(start + "->" + nums[nums.length - 1]);
    }
    return result; // Return the list of ranges
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in `nums`. The function iterates through the array a single time, processing each element once to determine if it belongs to the current range or starts a new one.
- **Space Complexity**: O(m), where m is the number of distinct ranges generated. In the worst case (when no two consecutive numbers are the same), this could be as large as n (each number is its own range).

This method efficiently identifies and formats ranges in a sorted, unique array by taking advantage of its sorted nature to minimize checks and simplify logic.
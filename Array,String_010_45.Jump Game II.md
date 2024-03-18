## Array,String_010_45.Jump Game II

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return *the minimum number of jumps to reach* `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.

## Intuition :

- We have to find the minimum number of jumps required to reach the end of a given array of non-negative integers i.e the shortest number of jumps needed to reach the end of an array of numbers.

## Explanation to Approach :

- We are using a search algorithm that works by moving forward in steps and counting each step as a jump.
- The algorithm keeps track of the farthest reachable position at each step and updates the number of jumps needed to reach that farthest position.
- The algorithm returns the minimum number of jumps needed to reach the end of the array.

## Complexity :

- Time complexity : O(n)

- Space complexity: O(1)

## Codes

```java
class Solution {
  public int jump(int[] nums) {
    int ans = 0;
    int end = 0;
    int farthest = 0;
    // Implicit BFS, BFS stands for Breadth-First Search, which is a fundamental and widely used algorithm in computer science for traversing or searching tree or graph data structures.
    for (int i = 0; i < nums.length - 1; ++i) {
      farthest = Math.max(farthest, i + nums[i]);
      if (farthest >= nums.length - 1) {
        ++ans;
        break;
      }
      // Visited all the items on the current level
      if (i == end) {
        // Increment the level
        ++ans;     
        // Make the queue size for the next level
        end = farthest; 
      }
    }
    return ans;
  }
}
```
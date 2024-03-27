## Array,String_09_55.Jump Game

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise*.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

## Intuition

The basic idea is this: at each step, we keep track of the furthest reachable index. The nature of the problem (eg. maximal jumps where you can hit a range of targets instead of singular jumps where you can only hit one target) is that for an index to be reachable, each of the previous indices have to be reachable.

> The nature of something is its basic quality or character.
>
> suffice /səˈfaɪs/ If you say that something will <b>suffice</b>, you mean it will be enough to achieve a purpose or to fulfil a need.

Upon analyzing the problem and solution, my initial thought was to consider the possible jumps from each position and track the maximum reachable index. The provided solution suggests a greedy approach, where we iteratively update the maximum reachable index and check if it's sufficient to reach the last index. Therefore, my intuition for solving this problem aligns with a greedy strategy that focuses on efficiently determining if the last index can be reached based on the maximum jump lengths at each position.

## Approach

The approach taken here is based on a greedy strategy. We iterate through the array, maintaining the maximum reachable index (`reach`). At each position, we update `reach` by considering the maximum jump length at the current position and adding it to the current index. By continuously updating `reach` as we traverse the array, we determine whether it's possible to reach the last index. If `reach` surpasses or equals the index of the last element, we can conclude that reaching the last index is possible.

1. Initilize variable reach as 0, to store reach of the highest index.

```java
int reach = 0;
```

2. Iterate the nums and check if reach is smaller than i then return false else overwrite reach with max of reach and i+nums[i].

```java
for(int i=0;i<nums.length;i++){
    if(reach<i){
        return false;
    }
    reach = Math.max(reach,i+nums[i]);
}
```

3. return true, beacause we reach the last value of the array nums.

```java
return true;
```

## Complexity

- Time complexity: O(n) where n is the number of elements in the `nums` array. The algorithm iterates through the array once, performing constant-time operations at each step.

- Space complexity: O(1) The algorithm utilizes only a constant amount of extra space, independent of the size of the input array.

## Code

```java
class Solution {
    public boolean canJump(int[] nums) {
        int reach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > reach) return false;
            reachableIndex = Math.max(reach, i + nums[i]);
        }
        return true;
    }
}
```
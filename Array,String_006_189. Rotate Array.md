## Array,String_06_189. Rotate Array

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative. 

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Example 2:**

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

**Follow up:**

- Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
- Could you do it in-place with `O(1)` extra space?

## Intuition

Reversing the entire array effectively shifts the elements that need to move to the rightmost positions.
Reversing the first k elements and then reversing the remaining elements correctly positions those elements within their respective segments, completing the rotation.

## Approach

Edge Case Handling: The code checks for an empty array and returns early, as there's nothing to rotate.
Modulo Operation: It calculates k % n to ensure k is within the array's bounds, handling cases where k might be larger than the array length.

Three Reversals:

1. reverse(nums, 0, n-1): Reverses the entire array.
2. reverse(nums, 0, k-1): Reverses the first k elements of the already-reversed array, effectively placing them in their final positions.
3. reverse(nums, k, n-1): Reverses the remaining elements (from index k to n-1), completing the rotation.

## Complexity

- Time complexity: O(n)

- Space complexity: O(1)

## Code

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int modK = k % nums.length;
        // Method1: Reverse 2 sub-array first, then reverse whole array 
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, modK - 1);
        reverse(nums, modK, nums.length - 1);
        // Method2: reverse whole array first, then reverse 2 sub-array 
        // reverse(nums, 0, nums.length - modK - 1);
        // reverse(nums, nums.length - modK, nums.length - 1);
        // reverse(nums, 0, nums.length - 1);
    }
    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

## Detailed Explanation

Alright, what the question is saying that we have **given an array & we have to rotate the array to the right by k steps, where k is non-negative.**

```python
Okay so, we have one thing that k will always be > 0.
But, I will teach you, one bonus point as well what if k < 0 i.e. k is -ve then how can you rotate the array.
```

Let's undertsand this problem using an example,
**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]

```csharp
"K all possible rotation"
[7,1,2,3,4,5,6], k = 1
[6,7,1,2,3,4,5], k = 2
[5,6,7,1,2,3,4], k = 3
[4,5,6,7,1,2,3], k = 4
[3,4,5,6,7,1,2], k = 5
[2,3,4,5,6,7,1], k = 6
[1,2,3,4,5,6,7], k = 7
```

We have **k is 3**, so it means we have to take **3 values from the back** and **put in the front** of the array values.

So, for that what we will do is, we will break Array into 2 parts. **Part1[P1] & Part2[P2]**

- `[P1] is defined as` the array part just before the last 3 values. What I mean is something like [1,2,3,4]
- `[P2] is defined as` the array part just after remaining values which we have to rotate [5,6,7]

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/bafbf0fc-198b-4fe2-b4e9-974330840daf_1643513368.0827003.png)

So, in order to rotate this Array **k times** what we will do is, we will reverse the **P1 first which become [4,3,2,1]** & then we **reverse P2 which becomes [7,6,5]**
Now finally what we have to do is we gonna **reverse the complete array** by doing that what will happen is our array become **[5,6,7,1,2,3,4]** and that's what we want in our **Output**

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/b303db4f-a485-41ef-9b0a-1e5141c634d3_1643514374.4636352.png)

But, what if we have **k = 101**, then we will **not rotate it** 101 times. It simply means **till 100 times it will be [1,2,3,4,5,6,7]** & we have to **rotate only 1 time i.e. [7,1,2,3,4,5,6]**. So, now your question is how can we handle this, we simply do the **modulo of "k"** with length of array

```csharp
Okay Bonus point what if we have k = -1, then how can we rotate the array. If k is -1 then we have to rotate the value backward not in the front.
Eg - 
Input : [1,2,3,4,5,6,7], k = -1
Output : [2,3,4,5,6,7,1]

Now how did we figure out this, if you carefully look that k = -1 is equals to k = 6.
Just look at the table which I have made for every possible k values

So, what It represent is that add the -ve value to the length of array. And you will get your answer!
```


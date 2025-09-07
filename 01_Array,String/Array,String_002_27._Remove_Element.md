## Array,String_02_27. Remove Element

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

> In [computer science](https://en.wikipedia.org/wiki/Computer_science), an **in-place algorithm** is an [algorithm](https://en.wikipedia.org/wiki/Algorithm) that operates directly on the input [data structure](https://en.wikipedia.org/wiki/Data_structure) without requiring extra space proportional to the input size. In other words, it modifies the input in place, without creating a separate copy of the data structure. An algorithm which is not in-place is sometimes called **not-in-place** or **out-of-place**.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

 

**Example 1:**

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

 

**Constraints:**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`



## Intuition

The intuition behind this solution is to iterate through the array and keep track of two pointers: `index` and `i`. The `index` pointer represents the position where the next non-target element should be placed, while the `i` pointer iterates through the array elements. By overwriting the target elements with non-target elements, the solution effectively removes all occurrences of the target value from the array.

## Approach

1. Initialize `index` to 0, which represents the current position for the next non-target element.
2. Iterate through each element of the input array using the `i` pointer.
3. For each element `nums[i]`, check if it is equal to the target value.
   - If `nums[i]` is not equal to `val`, it means it is a non-target element.
   - Set `nums[index]` to `nums[i]` to store the non-target element at the current `index` position.
   - Increment `index` by 1 to move to the next position for the next non-target element.
4. Continue this process until all elements in the array have been processed.
5. Finally, return the value of `index`, which represents the length of the modified array.

## Complexity

- Time complexity: O(n)

- Space complexity: O(1)

## Code

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int notValItemIndex = 0;
        for (int pointer = 0; pointer < nums.length; pointer++) {
            if (nums[pointer] != val) {
                nums[notValItemIndex] = nums[pointer];
                notValItemIndex++;
            }
        }
        return notValItemIndex;
    }
}
```
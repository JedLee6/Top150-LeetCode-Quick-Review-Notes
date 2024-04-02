## Array,String_013_238.Product of Array Except Self

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

> Product is a quantity obtained by multiplying several numbers. The product of 21 and 16 is 336.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Constraints:**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

## Intuition

The problem requires computing the product of all elements in an array except the element at the current index. A common approach is to use prefix and suffix products. The prefix product for an index `i` is the product of all elements before `i`, and the suffix product is the product of all elements after `i`.

## Approach
### 1 Brute Force

the simplest method that comes to mind is, I can loop through the complete array using a pointer, say `j`, for every index `i`, and multiply all the elements at index `j` except when `i == j`. This would ensure that I skip the element at index `i` from being multiplied.

Time Complexity : O(N^2), Where N is the size of the Array(nums). Here Two nested loop creates the time complexity.

Space complexity : O(1), Constant space. Extra space is only allocated for the Array(output), however the output does not count towards the space complexity.


```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int numsLength = nums.length;
        int[] answer = new int[numsLength];
        for (int i = 0; i < numsLength; i++) {
            int product = 1;
            for (int j = 0; j < numsLength; j++) {
                if (i == j) continue;
                product *= nums[j];
            }
            answer[i] = product;
        }
        return answer;
    }
}
```

### 2 Dynamic Programming Approach(Tabulation, Space Optimization)

To adhere to the constraint of not using division and achieving (O(n)) time complexity, we can use a two-pass approach that involves prefix and suffix products.

Directly store the product of prefix and suffix into the final answer array.

1. Initialize an array `ans` to store the final product for each index.
2. Compute the prefix products and store them in the `ans` array. Start from the leftmost element and multiply the running product by the element at the current index.
3. Compute the suffix product while updating the `ans` array. Start from the rightmost element, multiply the running product by the element at the current index, and update the `ans` array by multiplying it with the current suffix product.

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
      	//Initialize an array to store the final product for each index.
        int n = nums.length;
        int ans[] = new int[n];
      	//set each element as 1, as to multiply every prefix product and suffix product by itself
        Arrays.fill(ans, 1);
      	//set currentPrefixProduct as 1
        int currentPrefixProduct = 1;
      	//Calculate Prefix Products
        for(int i = 0; i < n; i++) {
            ans[i] *= currentPrefixProduct;
          	//update the prefix product for the next element
            currentPrefixProduct *= nums[i];
        }
	      //set currentSuffixProduct to 1
        int currentSuffixProduct = 1;
	      //Calculate Suffix Products
        for(int i = n - 1; i >= 0; i--) {
            ans[i] *= currentSuffixProduct;
          	//update the suffix product for the next element
            currentSuffixProduct *= nums[i];
        }
        return ans;
    }
}
```

Time Complexity : O(N), As we iterate the Array(nums) twice. Where N = size of the array.

Space complexity : O(1), Constant space. Extra space is only allocated for the Array(output), however the output does not count towards the space complexity.

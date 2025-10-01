## 103_Backtracking_46._Permutations



Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

 

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

 

> permutation N. A <b>permutation</b> is one of the ways in which a number of things can be ordered or arranged.

### Intuition

Alright, the problem asks for all possible permutations of a given array of distinct integers. The word **"permutations"** is key, as it means the order of elements matters.

When we see a problem asking for "all possible" arrangements or subsets, our mind immediately goes to **backtracking**. It's a perfect fit for systematically exploring every possible configuration.

My mental model is to build each permutation step by step. Imagine we have the numbers `[1, 2, 3]`.

1. For the first position, I can choose `1`, `2`, or `3`.
2. If I choose `1` for the first position, then for the second position, I can choose from the *remaining* numbers, `2` or `3`.
3. If I then choose `2`, the only number left for the third position is `3`, giving us the permutation `[1, 2, 3]`.

The core of the algorithm is this process: **choose, explore, and then un-choose (backtrack)**. We choose a number for the current position, recursively explore the possibilities for the next position, and then we backtrack by removing that number to try a different choice for the current position. To avoid using the same number twice in one permutation, we'll need to keep track of which numbers have already been included.

In terms of different solutions:

- **Brute-Force (Worst):** We could try to generate all possible sequences of length `n` and then filter out the ones that aren't valid permutations (i.e., contain duplicate numbers). This would be highly inefficient.
- **Backtracking (Best):** This is the standard and most efficient approach. It systematically builds only valid permutations, avoiding redundant work. Given the small constraints (`nums.length <= 6`), an exponential time complexity solution like this is expected and perfectly fine.

We'll proceed with the backtracking approach as it's the clean and optimal way to solve this.

------



### Code Walkthrough

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    /**
     * Okay, this is the main public method that the user will call.
     * Its job is to set up the necessary data structures and start the recursive process.
     */
    public List<List<Integer>> permute(int[] nums) {
        // First, we create a list to store all the final permutations.
        List<List<Integer>> result = new ArrayList<>();
        // Next, we create a boolean array to keep track of which numbers from the input array have been used in the current permutation. The index of this array will correspond to the index in the 'nums' array.
        boolean[] used = new boolean[nums.length];
        // Then, we'll kick off our recursive helper method, which we'll call 'backtrack'.
        // We'll pass it the final result list, a new empty list to build the current permutation, the original numbers, and our 'used' tracker.
        backtrack(result, new ArrayList<>(), nums, used);
        // And finally, after the recursion has explored all possibilities, we return the populated list of results.
        return result;
    }
    /**
     * Now, let's implement the recursive backtrack method. It builds the permutations step by step.
     */
    private void backtrack(List<List<Integer>> result, List<Integer> currentPermutation, int[] nums, boolean[] used) {
        // Every good recursive method needs a base case to know when to stop.
        // Here, our base case is when the permutation we're building is the same size as the input array.
        if (currentPermutation.size() == nums.length) {
            // At this point, we've formed a complete, valid permutation.
            // We need to add a copy of it to our final result list. It's critical to add a copy, because 'currentPermutation' will be modified as we backtrack up the recursion tree.
            result.add(new ArrayList<>(currentPermutation));
            // After adding the result, we're done with this specific recursive path, so we return.
            return;
        }
        // Otherwise, if their sizes don't match, we'll explore our choices for the next number in the permutation.
        // We'll iterate through all the numbers in the original input array.
        for (int i = 0; i < nums.length; i++) {
            // For each iteration, we have a check at first: if the number at index 'i' has already been used in this path, we simply skip it and continue to the next number to avoid duplicates in the permutation.
            if (used[i]) {
                continue;
            }
            // On the other hand, if the number is available, we add it to our current permutation. This is the "choose" step.
            currentPermutation.add(nums[i]);
            // Then, we mark this number as 'used' to avoid duplicates in subsequent recursive calls.
            used[i] = true;
            // Now we "explore". We make a recursive call to this same method to continue building the permutation from here.
            backtrack(result, currentPermutation, nums, used);
            // After the recursive call above returns, it means we have fully explored that path.
            // We now need to backtrack our choice so we can explore other paths. First, we un-mark the number.
            used[i] = false;
            // And then we remove the number from the end of our current permutation list.
            // This takes us back to the previous state, allowing the loop to continue and try the next number.
            currentPermutation.remove(currentPermutation.size() - 1);
        }
    }
}
```

### Complexity Analysis

Okay, that's our implementation. Let's run it against the test cases. Yes! It got accepted. let's break down the time and space complexity.

- **Time Complexity: O(n⋅n)**
    - The number of permutations for an array of `n` distinct elements is **n!** (n factorial).
    - Our backtracking algorithm generates each of these permutations exactly once.
    - For each permutation, we do `n` amount of work to build it (adding `n` elements to the list and creating a final copy).
    - Therefore, the total time complexity is the number of permutations multiplied by the work done for each one, resulting in **O(n!⋅n)**.
- **Space Complexity: O(n)**
    - The primary space usage comes from the recursion stack. The maximum depth of the recursion will be `n`.
    - We also use a `boolean[] used` array of size `n` and a `currentPermutation` list which stores up to `n` elements.
    - All of these are proportional to `n`, so the auxiliary space complexity is **O(n)**. If we include the output list, the space would be O(n!⋅n).
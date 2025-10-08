## 104_Backtracking_39._Combination_Sum

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

 

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

 

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`



## Initial Thought Process

Looking at this problem, we're asked to find all unique combinations of numbers from a given array that sum up to a target value. What makes this interesting is that we can reuse numbers as many times as needed.

Let's break down our initial thoughts:

1. We need to find combinations, not permutations, so order doesn't matter.
2. We can use the same number multiple times, which means this is an 'unlimited supply' problem.
3. The combinations must be unique, meaning we shouldn't have duplicates in our result.

This is a classic backtracking problem. We can think of a few approaches:

## Approach 1: Naive Recursive Approach

We could try all possible combinations by adding each candidate to our current combination and recursively finding remaining combinations. However, this would lead to many duplicate combinations and be highly inefficient.

## Approach 2: Backtracking with Index Control

A more efficient approach is to use backtracking with an index parameter to avoid duplicates. By only considering candidates from the current index forward, we ensure we don't reuse the same combinations in different orders.

## Approach 3: Dynamic Programming

We could use DP to build up combinations that sum to each value from 1 to target. However, since we need to return the actual combinations, not just the count, backtracking is more straightforward here.

Let's implement the second approach, which offers a good balance of simplicity and efficiency.

## Detailed Solution: Backtracking with Index Control

```java
class Solution {    
    // This is our main method that returns all unique combinations that sum up to the target
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        // First, we create a list to store all valid combinations we'll find
        List<List<Integer>> result = new ArrayList<>();
        // Then, we'll use a helper list to build our combinations one element at a time
        List<Integer> currentCombination = new ArrayList<>();
        // Next, we start the backtracking process with candidates array, full target, initial index 0, currentCombination and the result list
        backtrack(candidates, target, 0, currentCombination, result);
        // Finally, after all recursive paths have been explored, we return the result
        return result;
    }
    // This helper method implements the backtracking algorithm to find all combinations. It takes the final result list, the current combination being built, the candidates array, the remaining target we need to reach, and the starting index for our choices.
    private void backtrack(int[] candidates, int remaining, int start, 
                          List<Integer> currentCombination, List<List<Integer>> result) {
        // Every good recursive method needs a base case to know when to stop. We have two base cases here. First, if our remaining target becomes negative. This means the last candidate we added was too large, so this path is invalid. We just stop and return.
        if (remaining < 0) {
            return;
        }
        // The second, successful base case is when our remaining target is exactly 0. This means we've found a valid combination that sums up to the original target.
        if (remaining == 0) {
            // Just like in other backtracking problems, we add a copy of the current combination to our results. This is crucial because 'currentCombination' will be modified as we backtrack.
            result.add(new ArrayList<>(currentCombination));
            // We're done with this path, so we return.
            return;
        }
        // After the base case check, we iterate through our choices. We start from the 'start' index to ensure we generate combinations in a non-decreasing order, which prevents duplicates.
        for (int i = start; i < candidates.length; i++) {
            // First, we add the current candidate to our combination
            currentCombination.add(candidates[i]);
            // Then, we pass the new remaining target, which is the previous target minus the candidate we just added. Crucially, we pass 'i' as the new start index, not 'i + 1' to reuse the same candidate multiple times.
            backtrack(candidates, remaining - candidates[i], i, currentCombination, result);
            // After the recursive call returns, we've finished exploring all paths with the candidate we just added. So we must remove it from our current combination, so we can try the next candidate in the loop.
            currentCombination.remove(currentCombination.size() - 1);
        }
    }
}
```

## Time and Space Complexity Analysis

Okay, that's our implementation. Let's run it against the test cases. Yes! It got accepted. let's break down the time and space complexity.

First, let's say **`N`** is the number of candidates, **`T`** is our target value, and **`M`** is the smallest number in our `candidates` array.

**Time Complexity: Roughly O(N^(T/M))**

- The maximum depth of our recursion tree can be at most `T/M`, as we can't add a number more times than that without exceeding the target.
- At each node in the decision tree, we have up to `N` branches to explore (we can pick any of the `N` candidates, though our `start` index prunes this).
- Therefore, the overall time complexity is roughly **O(N^(T/M))**. The additional factor comes from the loop and list operations. This is a loose upper bound, and the actual performance is better due to pruning (when `remainingTarget < 0`).

**Space Complexity: O(T/M)**

- Next, for the space complexity. If we don't count the space needed for the final result list, which is common practice, the space complexity is determined by the maximum depth of the recursion stack.
- As mentioned, the longest possible combination will be `T/M` at most.
- Finally, the recursion depth and the size of `currentCombination` are both bounded by this, so the overall space complexity is **O(T/M)**.



### Time Complexity: O(N^(T/M))

- N is the number of candidates
- T is the target value
- M is the minimum value among all candidates

The time complexity is exponential because:

- In the worst case, we might need to explore all possible combinations
- The maximum depth of our recursion tree is T/M (if we keep selecting the smallest candidate)
- At each level, we have up to N choices to make

### Space Complexity: O(T/M)

- The recursion stack can go as deep as T/M in the worst case
- The currentCombination list also takes O(T/M) space as it holds at most T/M elements (when using the smallest candidate repeatedly)
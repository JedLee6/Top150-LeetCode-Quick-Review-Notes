## 102_Backtracking_77._Combinations

Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers chosen from the range* `[1, n]`.

You may return the answer in **any order**.

 

**Example 1:**

```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.
```

 

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`

> "Exponential nature" describes a pattern of growth where the rate of increase is proportional to the current amount, causing the growth to become faster and faster over time. the word **nature** means the **fundamental quality, character, or essence** of something.
>
> binominal /baɪˈnəʊmɪəl/ N. a mathematical expression consisting of two terms, such as 3x+2y.
>
> coefficient /ˌkəʊɪˈfɪʃənt/ N. A <b>coefficient</b> is a number that expresses a measurement of a particular quality of a substance or object under specified conditions.
>
> The binomial coefficient counts the number of ways to choose a smaller, unordered subset of items from a larger set. It answers the question: "If I have **n** items, how many different combinations of **k** items can I choose?" The order in which you choose the items does not matter. It is written as C(n,k) and is read aloud as "**n choose k**."
>
> **`C(n,k)`** is pronounced "**n choose k**", which refers to the number of combinations.

## Initial Thoughts

Alright, let's analyze this problem. We need to find all possible combinations of k numbers from the range [1, n]. This is a classic combination problem.

The key insight is that for each number from 1 to n, we have two choices: either include it in our combination or exclude it. And we need to form combinations of exactly k numbers.

We can think of 2 ways to approach this:

1. **Backtracking (Best and Standard):** This is the most efficient and standard way to solve this problem because it builds the combinations directly and avoids generating duplicates from the start. Given the constraints (`n <= 20`), the exponential nature of this solution is perfectly acceptable.
2. **Iterative Approach:** We could also implement the backtracking logic iteratively using a stack to manage the state, which would avoid deep recursion. However, the recursive solution is often more intuitive and easier to write correctly, so we'll focus on that one first as it's the most common and clear solution.

I believe we can solve this by starting with backtracking, which is a common approach for combination problems.

## Approach 1: Backtracking

Our first approach would be using backtracking. In backtracking, we build the solution incrementally and abandon a path as soon as we determine it can't lead to a valid solution.

The basic idea is to consider each number from 1 to n, and for each number, we have two choices: include it in our current combination or skip it. When we've selected exactly k numbers, we've found a valid combination.

Let's code this up:

```java
// This function will solve the combinations problem using backtracking
public List<List<Integer>> combine(int n, int k) {
    // We'll store all valid combinations in this list
    List<List<Integer>> result = new ArrayList<>();
    // We'll use this list to build each combination during backtracking
    List<Integer> currentCombination = new ArrayList<>();
    // Start the backtracking process from number 1
    backtrack(result, currentCombination, 1, n, k);
    // Return all found combinations
    return result;
}

// This helper function implements the backtracking logic
private void backtrack(List<List<Integer>> result, List<Integer> currentCombination, int start, int n, int k) {
    // If we've selected exactly k numbers, we've found a valid combination
    if (currentCombination.size() == k) {
        // Add a copy of the current combination to our result as the currentCombination list is a mutable object that continues to be modified throughout the backtracking process. 
        result.add(new ArrayList<>(currentCombination));
        return;
    }
    // Try each number from start to n
    for (int i = start; i <= n; i++) {
        // Include the current number in our combination
        currentCombination.add(i);
        // Recursively find all combinations that include this number
        // We start from i+1 to avoid using the same number twice
        backtrack(result, currentCombination, i + 1, n, k);
        // Backtrack: remove the last added number to try other possibilities. When we reach a dead end or finish exploring one path in a maze, we must retrace our steps to a previous intersection to try a different path. Removing the number is the equivalent of taking one step back.
        currentCombination.remove(currentCombination.size() - 1);
    }
}
```

Okay, that's our implementation. Let's run it against the test cases. Yes! It got accepted. let's break down the time and space complexity.

### Time and Space Complexity Analysis

Let's analyze the time and space complexity:

Time Complexity: O(C(n,k) * k), where C(n,k) is the binomial coefficient (n choose k). This is because we generate all possible combinations, and for each combination, we spend O(k) time to create a copy of the current combination.

Space Complexity: O(k) for the recursion stack and the currentCombination list. The result list will contain all combinations, so that's O(C(n,k) * k), but that's for the output and not typically counted in space complexity analysis.

## Approach 2: Iterative Solution

We could also solve this problem iteratively. One way is to use a bit manipulation approach where we represent each combination as a bitmask. However, for clarity, let me show an iterative backtracking approach:

```java
// This function solves the combinations problem iteratively
public List<List<Integer>> combine(int n, int k) {
    // Initialize an empty list to store all combinations
    List<List<Integer>> result = new ArrayList<>();
    
    // Initialize with an empty combination
    List<Integer> current = new ArrayList<>();
    
    // Initialize a stack to simulate recursion
    int[] stack = new int[k+1];
    
    // Start with the first number (1)
    int index = 0;
    stack[index] = 1;
    
    // Continue until we've processed all possibilities
    while (index >= 0) {
        // Get the current number we're considering
        int num = stack[index];
        
        // If we've gone beyond n, backtrack
        if (num > n) {
            index--;
            if (index >= 0) {
                // Move to the next number at the previous level
                stack[index]++;
            }
            continue;
        }
        
        // If we haven't filled k positions yet
        if (index < k - 1) {
            // Move to the next position and start with the next number
            index++;
            stack[index] = stack[index-1] + 1;
        } else {
            // We've filled k positions, so we have a valid combination
            current.clear();
            for (int i = 0; i < k; i++) {
                current.add(stack[i]);
            }
            // Add this combination to our result
            result.add(new ArrayList<>(current));
            
            // Move to the next number for the last position
            stack[index]++;
        }
    }
    
    // Return all found combinations
    return result;
}
```

### Time and Space Complexity Analysis

The time and space complexity analysis for this iterative approach is similar to the recursive one:

Time Complexity: O(C(n,k) * k) for the same reasons as before.

Space Complexity: O(k) for the stack array and the current list. We're not using the recursion stack, but we still need space to store our current state.

This approach avoids the overhead of recursion, but the logic is a bit more complex.

## Approach 3: Lexicographic Combinations

Another interesting approach is to generate combinations in lexicographic order. This can be done by starting with the smallest combination [1,2,...,k] and then finding the next lexicographically greater combination until we reach the largest one [n-k+1, n-k+2, ..., n].

Let's implement this approach:

```java
// This function generates combinations in lexicographic order
public List<List<Integer>> combine(int n, int k) {
    // Initialize the result list to store all combinations
    List<List<Integer>> result = new ArrayList<>();
    
    // Edge case: if k is 0, return an empty list
    if (k == 0) {
        result.add(new ArrayList<>());
        return result;
    }
    
    // Create the first combination: [1, 2, ..., k]
    int[] combination = new int[k];
    for (int i = 0; i < k; i++) {
        // Fill the array with numbers 1 through k
        combination[i] = i + 1;
    }
    
    // Continue until we've processed all combinations
    boolean hasNext = true;
    while (hasNext) {
        // Add the current combination to our result
        List<Integer> currentCombination = new ArrayList<>();
        for (int num : combination) {
            currentCombination.add(num);
        }
        result.add(currentCombination);
        
        // Find the rightmost element that can be incremented
        int i = k - 1;
        while (i >= 0 && combination[i] == n - k + i + 1) {
            i--;
        }
        
        // If no such element exists, we're done
        if (i < 0) {
            hasNext = false;
        } else {
            // Increment this element
            combination[i]++;
            
            // Reset all elements to the right
            for (int j = i + 1; j < k; j++) {
                combination[j] = combination[j-1] + 1;
            }
        }
    }
    
    // Return all found combinations
    return result;
}
```

### Time and Space Complexity Analysis

The time and space complexity for this approach:

Time Complexity: O(C(n,k) * k). We generate all C(n,k) combinations, and for each, we spend O(k) time to create a new list.

Space Complexity: O(k) for the combination array, plus the space for the result.

This approach is elegant because it directly generates each combination without the need for backtracking, making it potentially more efficient in practice, especially for large values of n and k.

## Conclusion

To summarize, we've presented three different approaches to solve the combinations problem:

1. Recursive backtracking, which is intuitive and easy to understand.
2. Iterative simulation of the recursive approach, which avoids recursion overhead.
3. Lexicographic generation, which directly generates combinations in order.

All three approaches have similar time and space complexity in the worst case, but they differ in implementation details and might perform differently in practice depending on the specific values of n and k.

For most interview scenarios, we'd recommend going with the recursive backtracking approach (Approach 1) because it's most straightforward to explain and implement. But it's good to be aware of the alternatives.

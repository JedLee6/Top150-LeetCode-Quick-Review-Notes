## 101_Backtracking_17._Letter_Combinations_of_a_Phone_Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<img src="https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/1200px-telephone-keypad2svg.png" alt="img" style="zoom:25%;" />

 

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

 

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

> inclusive ADJ. After stating the first and last item in a set of things, you can add inclusive to make it clear that the items stated are included in the set.



## Initial Intuition

We have a string of digits from 2-9, and each digit maps to several letters on a phone keypad. We need to find all possible letter combinations.

This is clearly a combination problem where we need to consider all possible choices at each position. For example, if the input is '23', digit '2' maps to 'abc' and '3' maps to 'def'. We need to generate: 'ad', 'ae', 'af', 'bd', 'be', 'bf', 'cd', 'ce', 'cf'.

I'm thinking of a few approaches:
1. Backtracking (DFS): Build combinations one character at a time. For each digit, we'll try all possible letters and recursively build the rest of the combination.
2. BFS approach: Start with empty string and build up combinations level by level
3. Iterative approach: Similar to BFS but using a queue

Let me start with the simplest solution and improve from there.

## Solution 1: Backtracking (DFS) Approach

Let me first explain the backtracking approach, which is very intuitive for this problem.

We'll use a recursive function that builds the combinations one character at a time. For each digit, we'll try all possible letters and recursively build the rest of the combination.

```java
// First, I'll define a class to solve this problem
class Solution {
    // I'll create a map to store the mapping of digits to letters, similar to a phone keypad
    private Map<Character, String> digitToLetters = new HashMap<>();
    // This list will store all our final letter combinations
    private List<String> result = new ArrayList<>();
    // This is our main function that will solve the problem
    public List<String> letterCombinations(String digits) {
        // If the input is empty, we return an empty list as there are no combinations
        if (digits == null || digits.length() == 0) {
            return result;
        }
        // Now I'll initialize our digit-to-letters mapping according to the phone keypad
        digitToLetters.put('2', "abc");
        digitToLetters.put('3', "def");
        digitToLetters.put('4', "ghi");
        digitToLetters.put('5', "jkl");
        digitToLetters.put('6', "mno");
        digitToLetters.put('7', "pqrs");
        digitToLetters.put('8', "tuv");
        digitToLetters.put('9', "wxyz");
        // I'll call our backtracking function with an empty current combination and starting index 0
        backtrack(digits, 0, new StringBuilder());
        // Finally, return all the combinations we've found
        return result;
    }
    // This is our recursive backtracking function
    private void backtrack(String digits, int index, StringBuilder current) {
        // If we've processed all digits, we've found a valid combination
        if (index == digits.length()) {
            // Add the current combination to our result list
            result.add(current.toString());
            return;
        }
        // Get the current digit we're processing
        char digit = digits.charAt(index);
        // Get all possible letters for this digit
        String letters = digitToLetters.get(digit);
        // Try each possible letter for the current digit
        for (int i = 0; i < letters.length(); i++) {
            // Add the current letter to our combination
            current.append(letters.charAt(i));
            // Recursively build combinations for the next digits
            backtrack(digits, index + 1, current);
            // Backtrack by removing the last letter, so we can try the next letter. When we reach a dead end or finish exploring one path in a maze, we must retrace our steps to a previous intersection to try a different path. Removing the last letter is the equivalent of taking one step back.
            current.deleteCharAt(current.length() - 1);
        }
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

"The time complexity is O(4^N * N), where N is the number of digits in the input:

- Each digit can map to at most 4 letters (for digits 7 and 9)
- We have N digits, so we have 4^N possible combinations
- For each combination, we spend O(N) time to build the string

The space complexity is O(N) for the recursion stack, plus O(4^N * N) for storing all the combinations in the result list. However, if we only consider the working space, it's O(N) for the recursion stack and the StringBuilder."

## Solution 2: BFS Approach

"Now let me show a BFS approach, which might be easier to understand for some people.

Instead of building combinations depth-first, we'll build them breadth-first, starting with an empty string and adding one character at a time for each digit."

```java
// Define our solution class
class Solution {
    // This is our main function that will solve the problem using BFS
    public List<String> letterCombinations(String digits) {
        // Create a list to store the results
        List<String> result = new ArrayList<>();
        // If the input is empty, return an empty list
        if (digits == null || digits.length() == 0) {
            return result;
        }
        // Create a mapping of digits to letters
        String[] digitMap = new String[] {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        // Initialize our queue with an empty string
        Queue<String> queue = new LinkedList<>();
        queue.add("");
        // Process each digit in the input
        for (int i = 0; i < digits.length(); i++) {
            // Get the current digit as an integer
            int digit = Character.getNumericValue(digits.charAt(i));
            // Get the possible letters for this digit
            String letters = digitMap[digit];
            // Process all combinations at the current level
            int size = queue.size();
            for (int j = 0; j < size; j++) {
                // Get the current combination
                String current = queue.poll();
                // Add each possible letter to the current combination
                for (char letter : letters.toCharArray()) {
                    // Create a new combination by appending the letter
                    queue.add(current + letter);
                }
            }
        }
        // After processing all digits, the queue contains all possible combinations
        result.addAll(queue);
        // Return the final result
        return result;
    }
}
```

"The time complexity for this BFS approach is also O(4^N * N), because:
- We process each digit in the input (N iterations)
- For each digit, we process all combinations built so far
- The number of combinations grows exponentially (4^N in the worst case)
- String concatenation (current + letter) takes O(N) time

The space complexity is O(4^N) to store all combinations in the queue at the last level."

## Solution 3: Iterative Approach with List

"Let me present one more approach that's similar to BFS but uses a list instead of a queue for clarity."

```java
// Define our solution class
class Solution {
    // This is our main function that solves the problem iteratively
    public List<String> letterCombinations(String digits) {
        // Create a list to store our results
        List<String> result = new ArrayList<>();
        
        // If the input is empty, return an empty list
        if (digits == null || digits.length() == 0) {
            return result;
        }
        
        // Define the mapping of digits to letters
        String[] digitMap = new String[] {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        
        // Initialize our result list with an empty string
        result.add("");
        
        // Process each digit in the input
        for (int i = 0; i < digits.length(); i++) {
            // Get the current digit as an integer
            int digit = Character.getNumericValue(digits.charAt(i));
            
            // Get the possible letters for this digit
            String letters = digitMap[digit];
            
            // Create a new list to store the updated combinations
            List<String> newResult = new ArrayList<>();
            
            // For each existing combination, add each possible letter
            for (String combination : result) {
                // Add each possible letter to the current combination
                for (char letter : letters.toCharArray()) {
                    // Create a new combination by appending the letter
                    newResult.add(combination + letter);
                }
            }
            
            // Update our result list with the new combinations
            result = newResult;
        }
        
        // Return the final list of combinations
        return result;
    }
}
```

"The time and space complexity analysis is the same as the BFS approach:
- Time complexity: O(4^N * N)
- Space complexity: O(4^N) for storing all combinations

## Comparing the Solutions

"Each solution has its merits:

1. **Backtracking (DFS)**: Very intuitive, efficient memory usage during computation, but potentially more complex to understand.

2. **BFS with Queue**: More structured, processes combinations level by level, but requires queue management.

3. **Iterative with List**: Clean and straightforward, easiest to understand, but requires creating new lists at each step.

The time complexity is the same for all solutions: O(4^N * N), driven by the number of possible combinations and the cost of string operations.

In a real interview, I might prefer the backtracking solution for its elegance, or the iterative solution for its simplicity. If memory usage during computation is a concern (not counting the output), backtracking would be preferred as it uses O(N) working space compared to O(4^N) for the others."
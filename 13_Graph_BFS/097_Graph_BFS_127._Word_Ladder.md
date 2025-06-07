## 097_Graph_BFS_127._Word_Ladder

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from* `beginWord` *to* `endWord`*, or* `0` *if no such sequence exists.*

 

**Example 1:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
```

**Example 2:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```

 

**Constraints:**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.



## Initial Intuition

Looking at this problem, we immediately recognize it as a graph traversal problem. Each word can be seen as a node in a graph, and there's an edge between two words if they differ by exactly one letter. The question is asking for the shortest path from `beginWord` to `endWord`, which naturally calls for a Breadth-First Search (BFS) approach.

The key insight is that we need to find all words that are one letter different at each step, which forms the "neighbors" in our graph. Since BFS explores level by level, it will naturally find the shortest path.

Let's walk through a few approaches to solve this:

## Solution Approaches (From Worst to Best)

1. **Brute Force DFS** - We could try all possible paths using Depth-First Search, but this would be inefficient for finding the shortest path.

2. **Standard BFS** - Use breadth-first search to explore all possible word transformations level by level.

3. **Bidirectional BFS** - Search from both `beginWord` and `endWord` simultaneously to potentially reduce the search space.

4. **Optimized BFS with Word Pattern** - Instead of comparing each word with every other word, we can generate all possible one-letter transformations and check if they exist in our dictionary.

We'll start by implementing the standard BFS, as it's the foundation, and then we can implement the bidirectional BFS to show the optimization.

## Solution 1: Standard BFS

The core idea is to perform a level-order traversal starting from `beginWord`. We'll use a `Queue` to keep track of words to visit and a `Set` to store visited words to prevent cycles. A crucial optimization is how we find "neighboring" words. Instead of comparing a word against the entire dictionary, it's far more efficient to generate all its possible one-letter mutations and see if any of them exist in the dictionary. The "level" of our BFS will directly correspond to the length of the word sequence.

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // First, we convert the wordList to a HashSet for O(1) lookups, which will make it efficient to check if a word exists in our dictionary
        Set<String> wordSet = new HashSet<>(wordList);
        // Next, check if endWord is in the dictionary - if not, return 0 immediately as it's impossible to reach.
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        // Then, Iinitialize our BFS queue with the beginWord
        Queue<String> queue = new LinkedList<>();
        // And we'll start the search from the beginWord, so we add it to the queue.
        queue.offer(beginWord);
        // Following that, we need to keep track of words we've already visited to avoid cycles
        Set<String> visited = new HashSet<>();
        // And we mark the beginWord as visited.
        visited.add(beginWord);
        // After that, we declare a variable to track the number of transformations (or words in the sequence), so we start with a length of 1 for the beginWord itself.
        int level = 1;
        // Now we begin our BFS with a while loop, which continues as long as there are words left to explore in our queue
        while (!queue.isEmpty()) {
            // For each iteration, We grab the size of the queue here. This represents all the nodes at the current level
            int size = queue.size();
            // We'll process all nodes at the current level before moving to the next.
            for (int i = 0; i < size; i++) {
                // Inside the inner for-loop, we get the current word we're examining
                String currentWord = queue.poll();
                // Next, if we've reached our target word, return the current level as we have found the shortest path.
                if (currentWord.equals(endWord)) {
                    return level;
                }
                // Otherwise, Try to change each character of the word to find neighbors by transforming the currentWord string to character arrays at first
                char[] wordChars = currentWord.toCharArray();
                // Then we use a for-loop to iterate all characters in currentWord
                for (int j = 0; j < wordChars.length; j++) {
                    // For each iteration, we save the original character so we can restore it later
                    char originalChar = wordChars[j];
                    // Now we create a enhanced for-loop to try each possible letter from 'a' to 'z' at the current position
                    for (char c = 'a'; c <= 'z'; c++) {
                        // For each possible letter, we'll skip it if it's the same letter
                        if (c == originalChar) {
                            continue;
                        }
                        // Otherwise, we replace the character and create a new word
                        wordChars[j] = c;
                        String newWord = new String(wordChars);
                        // After that, we check if this word is in our dictionary and hasn't been visited
                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            // If so, we add it to our queue for further exploration
                            queue.offer(newWord);
                            // And we mark it as visited to avoid processing it again
                            visited.add(newWord);
                        }
                    }
                    // Out of the enhanced for-loop, we need to restore the original character for the next iteration
                    wordChars[j] = originalChar;
                }
            }
            // After processing all words at the current level, we increment the level to reflect the increased distance from the start.
            level++;
        }
        // Finally, when the queue becomes empty and we never reached the endWord, it means no transformation sequence exists. We return 0.
        return 0;
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### Time and Space Complexity Analysis:

**Time Complexity**: O(N * L² * 26)

- Let **N** be the number of words in `wordList` and **L** be the length of the words.
- The BFS loop, in the worst case, visits every word once. For each word, we iterate through its `L` positions. For each position, we try 26 letters. Inside this loop, creating a new string and checking for its existence in the `HashSet` both take O(L) time.

    So, processing one word is `L * 26 * O(L) = O(L²)`.

    The total time complexity is dominated by the BFS, resulting in **O(N \* L²)**.

**Space Complexity**: O(N \* L)

- The `wordSet` and `visited` set can store up to N words, each of length L, leading to O(N * L) space.
- The `queue` can also, in the worst case, hold a large number of words, contributing O(N * L) space.

## Solution 2: Bidirectional BFS

This approach significantly cuts down the search space. We maintain two sets of words: one starting from `beginWord` (`beginSet`) and one from `endWord` (`endSet`). We also have a main `visited` set to prevent cycles from either direction. In each iteration, we choose the smaller of the two sets to expand. The search terminates when we generate a word from one set that already exists in the other, indicating the paths have met.

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // First, convert the wordList to a HashSet for O(1) lookups
        Set<String> wordSet = new HashSet<>(wordList);
        
        // If endWord is not in the dictionary, return 0 immediately
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        
        // We'll use two sets for our bidirectional search
        // One set starts from beginWord
        Set<String> beginSet = new HashSet<>();
        beginSet.add(beginWord);
        
        // The other set starts from endWord
        Set<String> endSet = new HashSet<>();
        endSet.add(endWord);
        
        // Track words we've already visited
        Set<String> visited = new HashSet<>();
        
        // Initialize our level counter (sequence length)
        int level = 1;
        
        // Continue while we have words to explore from both directions
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            // Always expand the smaller set for efficiency
            if (beginSet.size() > endSet.size()) {
                // Swap the sets if beginSet is larger
                Set<String> temp = beginSet;
                beginSet = endSet;
                endSet = temp;
            }
            
            // This set will hold all words for the next level
            Set<String> nextLevelSet = new HashSet<>();
            
            // Process all words in the current level
            for (String word : beginSet) {
                // Try changing each character of the word
                char[] wordChars = word.toCharArray();
                
                for (int i = 0; i < wordChars.length; i++) {
                    // Save the original character
                    char originalChar = wordChars[i];
                    
                    // Try all possible character replacements
                    for (char c = 'a'; c <= 'z'; c++) {
                        // Skip if it's the same character
                        if (c == originalChar) {
                            continue;
                        }
                        
                        // Replace with the new character
                        wordChars[i] = c;
                        String transformedWord = new String(wordChars);
                        
                        // If the other search has already found this word,
                        // we've found a meeting point
                        if (endSet.contains(transformedWord)) {
                            return level + 1;  // +1 because we count words, not transitions
                        }
                        
                        // If it's a valid dictionary word and not visited before
                        if (wordSet.contains(transformedWord) && !visited.contains(transformedWord)) {
                            // Add to the next level
                            nextLevelSet.add(transformedWord);
                            // Mark as visited
                            visited.add(transformedWord);
                        }
                    }
                    
                    // Restore the original character
                    wordChars[i] = originalChar;
                }
            }
            
            // Move to the next level
            beginSet = nextLevelSet;
            level++;
        }
        
        // If we've exhausted all possibilities without finding a path
        return 0;
    }
}
```

### Time and Space Complexity Analysis:

**Time Complexity**: O(N * L² * 26)
- Theoretically, the worst-case complexity is the same as standard BFS
- However, bidirectional BFS often explores significantly fewer nodes in practice
- If the branching factor is b and the distance is d, standard BFS explores O(b^d) nodes
- Bidirectional BFS explores approximately O(2 * b^(d/2)), which is much smaller

**Space Complexity**: O(N)
- We store the wordList in a HashSet: O(N)
- The two sets for bidirectional search and visited set can contain at most N words
- Overall space complexity is O(N)


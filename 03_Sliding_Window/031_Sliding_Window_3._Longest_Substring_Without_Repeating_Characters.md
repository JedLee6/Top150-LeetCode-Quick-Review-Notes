## 031_Sliding_Window_3._Longest_Substring_Without_Repeating_Characters

Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

 

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.



As an experienced Java developer, let's tackle the problem of finding the longest substring without repeating characters from a given string `s`. This is a common LeetCode problem that helps showcase one's understanding of string manipulation, hashing, and window sliding techniques.

### Intuition and Main Idea

To solve this problem, our main goal is to track characters we have seen as we traverse the string, and efficiently manage a running "window" of characters that contains no duplicates. Here are the approaches I would consider:

1. **Brute Force Approach**: Generate all possible substrings, check each for uniqueness.
2. **Sliding Window using HashSet**: Use a set to track characters in the current window and adjust the window dynamically.
3. **Optimized Sliding Window using HashMap**: Use a map to remember the last seen index of each character, which allows us to skip unnecessary characters efficiently.

### Approach 1: Brute Force
Here, we'll check each possible substring and see if it's composed of unique characters. This is not efficient but straightforward.

```java
public int lengthOfLongestSubstringBruteForce(String s) {
    int n = s.length();
    int maxLength = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j <= n; j++) {
            if (allUnique(s, i, j)) {
                maxLength = Math.max(maxLength, j - i);
            }
        }
    }
    return maxLength;
}

private boolean allUnique(String s, int start, int end) {
    Set<Character> set = new HashSet<>();
    for (int i = start; i < end; i++) {
        Character ch = s.charAt(i);
        if (set.contains(ch)) return false;
        set.add(ch);
    }
    return true;
}
```
- **Time Complexity**: \(O(n^3)\) — two nested loops to generate substrings and another inner loop to check for uniqueness.
- **Space Complexity**: \(O(min(n, m))\) — where `m` is the size of the character set (in this case, the ASCII character set).

### Approach 2: Sliding Window with HashSet
We optimize the above method using a sliding window technique with a HashSet.

```java
public int lengthOfLongestSubstringSlidingWindow(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();
    int maxLength = 0, i = 0, j = 0;
    while (i < n && j < n) {
        if (!set.contains(s.charAt(j))) {
            set.add(s.charAt(j++));
            maxLength = Math.max(maxLength, j - i);
        } else {
            set.remove(s.charAt(i++));
        }
    }
    return maxLength;
}
```
- **Time Complexity**: \(O(2n) = O(n)\) — worst case, each character is visited twice (once by `i` and once by `j`).
- **Space Complexity**: \(O(min(n, m))\).

### Approach 3: Optimized Sliding Window with HashMap
This method uses a HashMap to keep track of the characters' most recent indices, thus allowing us to skip redundant checks and reposition the start index directly.

```java
public int lengthOfLongestSubstringHashMap(String s) {
    int n = s.length(), maxLength = 0;
    Map<Character, Integer> map = new HashMap<>(); // current index of character
    for (int j = 0, i = 0; j < n; j++) {
        // Check if the current character has been seen before
        if (map.containsKey(s.charAt(j))) {
            // If so, shifts the window's start point to avoid character repetition and ensures that i does not move backwards.
            i = Math.max(map.get(s.charAt(j)), i);
            // When i is bigger than "map.get(s.charAt(j)", which means we found the duplicate character behind the start point of sliding window, and We can ignore this situation
        }
        //Calculates the length of the current substring
        maxLength = Math.max(maxLength, j - i + 1);
        //Puts or updates the entry for s.charAt(j) in the map with its new index, which is j + 1. Storing j + 1 as the index ensures that when this character is encountered again, the start index i can skip directly to this index, leveraging the sliding window mechanism.
        map.put(s.charAt(j), j + 1);
    }
    return maxLength;
}
```
- **Time Complexity**: (O(n)\) — each character is visited once.
- **Space Complexity**: (O(min(n, m))), where (m) is the size of the character set (the alphabet size used in the string).

### Conclusion

In practice, the optimized sliding window approach using a HashMap is the most efficient for this problem, offering \(O(n)\) time complexity with minimal space usage. This approach not only handles the complexity effectively but also leverages Java's HashMap to maintain a clear and efficient tracking of characters and their indices.
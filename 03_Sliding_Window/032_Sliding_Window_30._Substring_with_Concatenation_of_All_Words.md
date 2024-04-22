## 032_Sliding_Window_30. Substring with Concatenation of All Words

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

- For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of *the starting indices* of all the concatenated substrings in `s`. You can return the answer in **any order**.

 

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:**

The substring starting at 0 is `"barfoo"`. It is the concatenation of `["bar","foo"]` which is a permutation of `words`.
The substring starting at 9 is `"foobar"`. It is the concatenation of `["foo","bar"]` which is a permutation of `words`.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** []

**Explanation:**

There is no concatenated substring.

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12]

**Explanation:**

The substring starting at 6 is `"foobarthe"`. It is the concatenation of `["foo","bar","the"]`.
The substring starting at 9 is `"barthefoo"`. It is the concatenation of `["bar","the","foo"]`.
The substring starting at 12 is `"thefoobar"`. It is the concatenation of `["the","foo","bar"]`.

 

**Constraints:**

- `1 <= s.length <= 104`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `s` and `words[i]` consist of lowercase English letters.



### Problem Understanding and Intuition

Given a string `s` and an array `words` where each word in `words` is of the same length, we need to identify the starting indices in `s` where a substring is exactly the concatenation of any permutation of the `words`.

The main challenges are:
1. Identifying all possible permutations of `words`, which is computationally expensive.
2. Efficiently searching `s` for these permutations.

A more efficient approach involves:
- Counting the frequency of each word in `words` and trying to match these counts over substrings of `s` of appropriate length.
- Using a sliding window technique over `s` with the window size equal to the total length of all words combined.

### Solution Strategy

1. **Word Count and Length Calculation**: First, calculate the total length of all words when concatenated and the individual word length.
2. **Sliding Window Technique**: Use a sliding window over `s` of the size equal to the total length of all concatenated words.
3. **Matching Frequency Count**: Inside this window, check if the substring matches the frequency count of the words in `words`.

### Java Implementation

Let's implement this approach step-by-step with detailed annotations:

```java
public List<Integer> findSubstring(String s, String[] words) {
    List<Integer> result = new ArrayList<>();
    if (s == null || words == null || words.length == 0) return result;
    // Calculate word length and the total length of all words concatenated
    int wordLen = words[0].length();
    int windowLen = wordLen * words.length;
    // Check if the total length of concatenated words is greater than string length
    if (s.length() < windowLen) return result;
    // Count the frequency of each word in 'words' for checking if all words match the corresponding substring
    Map<String, Integer> wordCountMap = new HashMap<>();
    for (String word : words) {
        wordCountMap.put(word, wordCountMap.getOrDefault(word, 0) + 1);
    }
    // Sliding window to check each possible window in the string 's', i stands for the start point of sliding window, whose range is from 0 to `s.length() - windowLen`
    for (int i = 0; i <= s.length() - windowLen; i++) {
        Map<String, Integer> seenWords = new HashMap<>(); // Collect words seen so far
        int j = 0; // Count the checked words so far
        while (j < words.length) {
            String word = s.substring(i + j * wordLen, i + (j + 1) * wordLen);
            if (wordCountMap.containsKey(word)) {
                seenWords.put(word, seenWords.getOrDefault(word, 0) + 1);
                // If a word is seen more times than it appears in 'words', break
                if (seenWordsMap.get(word) > wordCountMap.get(word)) break;
            } else {
                break; // the word is not in 'words'
            }
            j++; // Increment j when the word match the substring so far
        }
        // If all words matched exactly the times they appear in 'words'
        if (j == words.length) {
            result.add(i);
        }
    }
    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O((n - m*k) * k), where ( n ) is the length of string `s`, ( m ) is the number of words, and ( k ) is the length of each word. The outer loop runs for (n - m*k) iterations and each iteration involves operations proportional to ( k ). 
- **Space Complexity**: (O(m)) for storing word counts and seen words, where ( m ) is the number of unique words.

This solution uses a hashmap to efficiently check if a given window in `s` can be rearranged to form a concatenation of the `words` array, avoiding the need to generate all permutations. This makes it feasible to handle larger inputs effectively.

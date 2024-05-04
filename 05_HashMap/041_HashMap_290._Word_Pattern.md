# 041_HashMap_290. Word Pattern

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`.

 

**Example 1:**

```
Input: pattern = "abba", s = "dog cat cat dog"
Output: true
```

**Example 2:**

```
Input: pattern = "abba", s = "dog cat cat fish"
Output: false
```

**Example 3:**

```
Input: pattern = "aaaa", s = "dog cat cat dog"
Output: false
```

 

**Constraints:**

- `1 <= pattern.length <= 300`
- `pattern` contains only lower-case English letters.
- `1 <= s.length <= 3000`
- `s` contains only lowercase English letters and spaces `' '`.
- `s` **does not contain** any leading or trailing spaces.
- All the words in `s` are separated by a **single space**.



### Problem Understanding
We are given a `pattern`, a string where each character represents a pattern to be followed, and a string `s` that needs to be checked against this pattern. The task is to determine if `s` follows the same pattern as described by the characters in `pattern`. The bijection implies that each character in `pattern` must map to a unique, non-empty word in `s` and vice versa.

### Intuition
Use two hash maps to store the mappings from  each character in the `pattern` to a word in `s` and vice versa, and check their consistency. If we find inconsistency, return false.

The intuitive approach is to map each character in the `pattern` to a word in `s`. If `s` properly follows the pattern, then each unique character in `pattern` will map to exactly one unique word in `s`, and each word in `s` should map back to one unique character in `pattern`. If not, we return false.

#### Method : Using Two Hash Maps

1. Split `s` into words based on spaces.
2. Check if the number of words in `s` matches the length of `pattern`.
3. Use two hash maps: 
   - One to map characters in `pattern` to words in `s`.
   - Another to check if a word in `s` has already been mapped to a different character in `pattern`.
4. Traverse the pattern and the words simultaneously:
   - If a mapping inconsistency is found, return false.
   - If a new character-word pair is found, add it to the map.
5. If no inconsistencies are found throughout the traversal, return true.

**Java Code**:
```java
public boolean wordPattern(String pattern, String s) {
    // Split `s` into words based on spaces
    String[] words = s.split(" ");
    // Check if the number of words in `s` matches the length of `pattern`
    if (words.length != pattern.length()) {
        return false; // Early exit if the lengths don't match
    }
    // One to map characters in `pattern` to words in `s`
    Map<Character, String> charToWord = new HashMap<>();
    // Another to check if a word in `s` has already been mapped to a different character in `pattern`
    Map<String, Character> wordToChar = new HashMap<>();
    // Traverse the pattern and the words simultaneously:
    for (int i = 0; i < pattern.length(); i++) {
        char c = pattern.charAt(i);
        String word = words[i];
        // Check if it has a mapping rule from c to word for charToWord 
        if (!charToWord.containsKey(c)) {
            // If not check the consistency of the mapping rule of wordToChar from word to c 
            if (wordToChar.containsKey(word)) {
                return false; // This word is already mapped to a different character
            }
            // If a new character-word pair is found, add it to the map
            charToWord.put(c, word);
            wordToChar.put(word, c);
        } else if (!charToWord.get(c).equals(word)) {
            // If so check the consistency of the mapping rule of charToWord from c to word
            return false; // Existing mapping does not match the current word
        }
    }
    return true; // String `s` Successfully matched the pattern
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n) where n is the number of characters in `pattern` or words in `s`. The operations in the hash map (insert and lookup) are on average O(1).
- **Space Complexity**: O(m + k) where m is the number of unique characters in `pattern` and k is the number of unique words in `s`. This reflects the space used by the two hash maps.

#### Additional Thoughts and Variants
- **Edge Cases**: Ensure the solution handles patterns and strings of different lengths immediately, and consider cases where the string `s` is empty or has extra spaces.
- **Scalability**: This solution is efficient for a reasonably large size of `pattern` and `s` due to its linear time complexity.

This method provides a clear and efficient way to solve the problem, ensuring that each character-word mapping is bijective and consistent across the string and pattern.

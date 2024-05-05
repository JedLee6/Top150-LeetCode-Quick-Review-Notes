# 042_HashMap_242. Valid Anagram

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

 

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.

 

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?



### Intuition

Every anagram pair has the exact same number of each character. Use a `HashMap` to count occurrences of each character in `s` and `t`.

### Method: Hash Map for Unicode Characters

1. Check if the lengths of `s` and `t` are the same.
2. Use a `HashMap` to count occurrences of each character in `s`.
3. Decrease each count for characters in `t`.
4. Check if all counts are zero.

**Java Code**:

```java
public boolean isAnagram(String s, String t) {
    // Check if the lengths of `s` and `t` are the same
    if (s.length() != t.length()) {
        return false;
    }
    // Use a `HashMap` to count occurrences of each character in `s`
    Map<Character, Integer> counts = new HashMap<>();
    for (char c : s.toCharArray()) {
        counts.put(c, counts.getOrDefault(c, 0) + 1);
    }
    // Decrease each count for characters in `t`.
    for (char c : t.toCharArray()) {
        counts.put(c, counts.getOrDefault(c, 0) - 1);
        if (counts.get(c) < 0) {
            return false;
        }
    }
    // Check if all counts are zero.
    for (int count : counts.values()) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```

**Complexity Analysis**:

- **Time Complexity**: O(n), where `n` is the length of the strings.
- **Space Complexity**: O(1) to O(n), depending on the number of unique characters in the strings. For Unicode characters, this could be considerably more than 26.
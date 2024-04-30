# 039_HashMap_383. Ransom Note

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` and `magazine` consist of lowercase English letters.



### Intuition and Main Idea

The goal is to determine whether it is possible to construct the ransom note using characters from the magazine. To achieve this, we need to count the occurrences of each character in the magazine and check if there are enough occurrences to form the ransom note.

Here are the steps to solve the problem:

1. **Count Frequency of Characters**: First, we count how many times each letter appears in `magazine`.
2. **Check Sufficiency of Characters**: Next, we check for each letter in `ransomNote` if `magazine` contains that letter in sufficient quantity.

### Solution: Using Hash Map

1. Create a HashMap called dictionary to store the character counts in the magazine.
2. Iterate through each character in the magazine string.
3. If the character is not present in the dictionary, add it with a count of 1.
4. If the character is already present, increment its count by 1.
5. Iterate through each character in the ransom note string.
6. Check if the character is present in the dictionary and its count is greater than 0.
7. If both conditions are satisfied, decrement the count of the character in the dictionary.
8. If a character is not present in the dictionary or its count is 0, return false.
9. If all characters in the ransom note have been checked successfully, return true.

## Code with Annotations

```java
public boolean canConstruct(String ransomNote, String magazine) {
    // Create a HashMap to store character counts
    HashMap<Character, Integer> charMap = new HashMap<>();
    // Iterate through the characters in the magazine and add them to charMap
    for (int i = 0; i < magazine.length(); i++) {
        char c = magazine.charAt(i);
        charMap.put(c, charMap.getOrDefault(c, 0) + 1);
    }
    // Iterate through the characters in the ransom note
    for (int i = 0; i < ransomNote.length(); i++) {
        char c = ransomNote.charAt(i);
        // If the count of the character in the HashMap is greater than 0, decrement its count by 1
        if (charMap.getOrDefault(c, 0) > 0) {
            charMap.put(c, charMap.get(c) - 1);
        } else {
            // If the count of the character is 0, return false
            return false;
        }
    }
    // All characters in the ransom note have been processed successfully,
    // so the ransom note can be formed from the magazine
    return true;
}
```

#### Complexity Analysis

- **Time Complexity**: O(N + M), where m is the length of the magazine string and n is the length of the ransom note string. This is because we iterate through each character in both strings once.
- **Space Complexity**: *O*(*m*), where m is the number of unique characters in the magazine string. This is because we store the character counts in the HashMap.
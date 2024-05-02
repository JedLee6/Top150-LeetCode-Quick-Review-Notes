# 040_HashMap_205. Isomorphic Strings

Given two strings `s` and `t`, *determine if they are isomorphic*.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

 

**Example 1:**

```
Input: s = "egg", t = "add"
Output: true
```

**Example 2:**

```
Input: s = "foo", t = "bar"
Output: false
```

**Example 3:**

```
Input: s = "paper", t = "title"
Output: true
```

 

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `t.length == s.length`
- `s` and `t` consist of any valid ascii character.



### Intuition and Main Idea

The problem of determining if two strings `s` and `t` are isomorphic involves checking if the characters in `s` can be consistently mapped to characters in `t`. The key challenges are:

1. Ensuring that each character in `s` maps to exactly one character in `t`.
2. No two characters from `s` can map to the same character in `t`, unless they are the same character.
3. The mapping must preserve the order of characters, meaning that the pattern of appearances in `s` must match those in `t`.

### Solution 1: Using Two Hash Maps

#### Approach

- Use two hash maps to store the mappings from characters in `s` to `t` and vice versa.
- As we traverse the strings, we establish and check these mappings to ensure that they are consistent throughout both strings.

#### Deep Dive into Why Both Mappings Are Checked

When determining if two strings `s` and `t` are isomorphic, it's essential to ensure that each character in `s` maps to one and only one character in `t`, and vice versa. The requirement for this bi-directional mapping check is to avoid collisions and maintain consistent mappings, which might not be evident if we only checked one direction.

Let's break this down:

1. **Checking from `s` to `t`**: It validates that each character in `s` consistently maps to a particular character in `t` every time it appears.
2. **Checking from `t` to `s`**: This ensures that no two different characters in `s` map to the a character in `t`. 

#### Scenario Illustration

Suppose we only checked the mapping from `s` to `t`. Consider `s = "ab"` and `t = "cc"`. If we only ensure that each character from `s` maps to a character in `t`, we might think `a -> c` and `b -> c` is valid because we aren’t looking for unique mappings from `t` back to `s`.

This results in a scenario where two different characters in `s` map to the same character in `t`, which violates the isomorphic property where characters must map one-to-one.

#### Code with Annotations
```java
public boolean isIsomorphic(String s, String t) {
    if (s.length() != t.length()) {
        return false; // If lengths differ, they can't be isomorphic
    }
    // "s to t" validates that each character in `s` consistently maps to a particular character in `t`.
    Map<Character, Character> sToT = new HashMap<>();
    // "t to s" ensures that no two different characters in `s` map to a same character in `t`.
    Map<Character, Character> tToS = new HashMap<>();
    // Double check is essential, suppose we only checked the mapping from s to t. Consider s = "ab" and t = "cc". If we only ensure that each character from s maps to a character in t, we might think a -> c and b -> c is valid because we aren’t looking for unique mappings from t back to s, but it violates the isomorphic definition where characters must map one-to-one.
    for (int i = 0; i < s.length(); i++) {
        char sChar = s.charAt(i);
        char tChar = t.charAt(i);
        // Check if it has a mapping rule from s to t for sChar 
        if (sToT.containsKey(sChar)) {
            // If so check for the consistency of the mapping rule from sChar to tChar
            if (sToT.get(sChar) != tChar) {
                return false; // Inconsistent mapping
            }
        } else {
            // If not, save the mapping rule from sChar to tChar
            sToT.put(sChar, tChar);
        }

        // Check if it has a mapping rule from t to s for tChar 
        if (tToS.containsKey(tChar)) {
            // If so check for the consistency of the mapping rule from tChar to sChar
            if (tToS.get(tChar) != sChar) {
                return false; // Inconsistent mapping
            }
        } else {
            // If not, save the mapping rule from tChar to sChar
            tToS.put(tChar, sChar);
        }
    }
    return true; // Consistent mappings found
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the length of the strings, since we traverse the strings once.
- **Space Complexity**: O(1) in terms of larger input sizes, as the size of the map is bounded by the size of the character set (256 for extended ASCII, much less for typical problems).

### Solution 2: Using Character Arrays for Mapping

#### Approach
- Instead of hash maps, use arrays for direct mapping based on character values. This reduces the overhead of hash map operations.

#### Code with Annotations
```java
public boolean isIsomorphic(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }

    // Character mappings stored in arrays
    char[] mapST = new char[256]; // Assuming extended ASCII
    char[] mapTS = new char[256];

    for (int i = 0; i < s.length(); i++) {
        char sChar = s.charAt(i);
        char tChar = t.charAt(i);

        // Check existing mapping from s to t
        if (mapST[sChar] != 0 && mapST[sChar] != tChar) {
            return false;
        }
        // Check existing mapping from t to s
        if (mapTS[tChar] != 0 && mapTS[tChar] != sChar) {
            return false;
        }

        // Set the mappings if not already set
        mapST[sChar] = tChar;
        mapTS[tChar] = sChar;
    }

    return true;
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the length of the strings.
- **Space Complexity**: O(1), using fixed-size character arrays.

### Summary

Both solutions effectively solve the problem with the same time complexity, but the second solution using arrays might offer slightly faster runtime in practice due to lower overhead compared to hash maps. However, the hash map solution is more straightforward to understand and implement, especially when dealing with character sets other than ASCII.
## Array,String_023_28. Find the Index of the First Occurrence in a String

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

**Example 2:**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```

**Constraints:**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` and `needle` consist of only lowercase English characters.



### Intuition and Understanding:
The core of this problem is substring search. The challenge is to efficiently find the starting index of `needle` in `haystack`.

#### Solution 1: Brute Force
This method involves checking each possible starting point in `haystack` to see if `needle` begins at that position. This approach is simple but potentially slow for large strings.

```java
public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();
  	// If needle is empty, return 0 as per problem statement.
    if (L == 0) return 0;  
  	//Iterate over haystack to find where needle can fit
    for (int start = 0; start < n - L + 1; start++) {
        if (haystack.substring(start, start + L).equals(needle)) {
          	// Return the start index where needle matches exactly.
            return start;  
        }
    }
  	// Return -1 if no match is found.
    return -1;  
}
```

**Complexity Analysis**:
- **Time Complexity**: O((n - L + 1) * L) because for each of the n - L + 1 possible starting positions in `haystack`, we compare a substring of length L.
- **Space Complexity**: O(1) because no additional space is proportional to the input size.

#### Solution 2: Knuth-Morris-Pratt (KMP) Algorithm
The KMP algorithm performs the search by maintaining a state of the longest prefix of the `needle` (pattern) that matches a suffix of the part of the `haystack` (text) scanned so far. When a mismatch occurs after some matches, it uses the LPS(Longest Prefix Suffix) array to determine where to restart the search, avoiding redoing matches that are known to be valid.

```java
public int strStr(String haystack, String needle) {
    if (needle.length() == 0) return 0; // Handle the edge case where needle is empty.
    int[] lps = computeLPSArray(needle); // Compute the LPS array for the needle.
    int i = 0; // Index for haystack.
    int j = 0; // Index for needle.
    while (i < haystack.length()) {
        if (haystack.charAt(i) == needle.charAt(j)) {
            i++;
            j++;
            if (j == needle.length()) { // Full needle found in haystack.
                return i - j; // Return the start position of the match in haystack.
            }
        } else if (j > 0) {
            j = lps[j - 1]; // Move the needle index to the position given by the LPS array.
        } else {
            i++;
        }
    }
    return -1; // If no match found, return -1.
}

private int[] computeLPSArray(String needle) {
    int[] lps = new int[needle.length()];
    int len = 0; // Length of the previous longest prefix suffix.
    int i = 1; // Start from the second character in needle.

    while (i < needle.length()) {
        if (needle.charAt(i) == needle.charAt(len)) {
            len++;
            lps[i] = len; // Increment length of the longest prefix suffix.
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1]; // Fall back in the LPS array.
            } else {
                lps[i] = 0; // No prefix suffix match found.
                i++;
            }
        }
    }
    return lps;
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n + m), where `n` is the length of `haystack` and `m` is the length of `needle`. This includes O(m) time for LPS array construction and O(n) for the scanning process.
- **Space Complexity**: O(m) for storing the LPS array.

These two solutions showcase the spectrum from a straightforward brute-force approach to a more complex but efficient pattern-matching algorithm. Both are valid, but the choice of solution depends on the specific requirements regarding time efficiency and the size of the input data.
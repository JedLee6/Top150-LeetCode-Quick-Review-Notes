## Array,String_020_14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

 

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

 

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters.

### Intuition

The key idea is to find the shortest string that all strings in the array have in common at the beginning. The longest common prefix cannot be longer than the shortest string in the array. We can compare characters one by one, from the start of each string, and continue until we can no longer find a common character among all strings.

### Approach 1: Vertical Scanning

- **Iterate over characters of the first string**: We use these characters to compare with the corresponding characters of other strings.
- **Check each string**: For each character in the first string, we check all other strings for a match at the same position.
- **Mismatch or end of string**: If a character doesn't match or we've reached the end of another string, we return the common prefix found so far.

#### Java Implementation:

```java
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";
      // Iterate over characters of the first string
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);
          // For each character in the first string, we check all other strings for a match at the same position.
            for (int j = 1; j < strs.length; j++) {
              // Mismatch or end of any string
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
}
```

#### Complexity Analysis:

- **Time Complexity**: O(S), similar to the first approach, but typically faster in cases where the common prefix is short compared to the string lengths.
- **Space Complexity**: O(1), constant extra space is used.

## Approach 2: Sorting-Based Solution

This code is used to find the longest common prefix of an array of strings, which is defined as the longest string that is a prefix of all the strings in the array. By sorting the array and then comparing the first and last elements, the code is able to find the common prefix that would be shared by all strings in the array.

1. Sort the elements of an array of strings called "strs" in lexicographic (alphabetical) order using the Arrays.sort(strs) method.
2. Assign the first element of the sorted array (the lexicographically smallest string) to a string variable s1.
3. Assign the last element of the sorted array (the lexicographically largest string) to a string variable s2.
4. Initialize an integer variable idx to 0.
5. Start a while loop that continues while idx is less than the length of s1 and s2.
6. Within the while loop, check if the character at the current index in s1 is equal to the character at the same index in s2. If the characters are equal, increment the value of idx by 1.
7. If the characters are not equal, exit the while loop.
8. Return the substring of s1 that starts from the first character and ends at the idxth character (exclusive).

## Complexity

- Time complexity:

1. Sorting the array of strings takes O(Nlog(N)) time. This is because most of the common sorting algorithms like quicksort, mergesort, and heapsort have an average time complexity of O(Nlog(N)).
2. Iterating over the characters of the first and last strings takes O(M) time. This is because the code compares the characters of the two strings until it finds the first mismatch.

Therefore, the total time complexity is O(Nlog(N) + M).

- Space complexity:
  The space used by the two string variables s1 and s2 is proportional to the length of the longest string in the array. Therefore, the space complexity is O(1) as it does not depend on the size of the input array.

## Reason for Sorting

The reason why we sort the input array of strings and compare the first and last strings is that the longest common prefix of all the strings must be a prefix of the first string and a prefix of the last string in the sorted array. This is because strings are ordered based on their alphabetical order (Lexicographical order).
For example, consider the input array of strings {"flower", "flow", "flight"}. After sorting the array, we get {"flight", "flow", "flower"}. The longest common prefix of all the strings is "fl", which is located at the beginning of the first string "flight" and the second string "flow". Therefore, by comparing the first and last strings of the sorted array, we can easily find the longest common prefix.

## Code

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
      // Sorted strings are ordered based on their alphabetical order
        Arrays.sort(strs);
        String s1 = strs[0];
        String s2 = strs[strs.length-1];
        int idx = 0;
      // Comparing the first and last elements and find their common prefix 
        while(idx < s1.length() && idx < s2.length()){
            if(s1.charAt(idx) == s2.charAt(idx)){
                idx++;
            } else {
                break;
            }
        }
        return s1.substring(0, idx);
    }
}
```

### Conclusion

Both approaches offer different ways to tackle the problem, with their time complexity being quite similar in theory but potentially different in practice based on the input data. Horizontal scanning might do unnecessary comparisons in some cases, while vertical scanning can be faster if the common prefix is short. The choice of approach depends on the expected nature of the input.
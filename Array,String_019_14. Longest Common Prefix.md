## Array,String_019_14. Longest Common Prefix

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

## Approach

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
        Arrays.sort(strs);
        String s1 = strs[0];
        String s2 = strs[strs.length-1];
        int idx = 0;
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
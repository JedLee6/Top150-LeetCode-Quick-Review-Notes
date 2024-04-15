## 026_Two Pointers_392. Is Subsequence

Given two strings `s` and `t`, return `true` *if* `s` *is a **subsequence** of* `t`*, or* `false` *otherwise*.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example 1:**

```
Input: s = "abc", t = "ahbgdc"
Output: true
```

**Example 2:**

```
Input: s = "axc", t = "ahbgdc"
Output: false
```

 

**Constraints:**

- `0 <= s.length <= 100`
- `0 <= t.length <= 104`
- `s` and `t` consist only of lowercase English letters.

 

**Follow up:** Suppose there are lots of incoming `s`, say `s1, s2, ..., sk` where `k >= 109`, and you want to check one by one to see if `t` has its subsequence. In this scenario, how would you change your code?



### Intuition and Approach:

The intuitive approach to this problem is to traverse both strings using pointers and ensure that every character in `s` appears in `t` in the same order.

### Approach: Two-Pointer Technique

```java
public class Solution {
    public boolean isSubsequence(String s, String t) {
        //Initialize two pointers, `indexS` and `indexT`, to traverse `s` and `t`.
        int indexS = 0, indexT = 0;
        //Continue until either pointer reaches the end of its respective string
        while (indexS < s.length() && indexT < t.length()) {
            //If characters at both pointers match, move the `indexS` pointer to check the next character in `s`.
            if (s.charAt(indexS) == t.charAt(indexT)) {
                indexS++;
            }
            indexT++; //Always move the pointer in t to continue scanning
        }
        //If we've traversed all of s in order, it's a subsequence
        return indexS == s.length(); 
    }
}
```

#### Complexity:

- **Time Complexity**: O(n), where n is the length of `t` because we potentially traverse all of `t`.
- **Space Complexity**: O(1) since we are using a constant amount of extra space.
## 033_Sliding_Window_76. Minimum Window Substring

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window*** 

***substring\***  *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.

The testcases will be generated such that the answer is **unique**.

 

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3:**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

 

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?



### Problem Understanding

We need to find the smallest substring in `s` that contains all the characters of `t`, with their counts in `t` maintained. This is a classical "sliding window" problem, which can efficiently be solved using two pointers.

### Intuition and Main Idea

The main idea is to use two pointers to create a window which can expand and contract as needed. The window will expand to include necessary characters and contract to minimize once a valid window that contains all the characters of `t` is found.

1. **Expand the right pointer** to include characters until all characters from `t` are within the window.
2. Once a valid window is found, **contract from the left** to try to reduce the size of the window while still keeping all the characters from `t`.
3. Record the smallest window that satisfies the condition.

### Solution 1: Sliding Window

Let's write the code and explain it:

```java
import java.util.*;
public class Solution {
    public String minWindow(String s, String t) {
        if (s.length() == 0 || t.length() == 0) return "";
        // Dictionary to keep a count of all the unique characters in t.
        Map<Character, Integer> tCharCountMap = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            int count = tCharCountMap.getOrDefault(t.charAt(i), 0);
            tCharCountMap.put(t.charAt(i), count + 1);
        }
        // Number of unique characters in t, which need to be present in the desired window.
        int required = tCharCountMap.size();
        // Left and Right pointer
        int l = 0, r = 0;
        // Formed is used to keep track of how many unique characters in t are present in the current window in its desired frequency.
        int formed = 0;
        // Build a hashmap for the frequency of characters in `t`.
        Map<Character, Integer> windowCharCountMap = new HashMap<>();
        // ans list of the form (window length, left, right)
        int[] ans = {Integer.MAX_VALUE, 0, 0};
        while (r < s.length()) {
            // Add one character from the right to the window
            char c = s.charAt(r);
            int count = windowCharCountMap.getOrDefault(c, 0);
            windowCharCountMap.put(c, count + 1);
            // If the frequency of the current character added equals to the desired count in t then increment the formed count
            if (tCharCountMap.containsKey(c) && windowCharCountMap.get(c).intValue() == tCharCountMap.get(c).intValue()) {
                formed++;
            }
            // Try and contract the window till the point it ceases to be 'desirable'.
            while (l <= r && formed == required) {
                c = s.charAt(l);
                // Save the smallest window until now.
                if (r - l + 1 < ans[0]) {
                    ans[0] = r - l + 1;
                    ans[1] = l;
                    ans[2] = r;
                }
                // The character at the position pointed by the `Left` pointer is no longer a part of the window.
                windowCharCountMap.put(c, windowCharCountMap.get(c) - 1);
                // Check if it's still a valid window after removing the character c
                if (tCharCountMap.containsKey(c) && windowCharCountMap.get(c).intValue() < tCharCountMap.get(c).intValue()) {
                    // If not, decrement `formed` to break the while loop and then increment `r` to find another valid window
                    formed--;
                }
                // Move the left pointer ahead, this would help to look for another possible smaller valid window.
                l++;
            }
            // Keep expanding to the right to find another valid window
            r++;   
        }
        //As the answer is unique, if a valid minimum window is found, return it; otherwise, return an empty string.
        return ans[0] == Integer.MAX_VALUE ? "" : s.substring(ans[1], ans[2] + 1);
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(m + n)\), where \(m\) is the length of `s` and \(n\) is the length of `t`. In the worst case, each character in `s` is visited twice by both the right and left pointers.
- **Space Complexity:** \(O(m + n)\) for storing the character counts and the substrings.

### Other Potential Solutions

1. **Binary Search on Window Size:** Another approach could involve using binary search on the size of the window combined with a fast check to see if all characters are present. This is more complex and generally slower due to repeated checking of conditions but can be considered for a non-standard approach.
2. **Optimized Sliding Window Using Array:** If the character set is limited (like ASCII), we can use an array instead of a hashmap for counting characters, which may provide a slight performance boost.

The first provided sliding window solution is generally the best for this problem, offering a clear balance between readability and efficiency.
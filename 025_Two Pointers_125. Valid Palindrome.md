## 025_Two Pointers_125. Valid Palindrome

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

>palindrome /ˈpælɪndroʊm/ A palindrome is a word or a phrase that is the same whether you read it backward or forward, for example, the word "refer".
>
>alphanumeric /ˌælfənuːˈmerɪk/ Alphanumeric characters include letters and numbers.
>
>canal /kəˈnæl/ A canal is a long, narrow stretch of water that has been made for boats to travel along or to bring water to a particular area.

**Example 1:**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

 

**Constraints:**

- `1 <= s.length <= 2 * 105`
- `s` consists only of printable ASCII characters.



### Intuition

1. **Normalize the String**: Convert the string such that it contains only lowercase alphanumeric characters.
2. **Check for Palindrome**: Compare the string from the beginning and end, moving towards the center, to see if it reads the same both ways.

### Approach: Two-Pointer Technique

This approach involves using two pointers, one starting from the beginning of the string and the other from the end, moving towards the center, comparing characters as they go.

#### Java Implementation Code:

```java
public class Solution {
    public boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            // Move left pointer to the next alphanumeric character or out of bounds
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            // Move right pointer to the previous alphanumeric character or out of bounds
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            // Compare characters
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false; // Characters do not match
            }
            // Move pointers towards the center
            left++;
            right--;
        }
        return true; // Passed all checks
    }
}
```

### Complexity

- Time Complexity: O(n), where n is the length of the string, since each character is processed once.
- Space Complexity: O(1), since no extra space proportional to the input size is used.
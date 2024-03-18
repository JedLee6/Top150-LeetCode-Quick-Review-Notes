## Array,String_018_58. Length of Last Word

Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*

A **word** is a maximal substring consisting of non-space characters only. A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
```

**Example 2:**

```
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.
```

**Example 3:**

```
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only English letters and spaces `' '`.
- There will be at least one word in `s`.

## Intuition

The problem requires finding the length of the last word in a string. We can start from the end of the string and iterate until the first non-space character is encountered. Then, we count the characters until the next space or the beginning of the string is reached.

## Approach 1

We initialize a pointer `i` to the last index of the string. We then move the pointer to the left while encountering consecutive spaces. After that, we set a variable `lastIndex` to the current position of `i`. We continue moving the pointer to the left until we find the start of the last word by encountering a space or reaching the beginning of the string. The length of the last word is then calculated by subtracting the current position of `i` from `lastIndex`.

## Complexity

- Time complexity: O(n), where n is the length of the input string. The algorithm iterates through the string once.
- Space complexity: O(1), as the algorithm uses a constant amount of extra space regardless of the input size.

## Code

```java
class Solution {
  public int lengthOfLastWord(String s) {
    int i = s.length() - 1;
    // Move pointer to the left while encountering consecutive spaces
    while (i >= 0 && s.charAt(i) == ' ')
      --i;
    // Set lastIndex to the current position of i
    final int lastIndex = i;
    // Move pointer to the left until the start of the last word
    while (i >= 0 && s.charAt(i) != ' ')
      --i;
    // Calculate the length of the last word
    return lastIndex - i;
  }
}
```

## Approach 2

The second approach is to split the input string by spaces using the `split` method, creating an array of words. Then, it retrieves the last word from the array and returns its length.

1. Split the input string using the `split` method with the space character as the delimiter.
2. Obtain the array of words.
3. Retrieve the last word from the array using `str[str.length-1]`.
4. Return the length of the last word.

## Complexity

- Time complexity: O(n), where n is the length of the input string. The `split` method and array operations take linear time.
- Space complexity: O(m), where m is the number of words in the string. The `split` method creates an array of words, which contributes to the space complexity.

## Code

```java
class Solution {
    public int lengthOfLastWord(String s) {
     String ss[] = s.split(" ");
     return ss[ss.length-1].length();
    }
}
```


## Array,String_020_151. Reverse Words in a String

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

 

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?

## Intuition

The intuition behind this solution is to split the input string into individual words, reverse their order, and then concatenate them back together with a single space between each word.

## Approach

1. The input string `s` is trimmed to remove any leading or trailing spaces using the `trim()` method.
2. The trimmed string is then split into an array of words using the `split("\\s+")` method. The regular expression "\s+" matches one or more whitespace characters, effectively separating the words.
3. A variable `out` is initialized as an empty string to store the reversed words.
4. Starting from the last word in the array (`str.length - 1`), the loop iterates backwards until the first word (index 0) is reached.
5. In each iteration, the current word `str[i]` is appended to `out` along with a space (" ") to separate the words.
6. Finally, the first word `str[0]` is appended to `out`.
7. The reversed string of words, stored in `out`, is returned as the result.

## Complexity

- Time complexity:O(n)

- Space complexity:O(n)

## Code

```rust
class Solution {
    public String reverseWords(String s) {
        // Trim the input string to remove leading and trailing spaces
        String[] str = s.trim().split("\\s+");
        // Initialize the output string
        String out = "";
        // Iterate through the words in reverse order
        for (int i = str.length - 1; i > 0; i--) {
            // Append the current word and a space to the output
            out += str[i] + " ";
        }
        // Append the first word to the output (without trailing space)
        return out + str[0];
    }
}
```
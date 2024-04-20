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

To solve this problem, we can follow these steps:

1. Trim leading and trailing spaces from the input string.
2. Split the string into words using space as a delimiter.
3. Reverse the order of the words.
4. Concatenate the reversed words with a single space between them.

### Meaning of every character in "\\s+"

In the regular expression "\s+", each character has a specific meaning:

1. **"\\"**: The backslash is an escape character in Java strings. It's used to indicate that the next character should be treated literally, rather than as a special character. In this case, the backslash is escaping the following backslash.
2. **"s"**: The letter "s" represents a whitespace character in regular expressions. It matches any whitespace character, including spaces, tabs, and line breaks.
3. **"+"**: The plus sign is a quantifier in regular expressions, meaning "one or more occurrences" of the preceding character or expression. In this case, it indicates that there should be one or more whitespace characters.

So, when combined together as "\\s+", it represents a regular expression pattern that matches one or more whitespace characters. **The reason for the double backslashes is that in Java strings, the backslash itself needs to be escaped with another backslash to represent a single backslash character in the regular expression**. Therefore, "\s+" matches one or more whitespace characters in a string.

## Code

```java
public String reverseWords(String s) {    
    // Split the string into words
  //The regular expression "\s+" matches one or more whitespace characters, effectively separating the words.
    String[] words = s.trim().split("\\s+");
    // Reverse the order of words
    StringBuilder reversed = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        reversed.append(words[i]);
      //Concatenate the reversed words with a single space between them
        if (i != 0) {
            reversed.append(" ");
        }
    }
    return reversed.toString();
}
```

## Complexity

- Time Complexity: O(n), where n is the length of the input string `s`. Trimming the string takes O(n) time, splitting the string takes O(n) time, and reversing and concatenating the words takes O(n) time.

- Space Complexity: O(n), where n is the length of the input string `s`. We use extra space to store the split words and the final reversed string.
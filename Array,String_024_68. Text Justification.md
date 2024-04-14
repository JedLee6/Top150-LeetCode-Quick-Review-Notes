## Array,String_024_68. Text Justification

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

**Note:**

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
- The input array `words` contains at least one word.

 

**Example 1:**

```
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Example 2:**

```
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```

**Example 3:**

```
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

**Constraints:**

- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` consists of only English letters and symbols.
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`



### Intuition

To tackle the Text Justification problem, we can break down the solution into a sequence of steps and implement a solution that adheres to the guidelines provided. The goal is to format a list of words to fit each line such that it conforms to a specific width (`maxWidth`), applying full justification and  left-justified rules as described.

### Approach:

1. **Greedy Construction**: Start by attempting to fit as many words as possible on a line until adding another word would exceed the `maxWidth`.
2. **Space Distribution**: Once we have a line's words, distribute spaces to meet the exact `maxWidth`. The spaces should be as evenly distributed as possible between the words.
3. **Special Handling of the Last Line and the line with one word**: The last line and the line with one word is left-justified without extra space between words, and any remaining space is added to the end of the line.

#### Code

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> justifiedText = new ArrayList<>();
        int firstWordIndex = 0; // first word index at each line
        while (firstWordIndex < words.length) {
            // Find the range of words to be included in this line
            int charNum = words[firstWordIndex].length(); // Number of character and space in this line
            int lastWordIndex = firstWordIndex + 1; // Last word index at each line
            while (lastWordIndex < words.length) {
                if (words[lastWordIndex].length() + charNum + 1 > maxWidth)
                    break;
                charNum += words[lastWordIndex].length() + 1; // 1 for space
                lastWordIndex++;
            }
            // Build the line
            StringBuilder builder = new StringBuilder();
            int gap = lastWordIndex - firstWordIndex - 1; // Number of gaps between words
            // If last line or number of words in the line is 1, left-justify
            if (lastWordIndex == words.length || gap == 0) {
                for (int i = firstWordIndex; i < lastWordIndex; i++) {
                    builder.append(words[i]).append(' ');
                }
                builder.deleteCharAt(builder.length() - 1); // Remove extra space
                while (builder.length() < maxWidth) { // Fill the rest with spaces
                    builder.append(' ');
                }
            } else {
                // Evenly distribute spaces for other lines
                int spaces = (maxWidth - charNum) / gap;
                int extraSpace = (maxWidth - charNum) % gap;
                for (int i = firstWordIndex; i < lastWordIndex; i++) {
                    builder.append(words[i]);
                    // Calculate spaces to add after each word
                    if (i < lastWordIndex - 1) {
                        // Handle any extra spaces that can't be evenly distributed.
                        int spaceToAdd = spaces + (i - firstWordIndex < extraSpace ? 1 : 0);
                        for (int s = 0; s <= spaceToAdd; s++) {
                            builder.append(' ');
                        }
                    }
                }
            }
            justifiedText.add(builder.toString());
            firstWordIndex = lastWordIndex;
        }
        return justifiedText;
    }
}
```

### Complexity Analysis:

- **Time Complexity**: O(n), where n is the total number of characters across all words. Each character is processed once when determining the lines and formatting them.
- **Space Complexity**: O(m), where m is the number of characters in the output list (the justified text). This is because, in the worst case, every line could be made up of a single word followed by spaces up to `maxWidth`.

This solution constructs each line in a greedy fashion, packs the words accordingly, and then adjusts the spacing to meet the justification requirements. The result is a list of fully justified lines, formatted according to the problem's constraints.
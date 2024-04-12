## Array,String_021_6. Zigzag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

 

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```

**Example 3:**

```
Input: s = "A", numRows = 1
Output: "A"
```

 

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of English letters (lower-case and upper-case), `','` and `'.'`.
- `1 <= numRows <= 1000`



## Intuition:

When arranging the string "PAYPALISHIRING" in a zigzag pattern with 3 rows, the characters of the string are written in a diagonal fashion downward until the number of specified rows is reached, and then they proceed in a reverse diagonal fashion upwards, forming a zigzag pattern. This cycle continues until all characters of the string are included in the pattern.

The main idea is to simulate this writing process by maintaining an array of `StringBuilder` objects, each corresponding to a row in the zigzag pattern. We'll iterate over the string, determining which row each character belongs to. We'll use a direction flag to decide whether we're moving "down" the rows or going back "up".

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/fdf22375-8354-4cb7-adb0-cef316e39a2d_1675385332.2793877-20240202231737765.png)

## Code

```java
public String convert(String s, int numRows) {
    if (numRows == 1 || s.length() <= numRows) return s;
    // Initialize StringBuilder objects for each row
    StringBuilder[] rows = new StringBuilder[numRows];
    for (int i = 0; i < numRows; i++) {
        rows[i] = new StringBuilder();
    }
    // Initialize currentRow to track which row we're on, and goingDown to control
    // the direction of movement through rows.
    int currentRow = 0;
    boolean goingDown = true;
    // Iterate over each character in the string
    for (char c : s.toCharArray()) {
        // Append the character to the corresponding row
        rows[currentRow].append(c);
        // Switch the direction when reaching the first or last row
        if (currentRow == 0) {
            goingDown = true;
        } else if (currentRow == numRows - 1) {
            goingDown = false;
        }
        currentRow += goingDown ? 1 : -1; // Move up or down
    }
    // Concatenate all rows to form the final string
    StringBuilder result = new StringBuilder();
    for (StringBuilder row : rows) {
        result.append(row);
    }
    return result.toString();
}
```

## Complexity

- **Time Complexity**: O(n), where `n` is the length of the string, since we go through all characters once.

- **Space Complexity**: O(n), as we store all characters in the `StringBuilder` array.
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

1. Just look at the top row what is the difference b/w each char i.e A and I and I and Q = 8
   5*2-2 == numberOf rows *2 - 2 (The corner elements are excluded).
   Similarly for each row i.e B and J the diff is 8, C and K is 8

2. The interesting part comes when the char in the diagnoal has to be added, but even this has a pattern

   There will be no char in between for row 0 and row n.
   There can be only one diagonal char and the diagonal diff is original diff -2 at each step or diff - (rowNumber*2);

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/fdf22375-8354-4cb7-adb0-cef316e39a2d_1675385332.2793877-20240202231737765.png)

## Approach

1. Create an empty StringBuilder which is our ans.
2. Calculate the diff = numRows*2 -2;
3. Iterate over 0 to rowNumber in a for loop
   The first char will be row number or i (append to String)
4. Write a while loop in the above for loop :
5. The first char will be row number or i (append to String)
6. Calculate the diagonalDiff if any and append to the String.
7. Increase the index by diff and return ans.

## Complexity

- Time complexity: **O(N)**

- Space complexity: **O(N)**

## Code

```dart
class Solution {
    public String convert(String s, int numRows) {
        int n = s.length();
        StringBuffer [] arr = new StringBuffer[numRows]; 
        for(int i=0; i<numRows; i++) arr[i] = new StringBuffer();

        int i=0;
        while(i<n){
            /// verticaly downword
            for(int ind=0; ind<numRows && i<n; ind++){
                arr[ind].append(s.charAt(i++));
            }
            /// bent upword
            for(int ind=numRows-2; ind>0 && i<n; ind--){
                arr[ind].append(s.charAt(i++));
            }
        }
        StringBuffer ans = new StringBuffer();
        for(StringBuffer el : arr){
            ans.append(el);
        }
        return ans.toString();
    }
}
```
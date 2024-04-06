## Array,String_017_13. Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

**Example 1:**

```
Input: s = "III"
Output: 3
Explanation: III = 3.
```

**Example 2:**

```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 3:**

```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

**Constraints:**

- `1 <= s.length <= 15`
- `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
- It is **guaranteed** that `s` is a valid roman numeral in the range `[1, 3999]`.

## Intuition:

The key intuition lies in the fact that in Roman numerals, when a smaller value appears before a larger value, it represents subtraction, while when a smaller value appears after or equal to a larger value, it represents addition.

## Explanation:

1. The unordered map `m` is created and initialized with mappings between Roman numeral characters and their corresponding integer values. For example, 'I' is mapped to 1, 'V' to 5, 'X' to 10, and so on.

2. The variable `ans` is initialized to 0. This variable will accumulate the final integer value of the Roman numeral string.

3. The for loop iterates over each character in the input string `s`.
   **For the example "IX":**

   - When `i` is 0, the current character `s[i]` is 'I'. Since there is a next character ('X'), and the value of 'I' (1) is less than the value of 'X' (10), the condition `m[s[i]] < m[s[i+1]]` is true. In this case, we subtract the value of the current character from `ans`.

     `ans -= m[s[i]];`
     `ans -= m['I'];`
     `ans -= 1;`
     `ans` becomes -1.

   - When `i` is 1, the current character `s[i]` is 'X'. This is the last character in the string, so there is no next character to compare. Since there is no next character, we don't need to evaluate the condition. In this case, we add the value of the current character to `ans`.

     `ans += m[s[i]];`
     `ans += m['X'];`
     `ans += 10;`
     `ans` becomes 9.

   **For the example "XI":**

   - When `i` is 0, the current character `s[i]` is 'X'. Since there is a next character ('I'), and the value of 'X' (10) is greater than the value of 'I' (1), the condition `m[s[i]] < m[s[i+1]]` is false. In this case, we add the value of the current character to `ans`.

     `ans += m[s[i]];`
     `ans += m['X'];`
     `ans += 10;`
     `ans` becomes 10.

   - When `i` is 1, the current character `s[i]` is 'I'. This is the last character in the string, so there is no next character to compare. Since there is no next character, we don't need to evaluate the condition. In this case, we add the value of the current character to `ans`.

     `ans += m[s[i]];`
     `ans += m['I'];`
     `ans += 1;`
     `ans` becomes 11.

4. After the for loop, the accumulated value in `ans` represents the integer conversion of the Roman numeral string, and it is returned as the result.

## Complexity

- Time complexity: O(N)

- Space complexity: O(N)

## Code

```java
class Solution {
    public int romanToInt(String s) {
      //mappings between Roman numeral characters and their corresponding integer values
        Map<Character, Integer> m = new HashMap<>();
        m.put('I', 1);
        m.put('V', 5);
        m.put('X', 10);
        m.put('L', 50);
        m.put('C', 100);
        m.put('D', 500);
        m.put('M', 1000);
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
          //when a smaller value appears before a larger value, it represents subtraction, while when a smaller value appears after or equal to a larger value, it represents addition.
            if (i < s.length() - 1 && m.get(s.charAt(i)) < m.get(s.charAt(i + 1))) {
                ans -= m.get(s.charAt(i));
            } else {
                ans += m.get(s.charAt(i));
            }
        }
        return ans;
    }
}
```
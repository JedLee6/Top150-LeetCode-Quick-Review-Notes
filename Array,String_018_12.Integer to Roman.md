## Array,String_018_12.Integer to Roman

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

For example, `2` is written as `II` in Roman numeral, just two one's added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.

 

**Example 1:**

```
Input: num = 3
Output: "III"
Explanation: 3 is represented as 3 ones.
```

**Example 2:**

```
Input: num = 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 3:**

```
Input: num = 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

 

**Constraints:**

- `1 <= num <= 3999`



### Intuition

My initial approach is based on the fact that Roman numerals are constructed by placing symbols from largest to smallest from left to right, except for specific cases that involve subtraction. Hence, I'd start by handling the largest symbols and work my way down, checking at each step how many times a symbol fits into the remaining part of the number.

### Approach : Greedy Algorithm

The Greedy algorithm is a straightforward approach for this problem. We start from the highest value going down to the lowest, subtracting the value from the number and appending the corresponding Roman numeral symbol to the result string until the number is reduced to 0.

#### Steps:

1. List all the Roman numeral symbols and their values, including the subtraction cases.
2. Start with the largest value (1000, corresponding to 'M') and go down the list.
3. For each symbol-value pair, determine how many times the symbol fits into the remaining number.
4. Append the symbol to the result string as many times as it fits and subtract the corresponding value from the number.
5. Repeat steps 3-4 until the number is reduced to 0.

#### Java Implementation:

```java
public class Solution {
    public String intToRoman(int num) {
        // Step 1: List all the Roman numeral symbols and their values, including the subtraction cases.
        int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        // Initialize the StringBuilder to build the Roman numeral string
        StringBuilder sb = new StringBuilder();
        // Start with the largest value (1000, corresponding to 'M') and go down the list until the number is reduced to 0
        for (int i = 0; i < values.length; i++) {
            // While the current value fits into num, append the symbol and subtract the value, otherwize skip the current value
            while (num >= values[i]) {
                sb.append(symbols[i]);
                num -= values[i];
            }
        }
        return sb.toString();
    }
}
```

#### Explanation:

- **`values` and `symbols` arrays**: These arrays hold the Roman numeral symbols and their values, including the special cases for subtraction.
- **`StringBuilder sb`**: Used for efficiently building the resulting Roman numeral string.
- **Loop and while condition**: For each symbol-value pair, if the current value can fit into `num`, the symbol is appended to `sb`, and the value is subtracted from `num`. This process repeats until `num` is reduced to 0.

#### Complexity Analysis:

- **Time Complexity**: O(1) - The loop iterates a constant number of times since the size of the `values` and `symbols` arrays are fixed and small (13 elements).
- **Space Complexity**: O(1) - The space used by `sb` and the arrays does not depend on the input `num`; therefore, the space complexity is constant.
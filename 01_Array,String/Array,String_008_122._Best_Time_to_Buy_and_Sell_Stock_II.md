## Array,String_08_122. Best Time to Buy and Sell Stock II

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return *the **maximum** profit you can achieve*. 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
```

**Constraints:**

- `1 <= prices.length <= 3 * 104`
- `0 <= prices[i] <= 104`

## Intuition

The problem requires maximizing profit by buying and selling stocks on the same day. We need to exploit every opportunity to gain profit, i.e., buy at a lower price and sell at a higher price on subsequent days. This problem can be solved by identifying the consecutive price increases and considering them as potential profit-making opportunities.



## Approach - Greedy Algorithm

We approach this problem greedily by maximizing profit at each opportunity. Since we can buy and sell on the same day if there's a price increase, instead of looking for the overall optimal solution, we focus on maximizing profit at each step, which eventually leads to the maximum overall profit.

The Greedy approach aims to maximize profits by summing up positive differences between consecutive days' prices. The implementation follows the equation: `∑(i=2,n)max(price[i]−price[i- 1],0)`, where only price increases contribute to the total profit.

1. Initialize `totalProfit` to 0, representing the total profit we'll accumulate.
2. Iterate through the prices starting from the second day (`i=1`) to the last day (`i<prices.length`).
3. Check if the current price (prices[i]) is greater than the buying price (prices[i - 1]).
   - If yes, calculate the profit by subtracting the buying price from the current price and add it to the `totalProfit`.
4. Return the accumulated profit (`totalProfit`).

## Complexity

**Time Complexity:** The algorithm has a time complexity of *O(n)*, where n is the number of days (length of the prices array). This is because we traverse the prices array only once.

**Space Complexity:** The algorithm has a space complexity of *O(1)*, as it uses only a constant amount of extra space regardless of the input size.

## Code

```python
class Solution {
    public int maxProfit(int[] prices) {
        int totalProfit = 0;
        for (int i = 1; i < prices.length; i++) {
            int priceDiff = prices[i] - prices[i - 1];
            if (priceDiff > 0) {
                totalProfit += priceDiff;
            }
        }
        return totalProfit;
    }
}
```

## Array,String_07_121.Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`. 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

------

## Approach

To solve this problem, I employed a straightforward approach that iterates through the array of stock prices. At each step, I kept track of the minimum stock price seen so far (`min_price`) and calculated the potential profit that could be obtained by selling at the current price (`prices[i] - min_price`). I updated the `maxprof` (maximum profit) variable with the maximum of its current value and the calculated profit. Additionally, I updated the `min_price` to be the minimum of the current stock price and the previously seen minimum.

## Complexity

- Time complexity: O(n)
  The algorithm iterates through the array of stock prices once, performing constant-time operations at each step. Therefore, the time complexity is linear in the size of the input array.
- Space complexity: O(1)
  The algorithm uses a constant amount of extra space to store variables like `min_price` and `maxprof`. The space complexity remains constant regardless of the size of the input array.

## Code

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPriceSoFar = prices[0];
        int maxProfitSoFar = 0;
        for (int i = 1; i < prices.length; i++) {
            minPriceSoFar = Math.min(minPriceSoFar, prices[i]);
            maxProfitSoFar = Math.max(maxProfitSoFar, prices[i] - minPriceSoFar);
        }
        return maxProfitSoFar;
    }
}
```

## Detailed Explanation

Let's understand this problem by an **imagination**. **Imagine** you have given a **time machine**, you can go to **past** to **buy** the **stock** of your choice when the price is very least. And again using that **time machine** you went into **future** to **sell** the **stock**.

By doing that you have achieve **maximum profit**. From `buying at very least price and selling at very higher price`. **And you have become rich now!**

Now let's just understand it with our given example,
**Input**: prices = [7,1,5,3,6,4]
**Output**: 5

![image](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/ca7614a0-a717-48c1-a4ba-7fba42046941_1643684561.8969386.png)

```
Remember one rule :- You can only buy one time & sell one time
```

- So, if **buy at 7** & **sell at any time in the future**, we'll face loss. Because **buying price** is way **higher** then **selling price** available we have
- Now, I have seen a dip & I **buy at 1** & **sell at 5** my **overall profit** will be **5 - 1 = 4**
- But what if, I had **buy at 1** & **sell at 6** my profit will be **6 - 1 = 5**. Which is **greater then my overall profit**. So, i will **update** my **overall profit** with new value.
- Now we have done as further we don't have any higher point to sell. We will **return our answer.**

I hope now question, approach is absolute clear.

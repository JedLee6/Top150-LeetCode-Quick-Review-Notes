## Array,String_015_135. Candy

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return *the minimum number of candies you need to have to distribute the candies to the children*.

**Example 1:**

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

**Example 2:**

```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions. 
```

**Constraints:**

- `n == ratings.length`
- `1 <= n <= 2 * 104`
- `0 <= ratings[i] <= 2 * 104`



## Comprehensive Guide to Solving "Candy": Distributing Candies Like a Pro

## Introduction & Problem Statement

Hey there, coding enthusiasts! Welcome back to another exciting coding session. Today's problem is a treat—literally! We're going to solve the "Candy" problem. Imagine you have a bunch of kids lined up, each with a rating assigned to them. Your task is to distribute candies to these kids following two simple rules:

1. Every child must get at least one candy.
2. A child with a higher rating should get more candies than their immediate neighbors.

Sounds like a piece of cake, right? But here's the twist: you need to accomplish this with the fewest candies possible. Let's dig into the mechanics of this problem and how to approach it.

## Key Concepts and Constraints

### Why is This Problem Unique?

1. **Child Ratings**:
   The ratings of each child are your only guide in how you distribute the candies. Following the rules while using these ratings makes this problem an intriguing puzzle.
2. **Minimum Candies**:
   You're not just distributing candies willy-nilly; the goal is to meet the conditions using the least amount of candy.
3. **Constraints**:
   - The length of the ratings array, `n` , is between 111 and 2×1042 \times 10^42×104.
   - Ratings are integers between 000 and 2×1042 \times 10^42×104.

### Strategies to Tackle the Problem

1. **Greedy Algorithm: Two-Pass Method**
   This method takes two passes through the ratings array to ensure that each child gets the appropriate amount of candy.
2. **One-Pass Greedy Algorithm: Up-Down-Peak Method**
   This more advanced method uses a single pass and employs three key variables—Up, Down, and Peak—to efficiently determine the minimum number of candies needed.

## Greedy Algorithm: Two-Pass Method Explained

### What is a Greedy Algorithm?

A Greedy Algorithm makes choices that seem optimal at the moment. The "Candy" problem is a classic example of a greedy algorithm challenge that requires distributing resources based on certain rules to minimize the total quantity used. For this problem, we use a two-pass greedy approach to make sure each child gets the minimum number of candies that still satisfy the conditions.

### The Nuts and Bolts of the Two-Pass Method

1. Initialize Candies Array
   - We start by creating a `candies` array of the same length as the `ratings` array and initialize all its values to 1. This is the base case and ensures that every child will receive at least one candy, satisfying the first condition.
2. Forward Pass: Left to Right
   - Now, we iterate through the `ratings` array from the beginning to the end. For each child (except the first), we compare their rating with the one to the left. If it's higher, we update the `candies` array for that child to be one more than the child on the left. This takes care of the second condition but only accounts for the child's left neighbor.
3. Backward Pass: Right to Left
   - After the forward pass, we loop through the array again but in the reverse direction. This time, we compare each child's rating with the child to their right. If the rating is higher, we need to make sure the child has more candies than the right neighbor. So, we update the `candies` array for that child to be the maximum between its current number of candies and one more than the right neighbor's candies. This ensures that both neighboring conditions are checked and satisfied.
4. Summing it All Up
   - Finally, we sum up all the values in the `candies` array. This will give us the minimum total number of candies that need to be distributed to satisfy both conditions.

## Intuition

The "Candy" problem is a classic example of a greedy algorithm challenge that requires distributing resources based on certain rules to minimize the total quantity used. For this problem, we use a two-pass greedy approach to make sure each child gets the minimum number of candies that still satisfy the conditions.

A key insight for solving this problem is recognizing that the distribution of candies can be determined by looking at the ratings of a child in relation to their neighbors from both sides (left and right). This problem can be broken down into two passes:

1. **Left-to-Right Pass**: Ensure each child has more candies than their left neighbor if their rating is higher.
2. **Right-to-Left Pass**: After the first pass, ensure each child also has more candies than their right neighbor if their rating is higher.

## Code

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
      // Assigning one candy to every child. This ensures the first rule is met.
        Arrays.fill(candies, 1);
      //Iterate through the children from left to right. For each child, if their rating is higher than the previous child's, give them one more candy than the previous child.
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }
      //Similarly, iterate from right to left. This time, if a child's rating is higher than the next child's, check if the current number of candies satisfies the condition. If not, update the candy count to be one more than what the next child has.
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = Math.max(candies[i], candies[i + 1] + 1);
            }
        }
      //Sum up the candies for all children. This total represents the minimum candies needed.
        int totalCandies = 0;
        for (int candy : candies) {
            totalCandies += candy;
        }
        return totalCandies;
    }
}
```

### Time and Space Complexity

- **Time Complexity**: O(n), because we make two passes through the array.
- **Space Complexity**: O(n), for storing the `candies` array.

## One-Pass Greedy Algorithm: Up-Down-Peak Method

### Why `Up`, `Down`, and `Peak`?

The essence of the one-pass greedy algorithm lies in these three variables: `Up`, `Down`, and `Peak`. They serve as counters for the following:

- **`Up`:** Counts how many children have **increasing ratings** from the last child. This helps us determine how many candies we need for a child with a higher rating than the previous child.
- **`Down`:** Counts how many children have **decreasing ratings** from the last child. This helps us determine how many candies we need for a child with a lower rating than the previous child.
- **`Peak`:** Keeps track of the **last highest point** in an increasing sequence. When we have a decreasing sequence after the peak, we can refer back to the `Peak` to adjust the number of candies if needed.

### How Does it Work?

1. Initialize Your Counters
   - Start with `ret = 1` because each child must have at least one candy. Initialize `up`, `down`, and `peak` to 0.
2. Loop Through Ratings
   - For each pair of adjacent children, compare their ratings. Here are the scenarios:
     - **If the rating is increasing**: Update `up` and `peak` by incrementing them by 1. Set `down` to 0. Add `up + 1` to `ret` because the current child must have one more candy than the previous child.
     - **If the rating is the same**: Reset `up`, `down`, and `peak` to 0, because neither an increasing nor a decreasing trend is maintained. Add 1 to `ret` because the current child must have at least one candy.
     - **If the rating is decreasing**: Update `down` by incrementing it by 1. Reset `up` to 0. Add `down` to `ret`. Additionally, if `peak` is greater than or equal to `down`, decrement `ret` by 1. This is because the peak child can share the same number of candies as one of the children in the decreasing sequence, which allows us to reduce the total number of candies.
3. Return the Total Candy Count
   - At the end of the loop, `ret` will contain the minimum total number of candies needed for all the children, so return `ret`.

By using `up`, `down`, and `peak`, we can efficiently traverse the ratings list just once, updating our total candies count (`ret`) as we go. This method is efficient and helps us solve the problem in a single pass, with a time complexity of O(n).

### Time and Space Complexity

- **Time Complexity**: O(n), for the single pass through the ratings array.
- **Space Complexity**: O(1), as we only use a few extra variables.

## Code One-Pass Greedy

```rust
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int totalCandies = n;
        int i = 1;

        while (i < n) {
            if (ratings[i] == ratings[i - 1]) {
                i++;
                continue;
            }

            int currentPeak = 0;
            while (i < n && ratings[i] > ratings[i - 1]) {
                currentPeak++;
                totalCandies += currentPeak;
                i++;
            }

            if (i == n) {
                return totalCandies;
            }

            int currentValley = 0;
            while (i < n && ratings[i] < ratings[i - 1]) {
                currentValley++;
                totalCandies += currentValley;
                i++;
            }

            totalCandies -= Math.min(currentPeak, currentValley);
        }

        return totalCandies;        
    }
}
```



> nuts and bolts, the essential or practical details
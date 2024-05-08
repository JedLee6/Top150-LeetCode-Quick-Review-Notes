# 045_HashMap_202. Happy Number

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

 

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**Example 2:**

```
Input: n = 2
Output: false
```

 

**Constraints:**

- `1 <= n <= 231 - 1`



## Intuition

The key to solving this problem lies in detecting the cycle in the sequence of transformations. If a number is a happy number, the sequence will eventually reach 1. If it's not, the sequence will enter a cycle. This detection can be efficiently managed using two primary approaches:
1. **Using a HashSet** to store numbers that have already appeared. If a number reappears, a cycle has been detected.
2. **Floyd's Cycle Detection Algorithm** (also known as the tortoise and the hare method), where two pointers move at different speeds.

## Method 1: Using HashSet

```java
public boolean isHappy(int n) {
    // Stores numbers already seen in the sequence to detect loops
    Set<Integer> seen = new HashSet<>();
    // Continues until `n` becomes 1 or `n` is found in `seen`
    while (n != 1 && !seen.contains(n)) {
        seen.add(n);
        n = getNext(n);
    }
    // If n reaches 1, the number is happy
    return n == 1;
}
// Helper method to calculate the next number in the sequence
private int getNext(int n) {
    int totalSum = 0;
    while (n > 0) {
        int d = n % 10; // Extract the last digit
        n = n / 10;     // Remove the last digit
        totalSum += d * d; // Add the square of the digit to the total
    }
    return totalSum; // Return the sum of squares of the digits
}
```
**Complexity Analysis**:
- **Time Complexity**: The complexity depends on the number of times the digits are squared and summed until 1 is reached or a loop is detected. This is generally hard to predict, but in practice, it converges quickly for most numbers.
- **Space Complexity**: O(k), where k is the number of unique numbers encountered. In the worst case, this could be large if the sequence takes many iterations to repeat or reach 1.

#### Method 2: Floyd's Cycle Detection Algorithm (Tortoise and Hare)

Floyd's Cycle Detection Algorithm, often referred to as the "Tortoise and the Hare" algorithm, is a pointer technique used to detect cycles within a sequence of iterated values. It's widely used in computer science to identify loops within linked lists, number sequences, and other iterative processes.

### Concept

The idea behind Floyd's Cycle Detection Algorithm is to use two pointers moving at different speeds through the sequence. Here's how these pointers are typically described:

- **Tortoise (Slow pointer)**: Moves one step at a time.
- **Hare (Fast pointer)**: Moves two steps at a time.

### How It Detects a Cycle

The algorithm is based on the premise that if there is a cycle, the faster-moving pointer (hare) will eventually "lap" (or meet) the slower-moving pointer (tortoise) inside the cycle. Here’s how you can visualize it:

> premis /ˈpremɪs/ A premise is something that you suppose is true and that you use as a basis for developing an idea.

1. **Initialization**: Start both pointers at the beginning of the sequence. In the case of happy numbers, both pointers start at the number `n`.
2. **Iteration**: Move the tortoise forward by one iteration (one step) and the hare by two iterations (two steps).
3. **Detection**: Continue the iteration until either:
    - The sequence reaches a terminating condition (e.g., reaches 1 in the happy number problem).
    - The hare meets the tortoise, indicating a cycle.

```java
public boolean isHappy(int n) {
    int slow = n; // Tortoise starts at the starting number
    int fast = getNext(n); // Hare moves one step ahead initially
    // Loop until the hare reaches 1 or hare meets tortoise
    while (fast != 1 && slow != fast) {
        slow = getNext(slow); // Move tortoise one step
        fast = getNext(getNext(fast)); // Move hare two steps
    }
    // If hare reaches 1, the number is happy
    return fast == 1;
}
// Helper method to calculate the next number in the sequence
private int getNext(int n) {
    int totalSum = 0;
    while (n > 0) {
        int d = n % 10; // Extract the last digit
        n = n / 10;     // Remove the last digit
        totalSum += d * d; // Add the square of the digit to the total
    }
    return totalSum; // Return the sum of squares of the digits
}
```
**Complexity Analysis**:

- **Time Complexity**: As before, hard to predict but tends to be efficient in detecting cycles.
- **Space Complexity**: O(1) since we are only using a few extra variables, regardless of the length of the sequence.

### Conclusion

Both methods provide efficient ways to determine if a number is happy. The HashSet approach is straightforward and easy to understand, while Floyd's cycle detection provides an elegant solution with constant space complexity.
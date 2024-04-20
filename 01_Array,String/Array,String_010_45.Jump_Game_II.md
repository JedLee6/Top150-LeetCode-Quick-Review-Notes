## Array,String_010_45.Jump Game II

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return *the minimum number of jumps to reach* `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.

## Intuition :

To solve the problem efficiently, we can use a Greedy algorithm called "BFS (Breadth-First Search)" to find the minimum number of jumps needed to reach the end.

Here's how we can approach it:

1. We start from the first index and explore all the reachable indices from there.
2. At each iteration, we calculate the farthest index we can reach from the current position.
3. Within one jump, we look at all the indices reachable from the current position and pick the one that takes us the farthest.
4. Once we reach the farthest index reachable from the current jump, we make a jump and repeat the process.
5. We keep track of the number of jumps we make until we reach the last index.

> Implicit BFS, BFS stands for Breadth-First Search, which is a fundamental and widely used algorithm in computer science for traversing or searching tree or graph data structures.

## Complexity :

- Time complexity : O(n)

- Space complexity: O(1)

## Codes

```java
class Solution {
  public int jump(int[] nums) {
    int jumps = 0;
    // End of the current jump
    int currentJumpEnd = 0;
    // Farthest index reachable within current jump
    int farthest = 0;
    //We iterate through the array nums up to the second last index because we don't need to jump from the last index.
    for (int i = 0; i < nums.length - 1; ++i) {
      // At each iteration, we update farthest with the maximum of its current value and i + nums[i], which represents the farthest index reachable from the current position i. This step is similar to queue.push(node) step of BFS, but here we are only greedily storing the max reachable index on next jump.
      farthest = Math.max(farthest, i + nums[i]);
      // If i reaches currentJumpEnd, it means we've reached the end of the current jump. So, we increment jumps and update currentJumpEnd with the value of farthest, indicating the end of the next jump.
      if (i == currentJumpEnd) {
        // Increment the level
        ++jumps;     
        // Make the queue size for the next level
        currentJumpEnd = farthest; 
      }
    }
    // Finally, we return the value of jumps, which represents the minimum number of jumps needed to reach the last index.
    return jumps;
  }
}
```

## Breadth-First Search (BFS) algorithm

### Definition

Breadth-First Search (BFS) is a fundamental algorithm for traversing or searching tree or graph data structures. Starting from a selected node (often called the 'root' in a tree, or any arbitrary node in a graph), BFS explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. It employs a queue to keep track of the nodes that need to be explored and works in a "level-by-level" manner.

BFS guarantees to find the shortest path on unweighted graphs, making it ideal for finding the minimum number of segments or steps to reach a node from a given source.

### Example: Finding the Shortest Path in a Graph

Let's consider an example where we want to find the shortest path between two nodes in a graph using Breadth-First Search (BFS).

Suppose we have the following undirected graph:

```
      A
     / \
    B   C
   / \   \
  D   E---F
```

We want to find the shortest path from node A to node F.

Here's how BFS would work step by step to find the shortest path:

1. We start at node A.
2. We visit all the neighbors of A (B and C) and mark them as visited.
3. We enqueue B and C into the queue.
4. We dequeue a node from the queue (let's say B).
5. We visit all the neighbors of B (D and E) that have not been visited yet and mark them as visited.
6. We enqueue D and E into the queue.
7. We dequeue a node from the queue (let's say C).
8. We visit the neighbor of C (F) that has not been visited yet and mark it as visited.
9. We have found node F, which is the destination.

So, the shortest path from A to F is A -> C -> F.

BFS guarantees that we find the shortest path because it explores all nodes at a given distance from the starting node before moving to nodes that are farther away. This ensures that the first time we reach the destination node, it is via the shortest path.
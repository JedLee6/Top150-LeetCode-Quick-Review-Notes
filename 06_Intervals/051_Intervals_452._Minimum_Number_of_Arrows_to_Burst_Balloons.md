# 051_Intervals_452. Minimum Number of Arrows to Burst Balloons

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return *the **minimum** number of arrows that must be shot to burst all balloons*.

 

**Example 1:**

```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```

**Example 2:**

```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon for a total of 4 arrows.
```

**Example 3:**

```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].
- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].
```

 

**Constraints:**

- `1 <= points.length <= 105`
- `points[i].length == 2`
- `-231 <= xstart < xend <= 231 - 1`



### Intuition and Main Idea

The main idea to solve this problem is to use a greedy algorithm. The intuition is that to minimize the number of arrows needed, we should always shoot the arrow in such a way that it bursts as many balloons as possible. This can be achieved by sorting the balloons based on their end coordinates and then shooting an arrow at the end of the first balloon. This arrow will burst all overlapping balloons. We then continue this process for the remaining balloons that are not yet burst.

1. Sort balloons based on their end points in ascending order to guarantee shooting from the leftmost or rightmost balloon, which obeys our sub-optimal assumption of greedy algorithm.
2. Use a greedy approach to select the minimum number of arrows:
   - Shoot an arrow at the end point of the first balloon to ensure that the arrow will burst balloons as many as possible.
   - Skip all subsequent balloons that the arrow already bursts.
   - When encountering a balloon that isn't burst by the current arrow, shoot a new arrow at the end point of the new balloon.

> Sort balloons by its start position in descending order and shoot at each balloon's start position is equivalent to the above solution.

### Java Implementation with Annotations

```java
public int findMinArrowShots(int[][] points) {
    if (points.length == 0) return 0;
    // We use Arrays.sort with a custom comparator to sort balloons based on their end points in ascending order to guarantee shooting from the leftmost or rightmost balloon, which obeys our sub-optimal assumption of greedy algorithm.
    Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));
    int arrowCount = 1; // Start with one arrow for the first balloon
    int arrowPos = points[0][1]; // Position the first arrow at the end of the first balloon to ensure that each arrow will burst balloons as many as possible
    // Iterate through the sorted balloons
    for (int i = 1; i < points.length; i++) {
        // If the start of the current balloon is greater than the position of the last arrow
        // If so, it means the last arrow cannot burst this balloon, so we need another arrow.
        if (points[i][0] > arrowPos) {
            arrowCount++; // We need a new arrow
            arrowPos = points[i][1]; // Update the arrow position to the end of the current balloon
        }
        // If not, the current arrow also bursts this balloon, do nothing
    }
    return arrowCount;
}
```

### Complexity Analysis
- **Time Complexity**: Sorting the balloons takes \(O(n * log n)\), and the subsequent single pass through the balloons takes \(O(n)\). Thus, the overall time complexity is \(O(n * log n)\).
- **Space Complexity**: 
    - **Sorting space:** (O(log n)) due to the sorting algorithm's space complexity.
    - **Extra space:** (O(1)) since we only use a few additional variables.


### Alternative Approaches
- **Dynamic Programming**: Could be used to find the minimum number of arrows, especially if balloons had more complex interactions or constraints, but would likely result in higher time complexity.
- **Interval Tree**: Building an interval tree for these ranges could provide efficient queries and updates for more complex scenarios but is overkill for this problem given the sorted intervals approach is optimal.

The greedy method described is both optimal and straightforward for this problem, making it an excellent choice for an interview setting.
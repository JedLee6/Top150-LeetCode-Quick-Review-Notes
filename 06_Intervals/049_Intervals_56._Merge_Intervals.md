# 049_Intervals_56. Merge Intervals

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

 

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

 

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`



### Intuition and Main Idea

The most effective approach to solve this problem is:
1. **Sorting the Intervals**: Begin by sorting the intervals based on their starting values. This will allow us to easily check for overlaps as we iterate through the sorted list.
2. **Merging Overlapping Intervals**: Traverse through the sorted intervals and merge any overlapping intervals by updating the end of the current interval if necessary.

### Step-by-Step Java Solution with Annotations

```java
public int[][] merge(int[][] intervals) {
    // Return the input if it's already non-overlapping or empty
    if (intervals.length <= 1) return intervals;
    // Sort the intervals by their starting points to ensure that any potential overlaps are contiguous in the array.
    Arrays.sort(intervals, (i1, i2) -> Integer.compare(i1[0], i2[0]));
    List<int[]> result = new ArrayList<>(); // This will store the merged intervals
    int[] newInterval = intervals[0]; // Start with the first interval
    result.add(newInterval); // Add the first interval to the result as the starting point for merging
    // Iterate through the intervals
    for (int[] interval : intervals) {
        // Check if there is an overlap
        if (interval[0] <= newInterval[1]) {
            // If there's an overlap, we merge them by updating the end of currentInterval
            newInterval[1] = Math.max(newInterval[1], interval[1]);
        } else {
            // Disjoint intervals, add the new interval to the list
            newInterval = interval; // Move to the next interval
            result.add(newInterval); // And add it to the result list
        }
    }
    // Return the merged intervals
    return result.toArray(new int[result.size()][]);
}
```

### Complexity Analysis
- **Time Complexity**: 
  - Sorting the intervals takes \(O(n * log n)\), where \(n\) is the number of intervals.
  - The merging process is \(O(n)\), as each interval is processed once.
  - Hence, the overall time complexity is \(O(n * log n)\).
- **Space Complexity**: 
  - \(O(n)\) for the `result` list in the worst case, if no intervals are merged.
  - Sorting might also take \(O(log n)\) space depending on the implementation.

This approach is optimal in terms of time complexity given the need to sort the intervals. It ensures that all overlapping intervals are merged correctly while maintaining a linear scan for the merge phase after sorting.
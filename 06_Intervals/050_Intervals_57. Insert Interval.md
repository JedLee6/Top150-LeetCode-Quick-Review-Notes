# 050_Intervals_57. Insert Interval

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

 

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

 

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`



### Intuition

We can iterate through the sorted intervals and compare each interval with the newInterval to add or merge it. There are several cases to consider:

1. If the current interval ends before the newInterval starts, we can add it to the result list as it is.
2. If there is an overlap between the current interval and the newInterval, we need to merge them by updating the start and end of the newInterval accordingly.
3. If the current interval starts after the newInterval ends, we can add the newInterval to the result list and then add the remaining intervals.

### Java Implementation with Annotations

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0;
    // Add all intervals that end before the start of the new interval. These do not overlap with the new interval.
    while (i < intervals.length && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }
    // For all overlapping interval that starts before or at the end of the new interval, we merge it to newInterval
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        // This involves adjusting the start and end of the new interval to include the current interval.
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]); // update the start of newInterval
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]); // update the end of newInterval
        i++;
    }
    result.add(newInterval); // add the merged interval to result
    // Any intervals that start after the end of the new interval are simply added to the result as they don't overlap.
    while (i < intervals.length) {
        // Add the rest of the intervals
        result.add(intervals[i]);
        i++;
    }
    // convert to int[][] array
    return result.toArray(new int[result.size()][]);
}
```

### Complexity Analysis
- **Time Complexity**: 
  - The algorithm traverses the list of intervals only once, making it \(O(n)\), where \(n\) is the number of intervals.
- **Space Complexity**: 
  - We store the result list, which in the worst case, will contain all the intervals plus one new interval, also \(O(n)\).

### Additional Solution Using Binary Search
Since the intervals are sorted, one could use binary search to find the correct insertion point more efficiently, potentially reducing the time complexity for insertion. However, since merging could still require \(O(n)\) time in the worst case (if many intervals overlap with the new interval), the overall worst-case time complexity remains \(O(n)\). This method would be more complex to implement and is typically more useful if we are performing multiple insertions.
# 046_HashMap_219. Contains Duplicate II

Given an integer array `nums` and an integer `k`, return `true` *if there are two **distinct indices*** `i` *and* `j` *in the array such that* `nums[i] == nums[j]` *and* `abs(i - j) <= k`.

 

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `0 <= k <= 105`

### Problem Understanding

The problem requires checking whether there exists any pair of distinct indices `i` and `j` in an integer array `nums` such that:
- `nums[i] == nums[j]` (the values at these indices are equal), and
- `abs(i - j) <= k` (the absolute difference between the indices is at most `k`).

### Intuition

The main idea to solve this problem is to ensure we can quickly check if a number has appeared before within a certain index range. We want a method that lets us:
1. Keep track of each number's last occurrence as we iterate through the array.
2. Easily check the distance between the current index and the last occurrence of the same number to see if it is within `k`.

### Solution Strategies

#### Method 1: Using a HashMap
We can use a HashMap to store numbers and their most recent indices. As we iterate through the array, we can use this map to check our conditions quickly.

**Java Code**:
```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    // Key is the number from `nums` and value is its most recent index.
    Map<Integer, Integer> map = new HashMap<>();
    // Iterate through `nums` to check and update the map.
    for (int i = 0; i < nums.length; i++) {
        // If `nums[i]` exists in `map`, compute the index difference and check if it is within `k`.
        if (map.containsKey(nums[i])) {
            int j = map.get(nums[i]);
            if (i - j <= k) return true; // Check if the difference is within k
        }
        // Always update the map with the current index after the condition check.
        map.put(nums[i], i); // Update the latest index of nums[i]
    }
    return false;
}
```
**Complexity Analysis**:
- **Time Complexity**: O(n), where n is the number of elements in `nums`. We iterate through the list once and operations with the HashMap (check and update) are O(1) on average.
- **Space Complexity**: O(m), where m is the number of unique elements in `nums` that could be stored in the map.

#### Method 2: Using a Sliding Window with HashSet
This method uses a HashSet to maintain a sliding window of `k` indices. As we move through the array, we keep only the last `k` numbers in our set.

**Java Code**:
```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    Set<Integer> set = new HashSet<>();

    for (int i = 0; i < nums.length; i++) {
        if (set.contains(nums[i])) return true; // Check for duplicate in the current window
        set.add(nums[i]); // Add current number to the set
        if (set.size() > k) {
            set.remove(nums[i - k]); // Keep the window size at most k
        }
    }

    return false;
}
```
**Explanation**:
- **HashSet `set`**: Maintains up to `k` latest unique numbers.
- **For Loop**: Iterate through `nums` to add to the set or check for duplicates.
- **Window Management**: Ensure that the size of the set does not exceed `k` by removing the oldest element when moving the window forward.

**Complexity Analysis**:
- **Time Complexity**: O(n), as each insert and remove operation in the HashSet is O(1) on average.
- **Space Complexity**: O(k), as the HashSet can contain up to `k` elements from `nums`.

### Conclusion

Both methods provide efficient ways to solve the problem. The choice between using a HashMap or a sliding window with HashSet depends on the specific constraints and requirements of the problem context, such as the size of `k` relative to the size of `nums` and the distribution of values within `nums`.
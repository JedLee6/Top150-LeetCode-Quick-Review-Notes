# 047_HashMap_128. Longest Consecutive Sequence

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

 

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

 

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`



### Intuition and Main Idea

The most effective way to tackle this problem within the O(n) constraint is to use a HashSet. The HashSet allows us to quickly check the existence of an element, which is crucial for efficiently finding consecutive sequences.

The basic idea is: Place all elements into a HashSet. Iterate over the array, and for each element, check if it is the start of a sequence. If so, continue to check for the next consecutive elements until we can't find another one, then update the maximum sequence length. After the iteration ended, return the maximum sequence length.

### Detailed Java Solution with Annotations

```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0; // Edge case for empty array
    // Use a HashSet to store all numbers for O(1) lookup
    Set<Integer> numSet = new HashSet<>(); 
    for (int num : nums) {
        numSet.add(num);
    }
    // To keep track of the maximum length found
    int longestStreak = 0; 
	// Iterate through the set
    for (int num : numSet) { 
        // For each element, check if it is the start of a sequence. We determine this by checking if the element just before it (`num - 1`) is not in the set
        if (!numSet.contains(num - 1)) { 
            // Only start counting if 'num' is the start of a sequence
            int currentNum = num;
            int currentStreak = 1;
            // Continues the sequence counting as long as consecutive numbers are found.
            while (numSet.contains(currentNum + 1)) { 
                // If found the next consecutive element, increment currentNum and currentStreak
                currentNum++;
                currentStreak++;
            }
            // Update the longest streak found
            longestStreak = Math.max(longestStreak, currentStreak); 
        }
    }
    return longestStreak;
}
```

### Complexity Analysis

- **Time Complexity**: O(n). Although it looks like there are nested loops, each element is processed in the inner while loop only once across all iterations of the outer loop. This maintains the overall time complexity at O(n) because every element is put into the HashSet once and each possible part of a sequence is checked once.
- **Space Complexity**: O(n) due to the storage of all elements in the HashSet.

This method effectively finds the longest consecutive sequence without needing to sort the array, leveraging the properties of the HashSet for efficient element look-up and ensuring that the algorithm runs in linear time.

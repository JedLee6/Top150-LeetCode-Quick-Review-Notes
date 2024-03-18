## Array,String_011_274. H-Index

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their `ith` paper, return *the researcher's h-index*.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): The h-index is defined as the maximum value of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

**Example 1:**

```
Input: citations = [3,0,6,1,5]
Output: 3
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
```

**Example 2:**

```
Input: citations = [1,3,1]
Output: 1
```

 

**Constraints:**

- `n == citations.length`
- `1 <= n <= 5000`
- `0 <= citations[i] <= 1000`

## Intuition

- The problem is to find the h-index of a researcher based on their citation counts. The h-index is the maximum value h such that the researcher has published at least h papers that have each been cited at least h times.

------

## Approach

- ***Two common approaches to solve this problem are:***

### 1. Brute Force Approach:

- Sort the array of citations in descending order and iterate through the sorted array. For each paper, check if its citation count is greater than or equal to its position in the sorted array. Keep track of the maximum h-index encountered. This approach has a time complexity of O(n log n) due to the sorting step.

### 2. Binary Search Approach:

- Sort the array of citations and perform a binary search to find the h-index. Initialize a search range between 0 and the length of the citations array. In each iteration, calculate the mid-point and count the number of papers with citations greater than or equal to the mid-point. Adjust the search range based on this count. This approach has a time complexity of O(n log n) due to the sorting step.

## Time complexity:

1. **Code 1 (Brute Force)**

- Time complexity: O(n log n) - Sorting the array takes O(n log n) time, and the subsequent iteration takes O(n) time.

1. **Code 2 (Binary Search)**

- Time complexity: O(n log n) - Sorting the array takes O(n log n) time, and the binary search takes O(log n) time.

## Space complexity:

1. **Code 1 (Brute Force)**

- Space complexity: O(1) - Constant space is used.

1. **Code 2 (Binary Search)**

- Space complexity: O(1) - Constant space is used.

## Code(Binary Search)

```cpp
public int hIndex(int[] citations) {
    int n = citations.length;
    int[] buckets = new int[n+1];
    for(int c : citations) {
        if(c >= n) {
            buckets[n]++;
        } else {
            buckets[c]++;
        }
    }
    int count = 0;
    for(int i = n; i >= 0; i--) {
        count += buckets[i];
        if(count >= i) {
            return i;
        }
    }
    return 0;
}
```

This type of problems always throw me off, but it just takes some getting used to. The idea behind it is some bucket sort mechanisms. First, you may ask why bucket sort. Well, the h-index is defined as the number of papers with reference greater than the number. So assume `n` is the total number of papers, if we have `n+1` buckets, number from 0 to n, then for any paper with reference corresponding to the index of the bucket, we increment the count for that bucket. The only exception is that for any paper with larger number of reference than `n`, we put in the `n`-th bucket.

Then we iterate from the back to the front of the buckets, whenever the total count exceeds the index of the bucket, meaning that we have the index number of papers that have reference greater than or equal to the index. Which will be our h-index result. The reason to scan from the end of the array is that we are looking for the greatest h-index. For example, given array `[3,0,6,5,1]`, we have 6 buckets to contain how many papers have the corresponding index. Hope to image and explanation help.
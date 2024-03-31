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

------

## Approach 1: Sorting 

### Intuition

One straightforward way to approach this is to sort the citations array in descending order. This way, we can easily compare the citation count of each paper with its rank in the sorted list (which, in a way, represents the number of papers with equal or more citations).

#### Step-by-Step Solution

1. Sort the `citations` array in ascending order.
2. Iterate through the sorted array. Let `i` be the current index (0-based).
3. The condition to check at each step is whether the citation at the current index `citations[i]` is greater than or equal to `i+1` (since `i` is 0-based, `i+1` represents the number of papers).
4. If the condition fails for the first time, the value of `i` (before the failure) is the H-Index. If the loop completes without this condition failing, then the H-Index is the array length.

#### Code

```java
import java.util.Arrays;
public class Solution {
    public int hIndex(int[] citations) {
        // Step 1: Sort the citations array in ascending order
        Arrays.sort(citations);
        // Step 2: Iterate through the array to find the H-Index
        for (int i = 0; i < citations.length; i++) {
            int h = citations.length - i; // Calculate the H-Index at the current position
            if (citations[i] >= h) {
                // If the citation count is greater than or equal to the calculated H,
                // we've found the H-Index
                return h;
            }
        }  
        // If we didn't return within the loop, the H-Index is 0
        return 0;
    }
}
```

#### Explanation:

- **`Arrays.sort(citations);`**: We sort the `citations` array in ascending order (the Java default).
- **`for (int i = 0; i < citations.length; i++)`**: We iterate through each citation.
- **`int h = citations.length - i;`**: We calculate `h` for the current position. In an ascending order array, `h` is the length of the array minus the current index, which effectively counts how many elements are greater than or equal to the current citation.
- **`if (citations[i] >= h)`**: We check if the current citation is greater than or equal to `h`. If so, it means we have found the H-Index.
- **`return h;`**: Once we find the H-Index, we return it.
- **`return 0;`**: If no such condition is met, it implies an H-Index of 0.

#### Time Complexity Analysis:

- Sorting the array takes O(n log n) time.
- Iterating over the sorted array takes O(n) time.
- Overall time complexity: O(n log n)

#### Space Complexity Analysis:

- Sorting in-place doesn't require extra space.
- Only a constant amount of extra space is used.
- Space complexity: O(1)

### Approach 2: Counting Sort (Optimized for this problem)

#### Intuition

We can also employ a different strategy that avoids sorting the array. The key idea here is to use a counting sort mechanism, which is particularly effective because the range of possible citation counts (hence, the possible H-Index values) is limited to the length of the citations array. This approach is based on creating a histogram of citations, where each bin represents the number of papers with a certain citation count. Then, we'll aggregate the counts from the end of this histogram to find the H-Index.

#### Step-by-Step Solution

1. **Create a histogram of citations:** Since the H-Index cannot be larger than the number of papers, create an array `histogram` of length `n + 1`, where `n` is the number of papers. For any paper with `citations[i]` greater than `n`, increment `histogram[n]`. Otherwise, increment `histogram[citations[i]]`.
2. **Find the H-Index by accumulating counts backwards:** Start from the end of the histogram, which corresponds to the maximum possible H-Index. Accumulate the count of papers while moving backwards. The first time the accumulated count is equal to or exceeds the index, that index is the H-Index.

#### Code

```java
public class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] counts = new int[n + 1]; // Counts array for citation counts
        // Step 1: Count citations for each paper
        for (int citation : citations) {
            counts[Math.min(citation, n)]++; // Count citations within range [0, n]
        }
        int papers = 0;
        // Step 2: Iterate backwards to find the h-index
        for (int i = n; i >= 0; i--) {
            papers += counts[i]; // Accumulate papers with citation count >= i
            if (papers >= i) return i; // Check if we found h-index
        }
        return 0; // Default case
    }
}
```

#### Explanation:

1. **Counting Citations**: We use a counts array to count the number of papers having each citation count. Since the h-index cannot be greater than the total number of papers, we maintain counts up to `n`.
2. **Iterating Backwards**: We start iterating from the highest possible citation count (`n`) and move downwards. This way, we ensure we find the maximum h-index.
3. **Accumulating Papers**: We accumulate the number of papers as we iterate through the counts array. Once the accumulated papers count becomes greater than or equal to the current citation count (`i`), we've found the h-index.
4. **Return**: We return the current citation count `i` as the h-index.

#### Time Complexity Analysis:

- Counting citations takes O(n) time.
- Iterating backwards through the counts array takes O(n) time.
- Overall time complexity: O(n)

#### Space Complexity Analysis:

- We use an extra counts array of size `n + 1`.
- Space complexity: O(n)
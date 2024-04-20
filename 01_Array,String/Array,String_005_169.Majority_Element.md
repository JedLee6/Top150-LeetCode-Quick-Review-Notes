## Array,String_05_169. Majority Element

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

```
Input: nums = [3,2,3]
Output: 3
```

**Example 2:**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

## Approach 1: Sorting

## Intuition:

The intuition behind this approach is that if an element occurs more than n/2 times in the array (where n is the size of the array), it will always occupy the middle position when the array is sorted. Therefore, we can sort the array and return the element at index n/2.

## Explanation:

1. The code begins by sorting the array `nums` in non-decreasing order using the `sort` function from the C++ Standard Library. This rearranges the elements such that identical elements are grouped together.
2. Once the array is sorted, the majority element will always be present at index `n/2`, where `n` is the size of the array.
   - This is because the majority element occurs more than n/2 times, and when the array is sorted, it will occupy the middle position.
3. The code returns the element at index `n/2` as the majority element.

## Complexity

The time complexity of this approach is O(n log n) since sorting an array of size n takes O(n log n) time.

## Code

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}
```

## Approach 2: Hash Map

## Intuition:

The intuition behind using a hash map is to count the occurrences of each element in the array and then identify the element that occurs more than n/2 times. By storing the counts in a hash map, we can efficiently keep track of the occurrences of each element.

Explanation:

1. The code begins by initializing a hash map `m` to store the count of occurrences of each element.
2. It then iterates through the array `nums` using a for loop.
3. For each element `nums[i]`, it increments its count in the hash map `m` by using the line `m[nums[i]]++;`.
   - If `nums[i]` is encountered for the first time, it will be added to the hash map with a count of 1.
   - If `nums[i]` has been encountered before, its count in the hash map will be incremented by 1.
4. After counting the occurrences of each element, the code updates `n` to be `n/2`, where `n` is the size of the array. This is done to check if an element occurs more than n/2 times, which is the criteria for being the majority element.
5. The code then iterates through the key-value pairs in the hash map using a range-based for loop.
   - For each key-value pair `(x.first, x.second)`, it checks if the count `x.second` is greater than `n`.
   - If the count is greater than `n`, it means that `x.first` occurs more than n/2 times, so it returns `x.first` as the majority element.
6. If no majority element is found in the hash map, the code returns 0 as the default value.
   - Note that this will only occur if the input array `nums` is empty or does not have a majority element.

## Complexity

The time complexity of this approach is O(n) because it iterates through the array once to count the occurrences and then iterates through the hash map, which has a maximum size of the number of distinct elements in the array.

## Code

```java
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap();
        for (int p = 0; p < nums.length; p++) {
            map.put(nums[p], map.getOrDefault(nums[p], 0) + 1);
        }
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            if (entry.getValue() > nums.length / 2) {
                return entry.getKey();
            }
        }
        return 0;
    }
}
```

## Approach 3: Moore Voting Algorithm

## Intuition:

The intuition behind the Moore's Voting Algorithm is based on the fact that if there is a majority element in an array, it will always remain in the lead, even after encountering other elements.

## Explanation:

**Algorithm:**

1. Initialize two variables: `count` and `candidate`. Set `count` to 0 and `candidate` to an arbitrary value.
2. Iterate through the array `nums`:
   a. If `count` is 0, assign the current element as the new `candidate` and increment `count` by 1.
   b. If the current element is the same as the `candidate`, increment `count` by 1.
   c. If the current element is different from the `candidate`, decrement `count` by 1.
3. After the iteration, the `candidate` variable will hold the majority element.

**Explanation:**

1. The algorithm starts by assuming the first element as the majority candidate and sets the count to 1.

2. As it iterates through the array, it compares each element with the candidate:

   a. If the current element matches the candidate, it suggests that it reinforces the majority element because it appears again. Therefore, the count is incremented by 1.

   b. If the current element is different from the candidate, it suggests that there might be an equal number of occurrences of the majority element and other elements. Therefore, the count is decremented by 1.

   - Note that decrementing the count doesn't change the fact that the majority element occurs more than n/2 times.

3. If the count becomes 0, it means that the current candidate is no longer a potential majority element. In this case, a new candidate is chosen from the remaining elements.

4. The algorithm continues this process until it has traversed the entire array.

5. The final value of the `candidate` variable will hold the majority element.

**Explanation of Correctness:**
The algorithm works on the basis of the assumption that the majority element occurs more than n/2 times in the array. This assumption guarantees that even if the count is reset to 0 by other elements, the majority element will eventually regain the lead.

Let's consider two cases:

1. If the majority element has more than n/2 occurrences:
   - The algorithm will ensure that the count remains positive for the majority element throughout the traversal, guaranteeing that it will be selected as the final candidate.
2. If the majority element has exactly n/2 occurrences:
   - In this case, there will be an equal number of occurrences for the majority element and the remaining elements combined.
   - However, the majority element will still be selected as the final candidate because it will always have a lead over any other element.

In both cases, the algorithm will correctly identify the majority element.

## Complexity

The time complexity of the Moore's Voting Algorithm is O(n) since it traverses the array once.

This approach is efficient compared to sorting as it requires only a single pass through the array and does not change the original order of the elements.

## Code

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
}
```
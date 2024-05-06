043_HashMap_49. Group Anagrams

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. 

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**

```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**

```
Input: strs = ["a"]
Output: [["a"]]
```

 

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.



**Intuition and Main Idea:**

To group anagrams together, we need to identify which strings are anagrams of each other. Two strings are anagrams if they have the same characters but in a different order. 

One way to approach this is to use a hashmap where the keys are the sorted version of each string and the values are lists of original strings that are anagrams of each other. By sorting each string, we can easily group anagrams together as they will have the same sorted representation.

**Solution 1: Using Sorting and Hashmap**

```java
public List<List<String>> groupAnagrams(String[] strs) {
    // Initialize a hashmap to store the sorted strings and their corresponding anagrams
    Map<String, List<String>> map = new HashMap<>();
    // Iterate through each string in the array
    for (String str : strs) {
        // Convert the string to a char array, sort it, and convert it back to a string
        char[] charArray = str.toCharArray();
        Arrays.sort(charArray);
        String sortedStr = String.valueOf(charArray);

        // Check if the sorted string is already in the hashmap
        // If not, add it with an empty list as its value
        if (!map.containsKey(sortedStr)) {
            map.put(sortedStr, new ArrayList<>());
        }
        // Add the original string to the list corresponding to its sorted version
        map.get(sortedStr).add(str);
    }
    // Convert the values of the hashmap to a list and return
    return new ArrayList<>(map.values());
}
```

**Time Complexity Analysis:**

Let \( n \) be the total number of strings in the input array and \( k \) be the maximum length of a string.

- Sorting each string takes O(k*log(k)) time.
- Iterating through each string in the array takes O(n) time.
- Therefore, the overall time complexity of this solution is O(n*k*log(k)) .

**Space Complexity Analysis:**

- The hashmap can contain up to \( n \) keys, each with a list of anagrams. Therefore, the space complexity of this solution is \( O(n) \).
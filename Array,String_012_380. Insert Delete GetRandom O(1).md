## Array,String_012_380. Insert Delete GetRandom O(1)

Implement the `RandomizedSet` class:

- `RandomizedSet()` Initializes the `RandomizedSet` object.
- `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
- `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

**Example 1:**

```
Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
```

**Constraints:**

- `-231 <= val <= 231 - 1`
- At most `2 * ``105` calls will be made to `insert`, `remove`, and `getRandom`.
- There will be **at least one** element in the data structure when `getRandom` is called.

### Intuition and Main Idea

To achieve (O(1)) average time complexity for all required operations, we need to use a combination of data structures that complement each other's strengths and weaknesses:

1. **Use a HashMap for (O(1)) insertion and deletion**: A HashMap can insert and delete elements in (O(1)) time complexity. It will store the elements as keys and their indices in an ArrayList as values. This allows us to quickly check if an element exists in the set and retrieve its index for deletion.
2. **Use an ArrayList for (O(1)) access and to support getRandom()**: An ArrayList allows (O(1)) access time, which is perfect for the `getRandom()` operation. By storing the elements of the set in an ArrayList, we can easily pick a random element by generating a random index.
3. **Handle deletion cleverly to maintain (O(1)) time**: The tricky part is deleting an element in (O(1)) time from the ArrayList, as direct removal would have (O(n)) time complexity due to the need to shift elements. To overcome this, we can swap the element to be deleted with the last element in the ArrayList and then remove the last element. This operation maintains the ArrayList's integrity and allows constant time deletion.

## Approach Analysis

- **insert(int val)**: Checks if the value exists using `valToIndex`. If not, adds the value to `nums` and records its index in `valToIndex`. Time Complexity: (O(1)), Space Complexity: (O(1)) for each insert operation, but grows with the number of elements in the set.
- **remove(int val)**: Checks if the value exists. If it does, swaps it with the last element in `nums`, updates `valToIndex` accordingly, and removes the last element. This ensures that the ArrayList can still be used for efficient random access without gaps. Time Complexity: (O(1)), as each operation involved is constant time.
- **getRandom()**: Simply picks a random index and retrieves the element at that index from `nums`. Time Complexity: (O(1)), since accessing an element by index in an ArrayList is constant time.

## Complexity

- Time complexity:
  O(1)
- Space complexity:
  O(n)

## Code

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Random;

public class RandomizedSet {
    private ArrayList<Integer> nums;
    private HashMap<Integer, Integer> valToIndex;
    private Random rand = new Random();
    public RandomizedSet() {
        nums = new ArrayList<>();
        valToIndex = new HashMap<>();
    }
    public boolean insert(int val) {
        if (valToIndex.containsKey(val)) {
            return false; // The value is already present.
        }
        valToIndex.put(val, nums.size()); // Map the value to its index in nums.
        nums.add(val); // Add the value to nums.
        return true;
    }
    public boolean remove(int val) {
        if (!valToIndex.containsKey(val)) {
            return false; // The value is not present.
        }
        int index = valToIndex.get(val); // Get the index of the value to be removed.
        int lastElement = nums.get(nums.size() - 1); // Get the last element in nums.
        nums.set(index, lastElement); // Move the last element to the index of the element to be removed.
        valToIndex.put(lastElement, index); // Update the index of the last element in the map.
        nums.remove(nums.size() - 1); // Remove the last element.
        valToIndex.remove(val); // Remove the value from the map.
        return true;
    }
    public int getRandom() {
        return nums.get(rand.nextInt(nums.size())); // Return a random element.
    }
}
```

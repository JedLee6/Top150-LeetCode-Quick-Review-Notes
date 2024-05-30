# 066_LinkedList_86. Partition List

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/partition.jpg)

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

**Example 2:**

```
Input: head = [2,1], x = 2
Output: [1,2]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 200]`.
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`



### Main Idea

To achieve the desired partitioning while maintaining the original relative order, we can use a two-pointer technique with two separate lists:
1. One list (`before`) will contain nodes with values less than \( x \).
2. The other list (`after`) will contain nodes with values greater than or equal to \( x \).

After processing all nodes, we can concatenate these two lists to form the final partitioned list.

### Detailed Explanation with Code

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    public ListNode partition(ListNode head, int x) {
        // Create two dummy nodes: `before_head` and `after_head` to be the heads of the `before` and `after` lists respectively
        ListNode before_head = new ListNode(0);
        ListNode after_head = new ListNode(0);
        // Pointers to build the 'before' and 'after' lists
        ListNode before = before_head;
        ListNode after = after_head;
        // Iterate through the original list to build the 2 sub-lists, and for each node, compare its value with x
        while (head != null) {
            // If the current node's value is less than x, append it to the 'before' list
            if (head.val < x) {
                before.next = head;
                before = before.next;
            } else {
                // If the current node's value is greater than or equal to x, append it to the 'after' list
                after.next = head;
                after = after.next;
            }
            // Move to the next node in the original list
            head = head.next;
        }
        // Ensure the 'after' list ends properly
        after.next = null;
        // Connect the 'before' list to the 'after' list
        before.next = after_head.next;
        // The head of the partitioned list is the next node of 'before_head'
        return before_head.next;
    }
}
```

### Time Complexity

The time complexity of this solution is \( O(n) \), where \( n \) is the number of nodes in the linked list. This is because we traverse the list once.

### Space Complexity

The space complexity is \( O(1) \) extra space, as we only used a constant amount of extra space for the pointers.



### Alternative Solution

An alternative approach could involve creating a new list by reallocating nodes from the original list, but this would not be efficient or necessary given the simplicity and effectiveness of the two-pointer technique described above.

### Explanation of Alternative Solution

1. **Using Extra Space (Array List)**:
   - Traverse the list and store nodes in an array list.
   - Use two additional lists to store values less than \( x \) and values greater than or equal to \( x \).
   - Combine these two lists.

This alternative solution, however, increases the space complexity to \( O(n) \) due to the extra array list used for storage.

### Conclusion

The two-pointer technique with `before` and `after` lists is optimal for this problem as it ensures \( O(n) \) time complexity and \( O(1) \) extra space, maintaining the required relative order of nodes efficiently.
# 059_LinkedList_21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.



### Intuition and Main Idea

The fundamental idea to approach this problem is similar to the "merge" part of the merge sort algorithm. You compare the current nodes of both lists, appending the smaller one to your result list and moving the pointer forward in that list. This is repeated until all nodes from both lists are consumed.

### Approach and Detailed Explanation

Let's break down the approach step-by-step:

1. **Initialize**: Start with a dummy node. This node acts as a placeholder to easily return the head of the merged list later and helps handle edge cases smoothly.
2. **Comparison and Linking**: Traverse both lists, comparing the current nodes. Attach the smaller node to the merged list and advance in that list.
3. **Exhaust Remaining Nodes**: Once one list is exhausted, connect the remainder of the other list to the merged list.

#### Java Implementation with Detailed Annotations

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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Dummy node to handle edge cases and simplify the merge process
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy; // The merged list pointer
        // Traverse both lists and compare each node, continue as long as there are elements in both lists to compare.
        while (list1 != null && list2 != null) {
            // Decide which node to attach behind the current node based on the smaller value
            if (list1.val <= list2.val) {
                current.next = list1; // Link the smaller node to the merged list
                list1 = list1.next;  // Move forward in list1
            } else {
                current.next = list2;
                list2 = list2.next;  // Move forward in list2
            }
            current = current.next;  // Move the merged list pointer forward
        }
        // After the loop, if one of the lists is not exhausted, link the remainder
        if (list1 != null) {
            current.next = list1;  // Attach the remainder of list1
        } else if (list2 != null) {
            current.next = list2;  // Attach the remainder of list2
        }
        // Return the merged list, starting from the next node of dummy as dummy is a placeholder
        return dummy.next;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n + m), where \( n \) and \( m \) are the lengths of the two lists. Each element from both lists is visited once.
- **Space Complexity**: O(1). No additional space is used; the nodes are just rearranged.

### Conclusion

This solution efficiently merges two sorted lists into one by sequentially comparing their elements, which utilizes the inherent order of the lists. The use of a dummy node simplifies edge case management and improves code clarity. This approach is optimal in terms of time and uses minimal space, making it well-suited for interview discussions and practical applications.
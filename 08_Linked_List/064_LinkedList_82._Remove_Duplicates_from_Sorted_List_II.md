# 064_LinkedList_82. Remove Duplicates from Sorted List II

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list **sorted** as well*.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/linkedlist1-20240528213451534.jpg)

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/linkedlist2-20240528213451634.jpg)

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.



### Intuition and Main Idea

The main idea to solve this problem is to traverse the linked list and identify nodes that have duplicate values. By maintaining pointers to track the current node and its previous distinct node, we can remove duplicates efficiently. The key is to skip all nodes that have the same value as the current node when a duplicate is detected.

### Detailed Solution with Code and Explanation

#### Approach 1: Using a Dummy Node

**Step-by-Step Explanation**:

- Use a dummy node to handle edge cases easily.
- Use two pointers: `prev` and `current`.
- Traverse the list to detect duplicates.
- Skip duplicate nodes by adjusting pointers accordingly.

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
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        // Dummy node to handle edge cases where the head node could be removed
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // Initialize previous and current pointers, `prev` starts from the dummy node
        ListNode prev = dummy;
        // `current` starts from the head
        ListNode current = head;
        // Traverse the list
        while (current != null) {
            // Used to check for duplicates
            boolean hasDuplicate = false;
            // Check if `current` has the same value as `current.next`
            while (current.next != null && current.val == current.next.val) {
                // If it does, mark `hasDuplicate` as true and move `current` to the next node
                current = current.next;
                hasDuplicate = true;
            }
            // If duplicates were found, adjust `prev.next` to skip all duplicates by pointing it to `current.next`
            if (hasDuplicate) {
                prev.next = current.next;
            } else {
                // If no duplicates were found, move `prev` to `current`
                prev = prev.next;
            }
            //Move `current` to the next node, as it's for preparing for the next iteration
            current = current.next;
        }
        return dummy.next; // Return the `next` pointer of the dummy node, which is the new head of the list
    }
}
```

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list in a single pass.

##### Space Complexity

The space complexity is \(O(1)\) because we only use a few extra pointers and no additional data structures.

#### Approach 2: Using Recursion

**Step-by-Step Explanation**:

- Use recursion to traverse the list.
- Skip duplicate nodes by checking the value of the next node.
- Recursively adjust pointers to skip duplicates.

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        if (head.val == head.next.val) {
            // Skip nodes with the same value
            while (head.next != null && head.val == head.next.val) {
                head = head.next;
            }
            // Move to the next non-duplicate node
            return deleteDuplicates(head.next);
        } else {
            // No duplicates, move to the next node
            head.next = deleteDuplicates(head.next);
            return head;
        }
    }
}
```

##### Explanation

1. **Base Case**: If the list is empty or has only one node, return the head.
2. **Check Duplicates**: If the current node's value is the same as the next node's value, skip all nodes with the same value.
3. **Recursion**: Recursively call `deleteDuplicates` on the node after the last duplicate.
4. **Adjust Pointers**: If no duplicates are found, set the `next` pointer of the current node to the result of the recursive call.
5. **Return Head**: Return the current node as the new head of the list.

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list in a single pass, albeit recursively.

##### Space Complexity

The space complexity is \(O(n)\) due to the recursion stack, where \(n\) is the depth of the recursion, which in the worst case is the number of nodes in the list.

### Conclusion

Both approaches effectively remove duplicates from the sorted linked list, ensuring only distinct nodes remain. The dummy node approach is iterative and generally more space-efficient, while the recursive approach offers a more elegant and readable solution but at the cost of higher space complexity due to the recursion stack. The choice between these methods depends on specific constraints and preferences for implementation complexity.




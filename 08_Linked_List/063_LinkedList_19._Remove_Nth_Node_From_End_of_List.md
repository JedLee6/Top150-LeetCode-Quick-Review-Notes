# 063_LinkedList_19._Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

 

**Follow up:** Could you do this in one pass?



### Intuition and Main Idea

The main idea to solve this problem is to use two pointers to find the nth node from the end of the list efficiently. By using a **two-pointer Approach**, we can make one pass through the list to find and remove the target node. This method ensures that we don't need to traverse the list multiple times, which improves efficiency.

- Use two pointers: `first` and `second`.
- Move `first` pointer (n + 1) steps ahead of `second`.
- Move both pointers until `first` reaches the end of the list.
- `second` will be at the node before the one we need to remove.
- Make second node points to the node 2 steps ahead to remove the target node.

### Code and Explanation

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // We use a dummy node to handle edge cases like removing the first node.
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // Initialize two pointers pointing to the dummy node
        ListNode first = dummy;
        ListNode second = dummy;
        // Move `first` pointer n+1 steps ahead to maintain a gap of n between `first` and `second`. We moved 1 extra step, as we wanna `second` will be at the node just before the target node to be removed.
        for (int i = 0; i < n + 1; i++) {
            first = first.next;
        }
        // Move both pointers until first reaches the end
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        // Finally, `second` will be at the node just before the target node to be removed
        // Make second node points to the node 2 steps ahead to remove the target node
        second.next = second.next.next;
        return dummy.next; // Return the `next` pointer of the dummy node, which is the new head of the list.
    }
}
```

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list in a single pass.

##### Space Complexity

The space complexity is \(O(1)\) because we only use a few extra pointers and no additional data structures.

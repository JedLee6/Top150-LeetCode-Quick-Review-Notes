# 062_LinkedList_25. Reverse Nodes in k-Group

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/reverse_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/reverse_ex2-20240526215447256.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?



### Intuition and Main Idea

The main idea to solve this problem is to reverse every k nodes in the linked list. If the number of nodes remaining is less than k, those nodes should remain in their original order. The process involves three main steps:

1. Traverse the list to identify groups of k nodes.
2. Reverse the nodes within each group.
3. Reconnect the reversed groups with the rest of the list.

To achieve this, we need to handle pointers carefully to keep track of the nodes before and after the group being reversed, as well as the current nodes being processed.

### Detailed Solution with Code and Explanation

#### Iterative Approach

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) return head;
        // Dummy node to handle edge cases more easily
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // `cur`, `nex`, and `pre` are used to track the current node, the next node, and the node before the group to be reversed.
        ListNode cur = dummy, nex = dummy, pre = dummy;
        int count = 0;
        // Traverse the list to count the total number of nodes
        while (cur.next != null) {
            cur = cur.next;
            count++;
        }
        // While there are at least k nodes left to reverse
        while (count >= k) {
            cur = pre.next; // The first node in the k-group
            nex = cur.next; // The node following the first node in the k-group
            // For each group of k nodes, we reverse the k nodes within the group using a nested loop
            for (int i = 1; i < k; i++) {                
                cur.next = nex.next; // Point the `cur` node to the node after `nex` node to connect the reversed groups to the latter part of the list
                nex.next = pre.next; // Move `nex` node to the front of the k-group
                pre.next = nex; // Point the `pre` node to the 'nex' node to connect the former part of the list to the reversed groups
                nex = cur.next; // Update the 'nex' node to the subsequent node, which will to be reversed in the following iteration
            }
            pre = cur; // Move pre to the end of the reversed k-group
            count -= k; // Decrease the node count by k
        }
        return dummy.next; // Return the next node of the dummy node, which is the new head of the reversed list.
    }
}
```

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list once to count the nodes and then again to reverse the nodes in groups of k.

##### Space Complexity

The space complexity is \(O(1)\) because we only use a few extra pointers and no additional data structures.

#### Recursive Approach

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        // Base case
        if (head == null) return null;

        // Check if there are at least k nodes left
        ListNode curr = head;
        int count = 0;
        while (curr != null && count < k) {
            curr = curr.next;
            count++;
        }

        // If we have k nodes, then we reverse them
        if (count == k) {
            // Reverse the first k nodes
            ListNode reversedHead = reverseLinkedList(head, k);
            // Recursively reverse the remaining nodes
            head.next = reverseKGroup(curr, k);
            return reversedHead;
        }

        // If we don't have k nodes, return the head as it is
        return head;
    }

    private ListNode reverseLinkedList(ListNode head, int k) {
        ListNode prev = null;
        ListNode curr = head;

        while (k > 0) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
            k--;
        }

        return prev;
    }
}
```

##### Explanation

1. **Base Case**: If the list is empty, we return null.
2. **Counting Nodes**: We check if there are at least k nodes left in the list.
3. **Reversing Groups**: If we have k nodes, we reverse the first k nodes and recursively call the function for the remaining nodes.
4. **Reversing Function**: A helper function to reverse k nodes in the list.
5. **Returning the Result**: The head of the reversed list is returned, connecting the recursively processed parts.

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we process each node exactly once.

##### Space Complexity

The space complexity is \(O(n/k)\) due to the recursion stack, which can go as deep as the number of groups we are reversing.

### Conclusion

Both solutions effectively reverse nodes in groups of k, handling edge cases and ensuring nodes are correctly reconnected. The iterative approach is generally more space-efficient, while the recursive approach can be more intuitive and easier to understand. Choosing between them depends on specific constraints and preferences for iterative versus recursive paradigms.




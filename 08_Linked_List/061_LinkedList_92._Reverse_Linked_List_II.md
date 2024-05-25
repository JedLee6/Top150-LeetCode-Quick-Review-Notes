# 061_LinkedList_92._Reverse_Linked_List_II

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*. 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?



### Intuition and Main Idea

To solve the problem of reversing a segment of a singly linked list between two given positions `left` and `right`, we need to carefully handle the pointers to ensure that only the specified segment is reversed while the rest of the list remains unchanged. 

The main idea is to:
1. Traverse the list to find the starting point of the segment to be reversed.
2. Reverse the segment.
3. Reconnect the reversed segment with the rest of the list.

We can achieve this in one pass through the list to position ourselves correctly and another pass to reverse the segment, making the solution efficient.

### Step-by-Step Solution with Detailed Explanation

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // Edge case: if the list is empty or left equals right, no need to reverse
        if (head == null || left == right) {
            return head;
        }
        // Dummy node to simplify the reversal process at the head of the list, for example, the head node could be reversed, so a dummy node can help up always find the final correct head code
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        // Step 1: Move prev to the node before the position left
        for (int i = 1; i < left; i++) {
            prev = prev.next;
        }
        // Step 2: Reverse the sublist from left to right
        ListNode start = prev.next;  // Set `start` to the first node of the sublist that needs to be reversed, `start` node wouldn't change during the iteration
        ListNode then = start.next;  // `then` is the node that will be moved to the front of the sublist during each iteration, so `then` node would change during each iteration
        // Create a for loop to reverse the sublist by rearranging the pointers
        for (int i = 0; i < right - left; i++) {
            start.next = then.next; // Connect the `start` node to the next node of the `then` node
            then.next = prev.next; // Move the 'then' node to the front of the sublist
            prev.next = then; // Connect the prev node to the 'then' node
            then = start.next; // Update the 'then' node to the next node to be reversed in the next iteration
            // Repeate the same steps for each subsequent node in the sublist until the entire segment is reversed
        }
        return dummy.next; // Return the new head of the list
    }
}
```

### Walkthrough Graph of Solving Problem

![0_1490008790876_reversed_linked_list.jpeg](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/1490008792563-reversed_linked_list.jpeg)

### Analysis of Time Complexity and Space Complexity

#### Time Complexity

- **Traversing to the left position**: This takes \(O(left)\) time.
- **Reversing the sublist**: This takes \(O(right - left)\) time since we only need to reverse the nodes between positions `left` and `right`.

Overall, the time complexity is \(O(n)\), where \(n\) is the number of nodes in the list.

#### Space Complexity

- The space complexity is \(O(1)\) because we are only using a few pointers and not any additional data structures that grow with the input size.

### Alternative Solution: Iterative Approach Without Dummy Node

We can solve this problem without using a dummy node by carefully handling the head of the list.

#### Step-by-Step Solution

```java
public class Solution {

    // Function to reverse the linked list between positions left and right without dummy node
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // Edge case: if the list is empty or left equals right, no need to reverse
        if (head == null || left == right) {
            return head;
        }

        ListNode prev = null;
        ListNode current = head;

        // Step 1: Traverse the list to reach the left position
        for (int i = 1; i < left; i++) {
            prev = current;
            current = current.next;
        }

        // Step 2: Start the reversal process
        ListNode connection = prev;  // Node before the left position
        ListNode tail = current;     // The first node of the sublist to be reversed

        ListNode next = null;
        for (int i = 0; i <= right - left; i++) {
            next = current.next;     // Store the next node
            current.next = prev;     // Reverse the current node's pointer
            prev = current;          // Move prev to the current node
            current = next;          // Move to the next node
        }

        // Step 3: Reconnect the reversed sublist with the rest of the list
        if (connection != null) {
            connection.next = prev;  // Connect the node before left to the new head of the sublist
        } else {
            head = prev;             // If left was 1, update the head of the list
        }
        tail.next = current;         // Connect the tail of the sublist to the remaining part of the list

        return head;  // Return the new head of the list
    }
}
```

### Analysis of Time Complexity and Space Complexity for the Alternative Solution

#### Time Complexity

- **Traversing to the left position**: This takes \(O(left)\) time.
- **Reversing the sublist**: This takes \(O(right - left)\) time.

Overall, the time complexity is \(O(n)\), where \(n\) is the number of nodes in the list.

#### Space Complexity

- The space complexity is \(O(1)\) because we are only using a few pointers and not any additional data structures that grow with the input size.

### Conclusion

Both solutions effectively reverse the segment of the linked list between positions `left` and `right`. The use of a dummy node simplifies the reconnection process, especially when reversing a segment that includes the head of the list. The alternative solution without a dummy node is slightly more complex but achieves the same result with the same time and space complexity. The choice between these methods can depend on personal preference or specific constraints of the problem.
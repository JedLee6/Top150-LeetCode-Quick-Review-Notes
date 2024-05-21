# 058_LinkedList_2. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/addtwonumber1.jpg)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

**Example 2:**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

 

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.



### Intuition and Main Idea

The fundamental idea for solving this problem is similar to how you would add two numbers on paper. We'll start with the least significant digits (the heads of the linked lists) and proceed to more significant digits, keeping track of any carry-over from each digit summation.

> carry-over: It represents the process of carrying a digit over to the next higher place value when the result of an addition operation exceeds a certain threshold, typically 10 in the base-10 decimal system.

### Approach and Step-by-Step Explanation

We'll use a single pass approach, traversing both linked lists simultaneously and adding corresponding digits. The carry resulting from each addition will be carried over to the next node's sum.

#### Java Implementation and Detailed Explanation

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // Initialize a dummy head node to simplify handling edge cases easily, which is the first node of the new Linked List of the two numbers' sum
        ListNode dummyHead = new ListNode(0);
        // Point to the trailing node for connecting the next node
        ListNode current = dummyHead;
        int carry = 0;
        // Traverse both lists, and continue processing as long as there is at least one node left in either list.
        while (l1 != null || l2 != null) {
            // Get the values of the current nodes, if present
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            // Calculate the sum of the values and the carry
            int sum = carry + x + y;
            carry = sum / 10;  // Update carry for next iteration
            int lastDigit = sum % 10;  // The last digit of the sum
            // Create a new node with the last digit and move the pointer
            current.next = new ListNode(lastDigit);
            current = current.next;
            // Move to the next nodes in the lists, if available
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        // If there is a carry left after the last addition
        if (carry > 0) {
            // Link it to the current node
            current.next = new ListNode(carry);
        }
        // The head of the new linked list is next to the dummy head
        return dummyHead.next;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(max(n, m)), where n and m are the lengths of the two linked lists. Each digit is processed once.
- **Space Complexity**: O(max(n, m)), the length of the new list is at most max(n, m) + 1 (in case there is a carry-over that adds an extra digit).

### Conclusion
This approach effectively and efficiently adds two numbers represented as linked lists by simulating the traditional method of addition. It handles edge cases smoothly with the use of a dummy node and operates within optimal time and space complexities.
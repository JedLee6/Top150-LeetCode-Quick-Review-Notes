# 065_LinkedList_61. Rotate List

Given the `head` of a linked list, rotate the list to the right by `k` places.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/rotate1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/roate2.jpg)

```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 500]`.
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 109`



### Intuition and Main Idea

To solve the problem of rotating a linked list to the right by \( k \) steps, the main idea is to first make the list circular and then break the circularity at the correct position. By converting the list to a circular list, we can easily handle rotations that are greater than the length of the list by using modulo operation.

### Reasons for Using Modulo Operation

When we rotate a list of length n by n steps, it brings every element back to its original position. So, when k is greater than the length of the list, the effective number of rotations is the remainder of k mod n. Therefore, we only rotate the linked list to the right by ( k mod n ) steps to avoid unnecessary rotations.

1. **Cyclic Nature of Rotations**:
    - When you rotate a list of length n by n positions, it results in the same list. This is because rotating a list by its length brings every element back to its original position.
    - Therefore, rotating the list by any multiple of n (i.e., k=mn for any integer m) will also result in the same list.
2. **Handling Large k Values**:
    - When k is greater than the length of the list, the effective number of rotations is reduced by considering only the remainder of k mod n.
    - Using k mod n effectively reduces the problem to a simpler one where k is within the bounds of the list length. This simplifies calculations and avoids unnecessary rotations.
3. **Simplifying the Problem**:
    - By reducing k using k mod n, we convert the problem into one where k is less than or equal to the length of the list.
    - This simplification means we only need to handle k within a single traversal cycle of the list, making the solution more efficient.

### Detailed Solution with Code and Explanation

#### Approach 1: Making a Circular List and Breaking It

**Step-by-Step Explanation**:

- Traverse the list to find its length and the last node.
- Connect the last node to the head to make the list circular.
- Find the new head position by using the length of the list and the given \( k \) value.
- Break the circularity at the correct position to form the new rotated list.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // Check if the list is empty, has only one node, or if \( k \) is zero. In such cases, return the head as is.
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        // Calculate the length of the list and get the last node
        ListNode current = head;
        int length = 1; // Initialize length as 1 since we start from head
        while (current.next != null) {
            current = current.next;
            length++;
        }
        // Connect the last node to the head to make it circular
        current.next = head;
        // Calculate the new head position using the modulo operation to handle cases where k is greater than the length of the list
        // When we rotate a list of length n by n steps, it brings every element back to its original position. So, when k is greater than the length of the list, the effective number of rotation steps is the remainder of k mod n. Therefore, we only rotate the linked list to the right by ( k mod n ) steps to avoid unnecessary rotations.
        int newHeadPosition = length - k % length;
        // Traverse the linked list to find the node just before the new head
        ListNode newTail = head;
        for (int i = 0; i < newHeadPosition - 1; i++) {
            newTail = newTail.next;
        }
        // The new head is the next node of newTail
        ListNode newHead = newTail.next;
        // Break the circular connection to form the new list starting from the new head
        newTail.next = null;
        return newHead;
    }
}
```

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list a couple of times.

##### Space Complexity

The space complexity is \(O(1)\) because we only use a few extra pointers and no additional data structures.

#### Approach 2: Two-Pointer Technique

**Step-by-Step Explanation**:

- Use two pointers to find the new head position.
- Move the first pointer \( k \) steps ahead.
- Move both pointers until the first pointer reaches the end.
- Adjust pointers to form the rotated list.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        // Calculate the length of the list
        ListNode current = head;
        int length = 1;
        while (current.next != null) {
            current = current.next;
            length++;
        }
        // Find the effective rotation count
        k = k % length;
        if (k == 0) {
            return head;
        }
        // Set up two pointers
        ListNode fast = head;
        ListNode slow = head;
        // Move fast pointer k steps ahead
        for (int i = 0; i < k; i++) {
            fast = fast.next;
        }
        // Move both pointers until fast reaches the end
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        // Adjust pointers to form the rotated list
        ListNode newHead = slow.next;
        slow.next = null;
        fast.next = head; 
        return newHead;
    }
}
```

##### Explanation

1. **Edge Cases**: Check if the list is empty, has only one node, or if \( k \) is zero. In such cases, return the head as is.
2. **Calculate Length**: Traverse the list to calculate its length.
3. **Effective Rotation**: Use modulo operation to find the effective rotation count, handling cases where \( k \) is greater than the length.
4. **Two Pointers**: Initialize `fast` and `slow` pointers at the head. Move `fast` pointer \( k \) steps ahead.
5. **Move Pointers**: Move both pointers until `fast` reaches the end.
6. **Adjust Pointers**: Set `newHead` as the node after `slow`, break the connection by setting `slow.next` to null, and connect `fast.next` to the original head to form the rotated list.

##### Time Complexity

The time complexity of this solution is \(O(n)\), where \(n\) is the number of nodes in the linked list. This is because we traverse the list a couple of times.

##### Space Complexity

The space complexity is \(O(1)\) because we only use a few extra pointers and no additional data structures.

### Conclusion

Both approaches effectively rotate the linked list to the right by \( k \) places. The first approach makes the list circular and breaks it at the correct position, while the second approach uses two pointers to find the new head position and adjust pointers accordingly. Both methods handle edge cases and ensure the list is rotated efficiently with \(O(n)\) time complexity and \(O(1)\) space complexity. The choice between these methods depends on specific constraints and preferences for implementation complexity.
# 057_LinkedList_141._Linked_List_Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

 

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/circularlinkedlist-20240520215444996.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/circularlinkedlist_test3-20240520215448226.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

 

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?



### Problem Understanding
We are given the head of a linked list and need to determine whether the list contains a cycle. A cycle occurs if a node's next pointer points back to a previous node in the list, causing an infinite loop.

### Intuition and Main Idea
To detect a cycle, the main challenge is identifying a repeating sequence without using extra space to store nodes already seen. Two popular approaches can be applied:

1. **Hash Set Method**: Traverse the list and store each node in a hash set. If a node reappears, a cycle is detected.
2. **Floyd's Cycle Detection Algorithm (Two-Pointer Approach)**: Use two pointers moving at different speeds. If there's a cycle, they will eventually meet.

### Solution 1: Hash Set Method

This method uses extra space to keep track of visited nodes. As we traverse the linked list, each node is added to a hash set. If we attempt to add a node already present in the set, a cycle is detected.

#### Java Implementation
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<>();
        ListNode current = head;
        while (current != null) {
            if (seen.contains(current)) {
                return true; // Cycle detected
            }
            seen.add(current);
            current = current.next;
        }
        return false; // No cycle found
    }
}
```
**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the number of nodes in the linked list.
- **Space Complexity:** O(n), due to the space required for the hash set.

### Solution 2: Floyd's Cycle Detection Algorithm (Two-Pointer Approach, Tortoise and Hare)

This is a space-efficient method using two pointers at different speeds (slow and fast). The slow pointer moves one step at a time, while the fast pointer moves two steps. If there is a cycle, the fast pointer will eventually "lap" the slow pointer.

#### Java Implementation
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) return false;
        ListNode slow = head; // Moves one step at a time
        ListNode fast = head.next; // Moves two steps at a time
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false; // No cycle if fast pointer reaches the end
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true; // Cycle detected when slow and fast meet
    }
}
```
**Complexity Analysis:**
- **Time Complexity:** O(n) for n nodes in the worst case. If a cycle exists, it would be detected faster.
- **Space Complexity:** O(1), as no additional space is used other than the two pointers.

### Conclusion
Both methods have their use cases:
- The Hash Set method is straightforward but uses additional memory.
- Floyd's Tortoise and Hare method is highly space-efficient and generally preferred in scenarios where space complexity is a concern.

In an interview, explaining both methods demonstrates an understanding of different approaches and a grasp of trade-offs in space and time complexity. This provides a comprehensive answer that showcases problem-solving skills and knowledge of data structures.
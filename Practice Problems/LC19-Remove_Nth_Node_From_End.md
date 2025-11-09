# Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

## Solutions

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
```

### Two Pointers (One Pass)

Because you do not know the length of the list and need to remove the item relative to the end, you can instead have two pointers. The first pointer will be faster and lead `n` spots ahead in the list. This way, once the leader hits the end, you know that the follower pointer is at the correct position to remove the node.

An important trick here is to use a `dummy` node attached before the head. This is a great way to have th

Because this is done in Java, you do not need to release the memory for the deleted node, although you would have to with a C language.

As another important note, although generally a one-pass solution is faster and more efficient compared to a two pass solution, in this case it might be slightly worse. This is mostly theoretical and useless talk, however.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode leader = head;
        for (int i = 0; i < n; i++) {
            leader = leader.next;
        }

        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        while (leader != null) {
            curr = curr.next;
            leader = leader.next;
        }

        curr.next = curr.next.next;

        return dummy.next;
    }
}
```

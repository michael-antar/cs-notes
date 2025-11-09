# Reorder List

You are given the head of a singly linked-list. The list can be represented as:

> L0 → L1 → … → Ln - 1 → Ln

_Reorder the list to be on the following form_:

> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

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

### Iterative

You can use a strategy that involves 3 main steps (that all take $O(n)$ time each):

- Find and split the list down the middle
- Reverse the second half of the list
- Stich together the first and second halfs to get the result

This solution also only uses $O(1)$ space since all of these actions are performed in-place.

You must also cut off the first half from the second half in order to prevent an infinite loop in the stiching process

```java
class Solution {
    public void reorderList(ListNode head) {
        ListNode middle = findMiddle(head);
        ListNode second = middle.next;
        middle.next = null; // Cut off first half from second half

        ListNode reversed = reverseList(second); // Reverse second half of list

        // Stitch
        while (reversed != null) {
            ListNode next1 = head.next;
            ListNode next2 = reversed.next;

            head.next = reversed;
            reversed.next = next1;

            head = next1;
            reversed = next2;
        }
    }

    private ListNode findMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }

    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {
            ListNode nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }

        return prev;
    }
}
```

### Recursive

TODO

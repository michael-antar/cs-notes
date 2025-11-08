# Reverse Linked List

Given the `head` of a **singly linked list**, reverse the list, and _return the reversed list_.

## Solution

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

### Recursive

This involves breaking the problem down to its **base case**: Last node in the list (the new head). Once you cover the base case, you now know that you have the final head to pass throughout the unravel, and that the list up-until this point has been properly reversed. For the **recursive step**, you have to do 2 things: Flip the pointer of the next node to the current node (since you do not have access to the previous in the stack call), then clean up the connection of the current node (that was originally used to access the next node). You can then continue to return the "true" head.

You must also cover the **edge case** of being given an empty list (initial head of null). In this case, just return null.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        // Edge case (empty list)
        if (head == null) {
            return null;
        }

        // Base case (last node)
        if (head.next == null) {
            return head; // The "true" head to pass down
        }

        // Recursive step: Reverses sub-problem, returns "true" head
        ListNode newHead = reverseList(head.next);

        head.next.next = head; // Pointer flip
        head.next = null; // Cleanup link

        // Continue passing down the "true" head
        return newHead;
    }
}
```

### Iterative

This solution involves iterating through the list, and keeping track of the next node before reversing the chain of the current node.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while(curr != null) {
            ListNode nextNode = curr.next; // Store next before breaking chain
            curr.next = prev; // Reverse current node link

            // Iterate
            prev = curr;
            curr = nextNode;
        }

        return prev;
    }
}
```

### Non-Destructive

This is an iterative, non-destructive solution. You iterate through the original list, adding a new node to the new list using the previously stored node.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode newHead = null; // Will hold new head
        ListNode curr = head; // Iterator for original list

        while (curr != null) {
            ListNode newNode = new ListNode(curr.val, newHead);
            newHead = newNode; // Promote new node to be new head
            curr = curr.next;
        }

        return newHead;
    }
}
```

You are given the heads of two sorted [[Linked Lists|linked lists]] `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return the _head of the merged linked list_.

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
This solution involves using the same nodes within both lists and "stitching" them together. You loop through both, adding the smaller item, rewriting the `node.next` as you go. You are done once at least one of the lists are completed. You then can just stitch in the remainder of the other list and be completed.

A great trick here is to use a **dummy node**. This gives a great starting head to continue with the loop, without having to add additional logic for the first one. You just return `dummy.next` as the final result.

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode();
        ListNode curr = dummy;
		
        while(list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                curr.next = list1;
                list1 = list1.next;
            }
            else {
                curr.next = list2;
                list2 = list2.next;
            }
            curr = curr.next;
        }
        curr.next = list1 != null ? list1 : list2; // Add remainder of other list if needed
		
        return dummy.next;
    }
}
```

### Recursive
A slightly worse solution that uses $O(n + m)$ time uses recursion. You can use the **base case** of one of the lists being empty to return the other list, and then as the **recursive step**, add to the smaller value the result of the recursion, returning the list you are stitching to.

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
		
        if (list1.val <= list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }
        else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

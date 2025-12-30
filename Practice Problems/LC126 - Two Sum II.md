Given a **1-indexed** array of integers `numbers` that is **_already sorted in non-decreasing order_**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is **exactly one solution**. You may not use the same element twice.

Your solution must use only **constant extra space**.
## Solution
The constant extra space constraint means that you cannot use a map to store previously seen numbers like in Two Sum 1. Because the list is already sorted, you can use two [[Pointers|pointers]] starting on opposite sides, and move them closer to the middle depending on the current sum compared to the target. If the sum is larger than the target, you know you can move the end pointer down, and vice versa.

This uses $O(n)$ time because even in the worst case, both pointers combined will come to meet each other for a length of n. It uses $O(1)$ space since only using space for the two pointer indexes.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int index1 = 0;
        int index2 = numbers.length - 1;
		
        while(index1 < index2) {
            int total = numbers[index1] + numbers[index2];
            if (total < target) {
                index1++;
            }
            else if (total > target) {
                index2--;
            }
            else {
                // Found target
                return new int[]{index1 + 1, index2 + 1};
            }
        }
		
        // Something went wrong
        return new int[]{-1, -1};
    }
}
```

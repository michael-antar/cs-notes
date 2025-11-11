# Binary Search

## Solutions

### Iterative

A very simple problem with a small note. A common old way to calculate the position of the middle was `(left + right) / 2`. Although this normally gives the same result as the modern one, it risks integer overflow if both `left` and `right` are large.

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int middle = left + ((right - left) / 2)

            if (nums[middle] < target) {
                left = middle + 1;
            }
            else if (nums[middle] > target) {
                right = middle - 1;
            }
            else {
                return middle;
            }
        }

        return -1;
    }
}
```

### Recursive

For the recursive solution, you can use a base case for when you know there is no result, or when you found the result. For the recursive step, you can subdivide the problem into its left or right half depending on the target.

Note: In Java, passing an object like the `target` array only passes the reference pointer to it, so the call stack will not be filled with many instances of the same array.

```java
public class Solution {
    public int binary_search(int l, int r, int[] nums, int target) {
        if (l > r) return -1;
        int m = l + (r - l) / 2;

        if (nums[m] == target) return m;
        return (nums[m] < target) ?
            binary_search(m + 1, r, nums, target) :
            binary_search(l, m - 1, nums, target);
    }

    public int search(int[] nums, int target) {
        return binary_search(0, nums.length - 1, nums, target);
    }
}
```

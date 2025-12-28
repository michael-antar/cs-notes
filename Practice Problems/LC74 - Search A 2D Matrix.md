You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, _return `true` if `target` is in `matrix` or `false` otherwise_.

You must write a solution in $O(log(m * n))$ time complexity.

## Solutions
### Binary Search (2-Pass)
The most "textbook" solution to this problem is to use 2 variants of [[Binary Search|binary searches]]:

- A **bounds-binary search** to find the _largest possible_ row that _could_ have the target.
- A normal binary search to find if the target exists within the row.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int minRow = 0;
        int maxRow = matrix.length - 1;
        int correctRow = 0;
		
        while (minRow <= maxRow) {
            int row = minRow + ((maxRow - minRow) / 2);
			
            if (matrix[row][0] > target) {
                maxRow = row - 1;
            } else {
                correctRow = row;
                minRow = row + 1;
            }
        }
		
        int left = 0;
        int right = matrix[0].length - 1;
		
        while (left <= right) {
            int middle = left + ((right - left) / 2);
			
            if (matrix[correctRow][middle] < target) {
                left = middle + 1;
            } else if (matrix[correctRow][middle] > target) {
                right = middle - 1;
            } else {
                return true;
            }
        }
		
        return false;
    }
}
```

### Virtual 1D Array (1-Pass)
A clever solution pretends that the `m x n` matrix is just one big sorted `m * n` length [[Array|array]].

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
		
        // Search space is from index 0 to (m * n) - 1
        int left = 0;
        int right = (m * n) - 1;
		
        while (left <= right) {
            int middle = left + (right - left) / 2;
			
            // "Translate" the 1D middle index back to 2D (row, col)
            int row = middle / n;
            int col = middle % n;
            int middleVal = matrix[row][col];
			
            // This is now a standard, 3-way binary search
            if (middleVal < target) {
                left = middle + 1;
            } else if (middleVal > target) {
                right = middle - 1;
            } else {
                return true;
            }
        }
		
        return false;
    }
}
```

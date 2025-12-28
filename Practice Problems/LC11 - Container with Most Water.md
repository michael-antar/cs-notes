You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

## Solution
You first have to come up with the equation to find the area (`area = min(height[x1], height[x2]) * (x2 - x1)`). This hints that the area of the container only really cares about the current shortest side.

You can use two [[Pointers|pointers]] to represent the left and right side of the container. You can use the idea that moving the _smaller_ side will **never** lead to missing a bigger container (since at worst, the other side would become the shorter one, and the width of the container will shrink).

```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        // Pointers starting on opposite ends, closing in
        int l = 0;
        int r = height.length - 1;
		
        while (l < r) {
            int area = Math.min(height[l], height[r]) * (r - l);
            maxArea = Math.max(maxArea, area);
			
            if (height[l] <= height[r]) {
                l++;
            }
            else {
                r--;
            }
        }
		
        return maxArea;
    }
}
```

# Koko Eating Bananas

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

_Return the minimum integer `k`_ such that she can eat all the bananas _within `h` hours_.

## Solution

This solution utlizes the "Lower Bound" binary search algorithm, where we are trying to find the _minimum_ value that satisfies the condition.

We can use the base principles that the smallest `k` is 1 and the largest possible `k` is the maximum value in the array.

You can calculate how long it takes Koko to eat the pile by dividing the pile by the speed and rounding up.

Logically, you can use the binary search ($O(logm)$) and check how long it takes to eat each pile ($O(n)$).

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int minSpeed = 1;
        int maxSpeed = Arrays.stream(piles).max().getAsInt();
        int result = maxSpeed;

        while (minSpeed <= maxSpeed) {
            int speed = minSpeed + ((maxSpeed - minSpeed) / 2);

            long totalTime = 0;
            for (int pile : piles) {
                totalTime += (int) Math.ceil((double) pile / speed);
            }

            if (totalTime > h) {
                minSpeed = speed + 1;
            } else {
                result = speed;
                maxSpeed = speed - 1;
            }
        }

        return result;
    }
}
```

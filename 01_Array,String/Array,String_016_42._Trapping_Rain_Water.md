## Array,String_016_42. Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![img](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2:**

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

**Constraints:**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

## Intuition

A key insight for this problem is understanding that the amount of water trapped on top of a bar is determined by the height of the tallest bars to its left and right. Specifically, the water level above a bar is the minimum of the tallest bar to its left and right minus the height of the bar itself.

## Approach

instead of computing the left and right parts separately, we may think of some way to do it in one iteration.

Suppose we start from both the left and right end by maintaining two pointers **left** and **right**. If the smaller tower is at the left end, water trapped would be dependent on the tower's height in the direction from left to right. Similarly, if the tower at the right end is smaller, water trapped would be dependent on the tower's height in the direction from right to left. So we first calculate water trapped on the smaller tower among height[left] and height[right] and move the pointer associated with the smaller tower.

Loop will run till the left pointer doesn't cross the right pointer. In terms of an analogy, we can think of height[left] and height[right] as a wall of a partial container where we fix the higher end and flow water from the lower end.

#### **Solution steps**

- We Initialise **left** and **right** pointers.

- We also Initialise variables **trappedWater**, **leftMax**, and **rightMax**.

  ```java
  int ans = 0
  int leftMax = INT_MIN, rightMax = INT_MIN
  int left = 0, right = n - 1
  ```

- Now we run a loop to scan the array i.e. while (left <= right)

  1. If **height[left] < height[right]** and **height[left] < leftMax**: We calculate the amount of rainwater trapped between both towers and increase the left pointer. But if **height[left] > leftMax**: We update the value of leftMax and increase the left pointer.

  ```java
  if (height[left] < height[right])
  {
      if (height[left] > leftMax)
          leftMax = height[left]
      else
          trappedWater = trappedWater + leftMax - height[left]
      left = left + 1
  }
  ```

  1. If **height[left] > height[right])** and **height[right] < rightMax**: We calculate the amount of rainwater trapped between both towers and decrease the right pointer. But if **height[right] > rightMax**: We update the value of rightMax and decrease the right pointer.

  ```java
  if (height[left] > height[right])
  {
      if (height[right] > rightMax)
          rightMax = height[right]
      else
          trappedWater = trappedWater + rightMax - height[right]
      right = right - 1
  }
  ```

- When left > right, then we exit the loop and return the value stored in the trappedWater.

## Complexity

Time complexity: Here the time complexity would be O(n) as there is only one loop running in the two pointers.

Space complexity: Here the space complexity is constant O(1), as we are not creating any extra space excluding the variables.

## Code

```java
public class Solution {
    public int trap(int[] h) {
      //the start element and end element defenitly can't trap water, so we initialize leftMax and rightMax with the value of the start element and end element
        int l = 0, r = h.length - 1, leftMax = h[l], rightMax = h[r], ans = 0;
        while (l < r) {
          //the water level above a bar is the minimum of the tallest bar to its left and right minus the height of the bar itself.
            if (leftMax < rightMax) {
                l++;
                leftMax = Math.max(leftMax, h[l]);
              // `leftMax - h[l]` won't be negative, as leftMax is the maximum value so far
                ans += leftMax - h[l];
            } else {
                r--;
                rightMax = Math.max(rightMax, h[r]);
                ans += rightMax - h[r];
            }
        }
        return ans;
    }
}
```

## Detailed Explanation

- Here the approach is like we basically find the left max and right max and based on that we start our movement in two pointer , first have a glance at the below depicted figure which is later followed by explaination.

![pic1.png](https://raw.githubusercontent.com/JedLee6/PublicPicBed/main/uPic/d5703973-8ea3-4427-8e28-a6cd53053572_1681157571.4794183.webp)

- As shown in the figure we start with finding the left most height and the right most height and then we do left++ , right-- and continue. Now if the new left height is greater than max left height then we update the lmax height and similarly for the right side.
- When This is not the case the we proceed with the side with the minimum height , say it's left for the further understanding , now we take the difference b/w the left heights and add to the water stored i.e `water += lmax - height[lpos];` or `water += rmax - height[rpos];` according to the current scenario as explained above.

- In the same way depicted above we further continue till the loop i.e ends `while(lpos <= ros)` then we would finally obtain the water which can be trapped during this process.
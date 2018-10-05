Question:

``` css
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Example 1:

Input: [1,3,5,6], 5
Output: 2
Example 2:

Input: [1,3,5,6], 2
Output: 1
Example 3:

Input: [1,3,5,6], 7
Output: 4
Example 4:

Input: [1,3,5,6], 0
Output: 0
```

Answer:
朴素方法：

``` js
searchInsert = function(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === target || nums[i] > target) return i;
    }
    return nums.length;
};
```

因为有序，所以二分方法:
``` js
searchInsert = (nums, target) => {
    let [l, h] = [0, nums.length - 1];
    let mid = -1;
    while (l <= h) {
        mid = l + h >> 1;
        if (nums[mid] < target) {
            l = mid + 1;
        } else if (nums[mid] > target) {
            h = mid - 1;
        } else {
            return mid;
        }
    }
    return mid;
};
```
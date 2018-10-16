Question:

``` css
Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example 1:

Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
Example 2:

Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

方法1： 快/慢 指针
``` js

/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
removeElement = function(nums, val) {
    const [i, j] = [0, 0];
    for (; i < nums.length; i++) {
        nums[i] !== val && (nums[j++] = nums[i]);
    }
    return nums.length = j;
};

// 问题分析： ([2, 3, 4, 5, 6], 2) ，这样的数组，后面的都要移动。。
```

方法2： 和最后一个交换然后释放
``` js

/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = (nums, val) => {
    let [i, len] = [0, nums.length];
    while (i < len) {
        nums[i] === val ? (nums[i] = nums[--len], nums.pop()) : i++;
    }
    return len;
};

```
Two Sum II - Input array is sorted

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.
Example:

``` bash
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

- 最先想到的是二分
先写遍二分
``` js
find = (arr, target) => {
    let [l, h] = [0, arr.length - 1];
    while (l <= h) {
        const mid = l + h >> 1;
        if (target < arr[mid]) {
            h = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            return mid;
        }
    }
};
```

---

好，一遍ac 二分 log(n)
``` js
find = (arr, target) => {
    let [l, h] = [0, arr.length - 1];
    while (l <= h) {
        const sum = arr[l] + arr[h];
        if (sum < target) {
            l++;
        } else if (sum > target) {
            h--;
        } else {
            return [l++, h++];
        }
    }
}
```

朴素方法 O(n)  map

``` js
find = (arr, target) => {
    const map = {};
    arr.forEach((e, i) => map[e] = i); // {num: idx}
    for (let i = 0; i < arr.length; i++) {
        const r = map[target - arr[i]];
        if (r && r !== i) return [i + 1, r + 1];
    }
};
```
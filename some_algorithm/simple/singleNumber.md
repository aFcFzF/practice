Given a non-empty array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Example 1:
``` bash
Input: [2,2,1]
Output: 1
```

``` bash
Input: [4,1,2,1,2]
Output: 4
```

1. 不让开新空间
2. 最多O(n),不然超时

想到的第一种方法,噗，感觉这是不行的，不仅最差情况下n2，而且不通用。感觉要朴素方法

``` js
find = arr => arr.sort((a, b) => a - b).reduce((a, b, i) => i % 2 ? a - b : a + b, 0);
```

朴素方法
``` js
find = arr => {
    arr.sort((a, b) => a - b);
    for (let i = 1; i < arr.length - 1; i++) {
        if (arr[i - 1] !== arr[i] && arr[i] !== arr[i + 1]) return arr[i];
    }
    return arr[0] === arr[1] ? arr[arr.length - 1] : arr[0];
}
```

有更简单的
``` js
const singleNumber = nums => nums.reduce((prev, cur) => prev ^ cur);
```
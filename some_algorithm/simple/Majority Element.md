Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:
``` bash
Input: [3,2,3]
Output: 3
Example 2:
```

``` bash
Input: [2,2,1,1,1,2,2]
Output: 2
```

- 朴素方法(92ms) ：
``` js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    const map = new Map();
    nums.forEach(e => {
        const v = map.get(e);
        v === undefined ? map.set(e, 1) : map.set(e, v + 1);
    });
    let count = Number.MIN_VALUE, r = null;
    for (const [k, v] of map.entries()) {
        v > count && (count = v, r = k);
    }
    return r;
};
```

- 变通方法(80ms) :
``` js
    const majorityElement = nums => {
        let count = 0, r = 0;
        for (const n of nums) {
            count === 0 && (r = n);
            r === n ? count++ : count--;
        }
        return r;
    }
```

注： 主要是规定众数 大于 n / 2，如果不是这句， [1, 2, 3, 1] 无效。
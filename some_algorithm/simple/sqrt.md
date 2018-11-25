Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

eg1: 

``` css
Input: 4
Output: 2
```

eg2: 
``` css
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

``` js
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    let [l, h] = [0, x];
    while (l <= h) {
        const mid = l + h >> 1;
        const midVal = mid ** 2; 
        if (midVal > x) {
            h = mid - 1;
        }
        else if (midVal < x) {
            l = mid + 1;
        }
        else {
            return mid;
        }
    }
    return h;
};
```

疑问： 难道说。。如果最终没找到该数，那么h是大于该数的最小值。
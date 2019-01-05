Given an array, rotate the array to the right by k steps, where k is non-negative.

``` js
fn = (arr, n) => {
    while (n--) {
        arr.unshift(arr.pop());
    }
    return arr;
};

```
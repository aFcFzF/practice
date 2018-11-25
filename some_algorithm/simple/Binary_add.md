bin add
eg: a = '11' b = '1' // '100'

朴素写法: 

``` js
binAdd = (a, b) => {
    let [lenA, lenB, add, r] = [a.length, b.length, false, ''];
    while (lenA > 0 || lenB > 0) {
        const tmp = +a.charAt(--lenA) + +b.charAt(--lenB) + add;
        if (tmp > 1) {
            r = tmp - 2 + r;
            add = true;
        }
        else {
            r = tmp;
            add = false;
        }
    }
    add && (r = 1 + r);
    return r;
};
```

归写法:

``` js
binAdd = (a, b, add = false) => {
     if (!(a || b)) return add ? '1' : '';
    const [preA, preB, lastA, lastB] = [
		a.slice(0, a.length - 1),
		b.slice(0, b.length -1),
		+a.charAt(a.length - 1),
		+b.charAt(b.length - 1)
	];
    const tmp = lastA + lastB + add;
    const r = tmp > 1 ? tmp - 2 + '' : tmp + '';
    return addBinary(preA, preB, tmp > 1) + r;
};
```


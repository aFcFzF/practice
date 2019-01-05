excel 题就是，然后还原
``` bash
1 -> 'A'
26 -> 'Z'
27 -> 'AA'
```

``` js
    // 数字转字母
    const fn = n => {
        if (typeof n !== 'number') throw new Error('input type must is number');
        let r = '';
        while (n--) {
            const m = n % 26;
            r = String.fromCodePoint(65 + m) + r;
            n = n / 26 | 0;
        }
        return r;
    };
```
// 按权展开
``` js
    // 字母转数字
    const fn = s => {
        if (typeof s !== 'string') throw new Error('input type must is string');
        let r = 0, len = s.length, l = len;
        while (l--) {
            r += (s.codePointAt(l) - 64) * 26 ** (len - 1 - l);
        }
        return r;
    };

```
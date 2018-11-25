求最后一个单词的长度
``` js
LastLen = str => {
    if (!str) return 0;
    let [i, b, e] = [0, 0, 0];
    while (i < str.length) {
        const char = str.charAt(i++);
        const next = str.charAt(i);
        if (next) { // 下个有值
            char === ' ' ? next !== ' ' && (b = i) : e = i; // b = i 因为i是空格后的第一个
        } 
        else {
            if (char === ' ') {
                return i - b;
            }
        }
    }
    return e - b;
};
```
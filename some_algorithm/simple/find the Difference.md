### Find the Difference

Given two strings s and t which consist of only lowercase letters.

String t is generated by random shuffling string s and then add one more letter at a random position.

Find the letter that was added in t.

example:
``` bash
Input:
s = "abcd"
t = "abcde"

Output:
e

Explanation:
'e' is the letter that was added.
```

---

解法1： hash表，由于可能出现重复的，所以就用O(n) 两次的。
``` js
findTheDifference = function(s, t) {
    const map = {};
    let [i, len] = [0, s.length];
    while (i < len) {
		const char = s.charAt(i++);
		map[char] = map[char] === undefined ? 1 : map[char] + 1;
    }
    i = 0;
    while (i <= len) {
        const char = t.charAt(i++);
		map[char] !== undefined && map[char]--;
        if (map[char] < 0 || map[char] === undefined) return char;
    }
};
findTheDifference('abcd', 'abcde')
```

解法2： 排序，然后对比，也许最差是 n2的，取决于排序算法。
``` js
var findTheDifference = function(s, t) {
    s = s.split('').sort((a, b) => a.charCodeAt(0) - b.charCodeAt(0));
    t = t.split('').sort((a, b) => a.charCodeAt(0) - b.charCodeAt(0));
    let [i, len] = [0, s.length];
    while (i < len) {
        if (s[i] !== t[i]) return t[i];
        i++;
    }
    return t[len];
};
```


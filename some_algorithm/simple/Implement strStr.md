Question:
``` css
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

Input: haystack = "hello", needle = "ll"
Output: 2
Example 2:

Input: haystack = "aaaaa", needle = "bba"
Output: -1
Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().
```

Answer:
``` js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if (needle === '') return 0;
    for (let [idx, char] of Object.entries(haystack)) {
        if (char === needle.charAt(0)) {
            let fail = false;
            let i = idx;
            for (let sub of needle) { // 这想法有问题，如果后面
                if (sub !== haystack[i++]) {
                    fail = true;
                    break;
                }
            }
            if (!fail) return +idx;
        }
    }
    return -1;
};
```

`问题： n2 的话，超时。 不对。。这一整段如果有问题，就应该跳过。`

**提示： 双指针**



``` js
strStr = (haystack, needle) {
    let [i, j] = [0, 0];
    while (i < haystack.length) {
        if (!needle.charAt(j)) {
            return i - j;
        }
        if (!(haystack[i++] === needle.charAt(j++))) {
            j = 0;
            idx = 0;
        }
    }
    return j === needle.length ? j : -1;
}
```
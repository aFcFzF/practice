>Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:

Input: ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
Note:

All given inputs are in lowercase letters a-z.


``` js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs, pos = 0) {
    const base = strs.length ? strs[0] : '';
	for (const char of base) {
		let fail = false;
		for (const e of strs) {
			if (char !== e[pos]) {
				fail = true;
				break;
            }
		}
		if (fail) break;
		pos++;
	}
	return base.slice(0, pos);
};
```

递归方法：
``` js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs, idx = 0) {
    if (strs.length < 1) return '';
    let pre = '';
	const same = strs.every(e => {
		!pre && (pre = e[idx]);
		return e[idx] && e[idx] === pre;
	});
	return same ? pre + longestCommonPrefix(strs, ++idx) : '';
};
```
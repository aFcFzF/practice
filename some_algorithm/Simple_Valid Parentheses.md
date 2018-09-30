``` css
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:

Input: "()"
Output: true
Example 2:

Input: "()[]{}"
Output: true
Example 3:

Input: "(]"
Output: false
Example 4:

Input: "([)]"
Output: false
Example 5:

Input: "{[]}"
Output: true
```

思路： 隐约觉得应该找出最内层的。。然后就是，碰到`({}[]{}([{()}])`这种前面的就要马上检测。
ps： 没想出来，看答案看明白了，写两遍。

---

``` js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    const stack = [];
    for (let char of s) {
        if (char === '(' || char === '[' || char === '{') {
            stack.push(char);
        }
        else {
            if (!stack.length) return false;
            let fail = false;
            const oldChar = stack.pop();
            oldChar === '(' && char !== ')' && (fail = true);
            oldChar === '[' && char !== ']' && (fail = true);
            oldChar === '{' && char !== '}' && (fail = true);
            if (fail) return false;
        }
    }
    return !stack.length;
};
```

Question:

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

思路：
``` css
1. {}{}()[] 这种型的一旦遇到到 ( [ { 就开始检测是否右边是 ) } ]
2. {{[](){}}} 这型的遇到嵌套 ( [ { 就一直嵌套（放到栈里），然后遇到右括号，取出来就行
3. ({} 这个也需要判断，因为遍历的时候，要和第一个对比，所以栈最后是放空的
4. ] 只有一个的时候，最后不会放到栈里，检测是否栈为空
```

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

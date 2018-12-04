Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

Note: A leaf is a node with no children.

Example:
Given the below binary tree and sum = 22,

``` bash
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```
path is `5 -> 4 -> 11 -> 2`

---

这题主要是读题： 要求是 `root-to-leaf`,也就是从根到叶子节点。
postscript： 注意判断空root

两种解法： bfs, dfs

1. bfs 循环
``` js
const trave = (root, sum) => {
    if (!root) return false;
    root.sum = root.val;
    const que = [root];
    while (que.length) {
        const item = que.shift();
        if (!item.left && !item.right && item.sum === sum) return true;
        item.left && que.push(Object.assign(item.left, {sum: item.left.val + item.sum}));
        item.right && que.push(Object.assign(item.right, {sum: item.right.val + item.sum}));
    }
    return false;
};
```

2. dfs 循环
``` js
const trave = (root, sum) => {
    if (!root) return false;
    root.sum = root.val;
    const stack = [root];
    while (stack.length) {
        const item = stack.pop();
        if (!item.left && !item.right && item.sum === sum) return true;
        item.right && stack.push(Object.assign(item.right, {sum: item.right.val + item.sum}));
        item.left && stack.push(Object.assign(item.left, {sum: item.left.val + item.sum}));
    }
    return false;
};
```

3. dfs 递归
```js
const trave = (root, sum) => {
    if (!root) return false;
    let b = false;
    const dfs = ([n, s]) => {
        if (b || (s === sum && !n.left && !n.right)) return b = true;
        n.left && dfs([n.left, s + n.left.val]);
        n.right && dfs([n.right, s + n.right.val]);
    };
    dfs([root, root.val]);
    return b;
};
```
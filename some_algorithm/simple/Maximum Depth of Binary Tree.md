给定一个二叉树，找出其最大深度。

给定一个二叉树 [3,9,20,null,null,15,7]
``` bash
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

- 朴素
``` js
var maxDepth = function(root) {
    if (!root) return 0;
    let max = root.depth = 1;
    const stack = [root];
    while (stack.length) {
        const item = stack.pop();
        const depth = item.depth + 1;
        item.right && stack.push(Object.assign(item.right, {depth}));
        item.left && stack.push(Object.assign(item.left, {depth}));

        // 下面两句实现不好
        // item.left && item.left.val > max && (max = item.left.val);
        // item.right && item.right.val > max && (max = item.right.val);

        (item.left || item.right) && depth > max && (max = depth);
    }
    return max;
};
```

- 递归
``` js
maxDepth = root => {
    const dfs = (n, i = 0) => {
        if (!n) return i;
		return Math.max(dfs(n.left, i + 1), dfs(n.right, i + 1));
    };
    return dfs(root);
};

maxDepth(root);
```
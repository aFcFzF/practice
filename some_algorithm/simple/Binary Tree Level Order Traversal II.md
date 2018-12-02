给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
给定二叉树 [3,9,20,null,null,15,7]

``` bash
    3
   / \
  9  20
    /  \
   15   7
```

``` js
[
  [15,7],
  [9,20],
  [3]
]
```

1. 朴素解法
``` js
const trave = root => {
    if (!root) return [];
    const r = [[root.val]];
    const q = [root];
    root.depth = 0;
    while (q.length) {
        const item = q.shift();
        const depth = item.depth + 1;
        (item.left || item.right) && !r[depth] && (r[depth] = []);
        item.left && q.push(Object.assign(item.left, {depth})) && r[depth].push(item.left.val);
        item.right && q.push(Object.assign(item.right, {depth})) && r[depth].push(item.right.val);
    }
    return r.reverse();
};
```

2. 递归方法
- bfs `有问题，想明白再写吧  问题是这个： a = [1, 2, 3, 4, null, null, 5];`
``` js
const trave = root => {
    let r = [];
    const q = [root];
    if (!root) return r;
    const bfs = dpt => {
        const item = q.shift();
        const d = dpt + 1;
        (item.left || item.right) && !r[d] && (r[d] = []);
        item.left && q.push(item.left) && r[d].push(item.left.val);
        item.right && q.push(item.right) && r[d].push(item.right.val);
        q.length && bfs(d);
    };
    bfs(0);
    return r.reverse();
};
```

- dfs
``` js
var levelOrderBottom = function(root) {
    let r = [];
    if (!root) return r;
    const dfs = (n, dpt) => {
		!r[dpt] && (r[dpt] = []);
		r[dpt].push(n.val);
        n.left && dfs(n.left, dpt + 1);
		n.right && dfs(n.right, dpt + 1);
    };
    dfs(root, 0);
    return r.reverse();
};
```
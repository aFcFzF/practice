Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

Example:

Given binary tree [3,9,20,null,null,15,7],

``` bash
    3
   / \
  9  20
    /  \
   15   7
```

return its minimum depth = 2.

- 朴素 bfs
``` js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var minDepth = function(root) {
    if (!root) return 0;
    const stack = [[root, 1]];
    let min = Number.MAX_VALUE;
    while (stack.length) {
        const [item, dpt] = stack.pop();
        !item.left && !item.right && dpt < min && (min = dpt);
        item.right && stack.push([item.right, dpt + 1]);
        item.left && stack.push([item.left, dpt + 1]);
    }
    return min;
};
```

- 递归 dfs
``` js
var minDepth = function(root) {
    if (!root) return 0;
    let min = Number.MAX_VALUE;
    const dfs = (n, dpt = 1) => {
        !n.left && !n.right && dpt < min && (min = dpt);
        n.left && dfs(n.left, dpt + 1);
        n.right && dfs(n.right, dpt + 1);
    };
    dfs(root);
    return min;
};
```
---

- bfs
``` js
var minDepth = function(root) {
    if (!root) return 0;
    const que = [[root, 1]];
    let min = Number.MAX_VALUE;
    while (que.length) {
        const [n, dpt] = que.shift();
        !n.left && !n.right && dpt < min && (min = dpt);
        n.left && que.push([n.left, dpt + 1]);
        n.right && que.push([n.right, dpt + 1]);
    }
    return min;
};
```
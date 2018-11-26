Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

eg1:

``` bash
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```

eg2:
``` bash
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```

eg2:
``` bash
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

``` js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    const [pl, ql] = [[], []];
    const trave = (n, l) => n && l.push(n.val, trave(n.left, l), trave(n.right, l));
    trave(p, pl);
    trave(q, ql);
    if (pl.length !== ql.length) return false;
    let i = 0;
    while (i < pl.length) {
        if (pl[i] !== ql[i++]) {
            return false;
        }
    }

    return true;
};
```

貌似。。开了俩数组。看看其他方法


``` js
isSame = (p, q) => {
    if (!p && !q) return true;
    if ((!p && q) || (p && !q) || (p.val !== q.val)) return false;
    let r = true;
    r = isSame(p.left, q.left);
    r && (r = isSame(p.right, q.right));
    return r;
};
```
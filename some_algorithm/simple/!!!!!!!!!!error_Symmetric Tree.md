给定一个二叉树，检查它是否镜像对称。

例如：
 [1, 2, 2, 3, 4, 4, 3, 5, 6, 7, 8, 8, 7, 6, 5] 是对称的

``` bash
         1
      /     \
    2        2
   / \       /  \
  3    4    4    3
 / \  / \  / \  / \
5  6 7  8  8  7 6  5
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

``` bash
    1
   / \
  2   2
   \   \
   3    3
```

`**bfs先建立该树。**`
``` js
const a = [1, 2, 2, 3, 4, 4, 3];
const build = a => {
	const ListNode = class {
		constructor (val) {
			this.val = val;
			this.left = this.right = null;
		}
	};
	let i = 0;
	const root = new ListNode(a[i]);
	const p = [];

	const proc = n => {
		if (!n) return;
		const [l, r] = [++i, ++i];
		if (a[l]) {
			const node = new ListNode(a[l]);
			p.push(node);
			n.left = node;
		}
		if (a[r]) {
			const node = new ListNode(a[r]);
			p.push(node);
			n.right = node;
		}
		proc(p.shift());
	};
	proc(root);
	return root;
};
build(a);
```

1. 感觉递归不好回溯，要用循环写。
2. 确实没反应过来, 思路是`左子节点的左子节点 === 右子节点的右节点 && 左子节点的右子节点 === 右子节点的左子节点`

`bfs 遍历 - 朴素方法`
原理：
![img](./img/bfs.jpg)
``` js
check = root => {
	if (!root) throw new Error('该树为空');
	const [r, p, l, r] = [[], [root], [], []];
	const proc = () => {
		while (p.length > 0) {
			const item = p.shift();
			r.push(item.val);
			item.left && p.push(item.left); // 错误写法 item.left && proc(item.left) 因为递归就一直递归了。
			item.right && p.push(item.right);
		}
	};
	proc();
    console.log(l, r);
	return r;
};
check(root);
```





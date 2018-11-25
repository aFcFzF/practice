Question:

``` css
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

ps: leetcode 对js的用例。。是链表，最终要输出数组

``` js
// 我就都输入输出数组吧。
const mergeTwoLists = function(l1, l2) {

const ListNode = function(val) {
    this.val = val;
    this.next = null;
};

const build = (arr, node = null) => {
    let tmp = null;
    for (let i of arr) {
        !node ? tmp = node = new ListNode(i)
        : (tmp.next = new ListNode(i), tmp = tmp.next);
    }
    return node;
};

l1 = build(l1);
l2 = build(l2);
const r = new ListNode(0);
let tmp = r;
while (l1 && l2) {
    let val = null;
    l1.val < l2.val ? (val = l1.val, l1 = l1.next) : (val = l2.val, l2 = l2.next);
    tmp.next = new ListNode(val);
    tmp = tmp.next;
}

l1 && (tmp.next = l1);
l2 && (tmp.next = l2);

const result = [];
tmp = r.next
while (tmp) {
    result.push(tmp.val);
    tmp = tmp.next;
}

return result;
};
mergeTwoLists([1, 1, 2, 3], [1, 2, 3, 4]);
```


递归解法

``` js
mergeTwoLists = function(l1, l2) {
	const ListNode = function(v) {
		this.val = v;
		this.next = null;
	};

	const build = arr => {
		let tmp = null;
		const head = tmp = new ListNode(0);
		arr.forEach(e => (tmp.next = new ListNode(e), tmp = tmp.next));
		return head.next;
	};

	[l1, l2] = [build(l1), build(l2)];

	let tmp = null;
	const head = tmp = new ListNode(0);
	const merge = (l1, l2, tmp) => {
		if (!(l1 && l2)) {
			l1 && (tmp.next = l1);
			l2 && (tmp.next = l2);
			return;
		}
		l1.val < l2.val ? (tmp.next = new ListNode(l1.val), merge(l1.next, l2, tmp.next))
		: (tmp.next = new ListNode(l2.val), merge(l1, l2.next, tmp.next));
	};

	merge(l1, l2, tmp);
	tmp = head.next;
	const r = [];
	while(tmp) {
		r.push(tmp.val);
		tmp = tmp.next;
	}
	return r;
};

mergeTwoLists([1, 1, 2, 3], [1, 2, 3, 4])
```

这题还有三四种解法。。后面补上 :)
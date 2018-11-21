加1,输入和输出都是数组，每个数组项只有1位。

eg：  
输入： [1, 2, 3]
输出： [1, 2, 4]

---

输入：　[9, 9, 9]
输出：　[1, 0, 9, 9]


``` js
/**
* 加1 plusOne 输入 [1, 2, 3] 输出 [1, 2, 4]
*
* @param {Array}
* @return {Array}
*/
var plusOne = function(d) {
    let [i, len, full, r] = [0, d.length, 0, []];
	while (len-- > i) {
        const f = full;
		const j = d[len] + (r.length < 1) + f;
		const n =  j > 9 ? (full = 1, 0) : (full = 0, d[len] + (r.length < 1) + f);
		len === 0 && full ? r.unshift(1, 0) : r.unshift(n);
    }
    return r;
};
```
朴素方法很慢啊，同时做题时间太久了。


``` js
/**
 * @param {number[]} d
 * @return {number[]}
 */
var plusOne = function(d) {
    const r = [];
	const proc = (n, carry, init) => {
		if (n === undefined) return carry && (r.unshift(1));
		n + init + carry > 9 ? r.unshift(0) : r.unshift(n + init + carry);
		proc(d.pop(), n + init + carry > 9, 0);
	};
	proc(d.pop(), false, 1);
	return r;
}
```
上面是递归方法
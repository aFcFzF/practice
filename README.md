### 练习..

1. 【2018-08-25】创建两元素A和B, B绕A匀速旋转。

``` js
const CircleAnimate = class {
    constructor() {
        this.init();
    }

    /**
    * init 创建节点
    *
    * @inner
    */
    init() {
        const createEls = (...args) => args.map(e => document.body.appendChild(document.createElement(e)));
        const [p, c] = createEls('div', 'span');
        Object.assign(this, {p, c});
        const sty = {
            position: 'absolute',
            left: '200px',
            top: '200px',
            width: '10px',
            height: '10px',
            borderRadius: '3px'
        };
        Object.assign(p.style, sty, {backgroundColor: '#000'});
        Object.assign(c.style, sty, {backgroundColor: '#00f'});
    }

    /**
        * run 执行动画
        *
        * start {number=} 开始角度 default = 0
        * end {number=} 结束角度 default = 360
        * duration {number=} 默认动画时间  default = 1000ms
        * r {number=} 环绕半径 default 100px
    */
    run(start = 0, end = 360, duration = 1000, r = 100) {
        const [pX, pY] = [this.p.offsetLeft, this.p.offsetTop];
        const ani = deg => {
            const [x, y] = [
                Math.round(Math.sin(2 * Math.PI / -360 * deg) * r + pX),
                Math.round(Math.cos(2 * Math.PI / 360 * deg) * r + pY)
            ];
            Object.assign(this.c.style, {
                left: x + 'px',
                top: y + 'px'
            });
        };
        const frames = duration / 16.7;
        const step = (end - start) / frames;
        const aniList =  Array.from({length: Math.ceil(frames)}, (_, i) => _=> ani(step * ++i + 180 + start));
        const exec = _ => aniList.length > 0 && (aniList.shift()(), 1) && window.requestAnimationFrame(exec);
        exec();
    }
}

a = new CircleAnimate;
a.run();
```

2. 【2018-08-25】平衡二叉树交换子节点

``` js
// 测试用例
const node = {
    value: 1,
    left: {
        value: 2,
        left: {
            value: 4,
            left: {value: 8}
        },
        right: {value: 5}
    },
    right: {
        value: 3,
        left: {
            value: 6
        },
        right: {
            value: 7
        }
    }
};

traverse = n => {
    if (!n) return;
    n.left && n.right && ([n.left, n.right] = [n.right, n.left]);
    traverse(n.left);
    traverse(n.right);
    return n;
};

traverse(node);
```

3. 【2018-08-25】构造函数A继承B

``` js
    const inherit = (A, B) => {
        Reflect.setPrototypeOf(A.prototype, B.prototype);
        Reflect.setPrototypeOf(A, B);
    }

    // 兼容方案
    function inherit (A, B) {
        if (typeof A !== 'function' || typeof B !== 'function') throw new Error('A和B必须是构造函数');
        if (typeof Reflect !== 'undefined') {
            Reflect.setPrototypeOf(A.prototype, B.prototype);
            Reflect.setPrototypeOf(A, B);
            return A;
        }
        if (Object.create) {
            var p = Object.create(B.prototype);
            Object.keys(A.prototype).forEach(function(k) { // 相当于 var p = Object.assign(Object.create(B.prototype), A.prototype);
                p[k] = A.prototype[k];
            });
        }
        else {
            var p = new B; // 参数麽？
            for (var k in A.prototype) {
                A.prototype.hasOwnProperty(k) && (p[k] = A.prototype[k]);
            }
        }
        A.prototype = p;
        A.__proto__ && (A.__proto__ = B);
        return A;
    }
```
4. 【2018-08-25】 模板解析

``` js
    // 测试用例 tpl和 data
    const tpl = `
        <div>{%name%}
            <span>{%desc%}</span>
        </div>`;
    const data = {
        name: 'husky',
        desc: 'fe'
    };

    // 结果
    const parse = (tpl, data) =>
    tpl.trim().replace(/{%([_a-zA-Z]\w+)%}/g, (m, w) => data.hasOwnProperty(w) ? data[w] : m);
```

5. 【2018-09-04】 实现类似JSON.stringify 的功能

ps： new Date().toISOString()...之前不知道 QAQ

``` js
    obj = {
        str: 'baidu ',
        num: 123,
        bool: true,
        arr: [new Number(3), new String('false'), new Boolean(false)],
        obj: { x: [10, undefined, function(){}, Symbol('')] },
        fn: function(){},
        date: new Date(2006, 0, 2, 15, 4, 5)
    };
    // obj.self = obj; 测试环
    stringify = obj => {
        const parent = [];
        const stringifyProc = (obj, r = '') => {
            // 主要是为了得到plainObject和一些特殊类型，例如 Date
            const getType = o => ({}).toString.call(o).match(/\[object (\w+)\]/)[1].toLowerCase();

            if (getType(obj) !== 'object' && getType(obj) !== 'array') {
                if (getType(obj) === 'string') return `"${obj}"`;
                if (getType(obj) === 'date') return `"${obj.toISOString()}"`;
                let r = undefined;
                const tps = ['undefined', 'null', 'function', 'symbol'];
                tps.indexOf(getType(obj)) === -1 && (r = obj.valueOf());
                return r;
            }

            if (parent.indexOf(obj) > -1) throw Error('Converting circular structure to JSON');
            parent.push(obj);
            Array.isArray(obj) ? obj.forEach((item, idx, arr) => {
                const b = idx === 0 ? '[' : '';
                const e = idx >= arr.length -1 ? ']' : '';
                const comma = idx < arr.length -1 ? ',' : '';
                let result = stringifyProc(item);
                result = result === undefined ? null : result;
                r += `${b}${result}${comma}${e}`;
            })
            : Object.entries(obj).forEach(([k, v], idx, arr) => {
                const b = idx === 0 ? '{' : '';
                const e = idx >= arr.length -1 ? "}" : '';
                const comma = idx < arr.length -1 ? ',' : '';
                const result = stringifyProc(v);
                result !== undefined && (r += `${b}"${k}":${result}${e}${comma}`);
            });

            return r;
        }
        return stringifyProc(obj);
    };
    stringify(obj);
```
先检测环？一般环的场景不多罢。。。

``` js
const cycleDetector = o => {
    const p = [];
    const getType = o => ({}).toString.call(o).match(/\[object (\w+)\]/)[1].toLowerCase();
	const chkProc = o => {
        if (getType(o) === 'object' || getType(o) === 'array') { // 不如用typeof + for...in 果然还是语言层面的理解不够
            const iter = Array.isArray(o) ? o : Object.values(o);
            for (let v of iter) {
                if (p.indexOf(v) > 0 || chkProc(v)) return true;
            }
            p.push(o);
        }
        return false;
	}
	return chkProc(o);
};
cycleDetector(obj);
```

6. 实现bind
``` js
/* 测试用例
    func._bind({y: 'foo'})() // undefined "foo"
    func._bind()()  // undefined undefined
    func._bind({y: 'bar'}, 'foo')() // foo bar
*/
const bindPolyfill = _ => {
   if (Reflect.has(Function.prototype, '_bind')) throw Error('_bind property has existed in proto in Function');
   Function.prototype._bind = function (thisObj = {}, ...args1) {
       return (...args2) => {
           thisObj._fn_ = this;
           const r = thisObj._fn_(...args1, ...args2);
           Reflect.deleteProperty(thisObj, '_fn_');
           return r;
        }
   }
};
bindPolyfill();
```

7. nodejs 实现一个简单的静态服务器，要求与说明：
a. 只相应 .js 后缀的资源请求，当本地存在时响应200，不存在响应400，默认utf-8
b. 客户端过期时间设置为1小时
c. 静态资源存在在服务器`/home/admin/htdocs`，url映射规则为 `/foo/bar.js->home/admin/htdocs/foo/bar.js`，即为$(根目录)/${url}的拼接。
d. 实现304状态码响应逻辑
e. 尽可能提高响应性能，以提高服务器吞吐能力
f. 注意安全问题，防止/../../foo.js 这种相对路径访问到其他系统文件
g. 允许使用任意版本的nodejs api
h. 不允许使用任何第三方开源模块

8. 实现一个网页版的聊天室（类似与钉钉群），请列出关键技术方案及要点。需求如下：
高实时性、高性能
你发的每条消息可以看到有多少人已读
当信息含有@某人时，被@的人的界面上会显示“有人@你”的提醒字样
刷新页面或断网状态下，历史记录不会消失。

9. 【2018-09-12】 两数之和 （112ms）
``` js
    const twoSum = (nums, target) => {
        const map = {};
        let r = [];
        nums.some((e, i) => {
            if (map[e] != null) return r = [map[e], i];
            const d = target - e;
            map[d] == null && (map[d] = i);
        });
        return r;
    };
```
10. 【2018-09-13】 反转整数
取值范围 32位有符号

``` js
    // 数学方法
    const reverse = x => {
        let r = 0;
        const [min, max] = [(-2) ** 31 / 10, (2 ** 31 -1) / 10];
        while(x !== 0) {
            const mod = x % 10;
            if (r < min || (r === ~~min && mod < -8)) return 0;
            if (r > max || (r === ~~max && mod > 7)) return 0;
            x = ~~(x / 10);
            r = r * 10 + mod;
        }
        return r;
    }
```
11. 检测回文数
思路：
    1. 转字符串: 开了额外空间
    2. 全转： 如果大于int32，则溢出。（看反转整数）
    3. 转一半，给原数不断除10.如果原数小于当前数，就说明不是了。中位数如果是奇数，那(x || x / 10)
tip： 所有的负数都不是回文数, 还有俩特殊值。 10 和 0
``` js
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if (x < 0 || (x % 10 === 0 && x !==0)) return false;
    let r = 0;
    while(x > r) {
        const mod = x % 10;
        x = ~~(x / 10);
        r = r * 10 + mod;
    }
    return x === r || x === ~~(r / 10);
};
```

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
        if (getType(o) === 'object' || getType(o) === 'array') {
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
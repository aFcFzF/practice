### 练习..

1. 【2018-08-25】创建两元素A和B, B绕A匀速旋转。

``` js
const CircleAnimate = class {
    constructor() {
        this.init()
    }

    /**
    * init 创建节点
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
a.run()
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
    const inherit = (A, B) =>
    Reflect.setPrototypeOf(A.prototype, B.prototype);

    // 兼容方案
    function inherit (A, B) {
        if (typeof A !== 'function' || typeof B !== 'function') throw new Error('A和B必须是构造函数');
        if (typeof Reflect !== 'undefined') {
            Reflect.setPrototypeOf(A.prototype, B.prototype);
            return A;
        }
        if (Object.create) {
            var p = Object.create(B.prototype);
            Object.keys(A.prototype)
            .forEach(function(k) {
                p[k] = A.prototype[k]
            });
        } else {
            var p = new B; // 参数麽？
            for (var k in A.prototype) {
                A.prototype.hasOwnProperty(k) && (p[k] = A.prototype[k]);
            }
        }
        A.prototype = p;
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
    tpl.trim().replace(/{%(\w+)%}/g, (m, w) => data[w]);
```
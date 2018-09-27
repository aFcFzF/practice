手写有点问题？

``` javascript
const getScript = (src, prop, j) => {
        const s = document.createElement('script');
        s.src = src;
        s.onload = _ => Reflect.deleteProperty(window, prop);
        s.onerror = err => j(err);
        document.head.appendChild(s);
    };

    const getData = (url, data) => {
        const name = '_' + (Math.random() * 1e5 >> 1);
        url = !url.includes('?') ? url + '?callback=' + name + data : url + '&callback=' + name + data;
        return new Promise((r, j) => {
            window[name] = data => r(data);
            getScript(url, name, j);
        });
    };

    getData('http://127.0.0.1:8080', '&tag=abc&date=2011')
    .then(data => console.log(data))
    .catch(err => console.error('错误：', err));
    ```

    一、节流，防抖
    ```
    // 防抖：
    debounce = (fn, interval) => {
        let timer = null;
        return _ => {
            clearTimeout(timer);
            timer = setTimeout(fn, interval);
        };
    };
    // eg:
    window.addEventListener('resize', debounce(() => console.log('resize'), 500));

    // 节流:
    throttle = (fn, interval) => {
        let last = 0;
        return (...args) => {
            const curr = +new Date();
            curr - last >= interval && (fn(...args), last = curr);
        }
    };
    // eg:
    t = throttle(console.log, 1000);
    Array.from({length:　100}, e => t(1, 2, 3)); // 1 2 3
    ```

二、 new一个构造函数发生了4步操作：
    1. 创建一个plainObject
    2. 绑定构造函数的this为该对象
    3. 执行构造函数
    4. 返回该对象

三、 链式调用，函数结束后返回this, 具体查看$.fn下的原型方法
四、 document 和 window，一个dom对象，一个顶层对象。包含关系: window.document
五、 合并有序数组: merge = (...args) => args.reduce((a, b) => a.concat(b), []);
六、 短路径bfs， 长路径dfs
七、 性能优化： http2 多路复用，同源减少握手，无最大连接限制。非http2： 精灵图, 模块化, tree-shake, splitChunks, uglify
八、 AMD 思想： 定义packages， 先加载所有factory返回的模块，后缓存。首屏较慢，模块带缓存，非esm动态加载，babel注意编译之后，没有_default.
九、webpack插件使用： plugins: [new plug], 常用的 ExtractTextPlugin, BundleAnalysePlugin, HtmlWebpackPlugin 插件编写。。不知
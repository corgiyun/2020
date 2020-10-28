### 数据类型有哪些，区别是什么
有基本类型和引用类型
基本类型：undefined, null, string, number, boolean
引用类型：object, Array, RegExp, Date, Function, Global, Math
区别：
- 基本类型是按值访问，不可变的；而引用类型的值是可变的
- 基本类型的变量是存放在栈区的；引用类型的值是同时保存在栈内存(引用)和堆内存中的(对象)
- 基本类型将一个变量赋值给另一个变量，复制的是副本，互相不受影响；引用类型赋值其实是对象保存在栈区指针的赋值，两个变量都指向同一个对象，任何操作都会互相影响


### 常用的ES6新特性
箭头函数，解构赋值，let，const，promise，模版字符串，函数参数默认值，class，数组遍历方法


### new 操作符实现原理
```js
// 1. 创建一个新对象
// 2. 设置新对象的构造函数，原型链接到目标对象
// 3. 执行构造函数，将属性和方法添加到新对象上，绑定this
// 4. 如果构造函数执行结果返回的是一个对象，返回这个对象，否则，返回创建的新对象
function _new() {
  let obj = {};
  let [constructor, ...args] = [...arguments];
  obj.__proto__ = constructor.prototype;
  let result = constructor.apply(obj, args);
  if(result && (typeof(result) == 'object' || typeof(result) == 'function')) {
    return result;
  }
  return obj;
}
```


### 防抖和节流
```js
// 防抖：事件被触发n秒后执行，如果在n秒内事件再次触发，则需重新等待n秒，函数才会执行。场景：input 输入联想
function debounce(fn, delay) {
  let timeout = null;
  return function() {
    timeout && clearTimeout(timeout);
    timeout = setTimeout(()=> {
      fn.apply(this, arguments)
    }, delay)
  }
}

// 节流：单位时间内只能触发一次函数，如果在单位时间内触发多次，只有一次生效，稀释函数的执行频率。场景：鼠标滑入，滚动等
function throttle(fn, delay) {
  let canRun = true;
  return function() {
    if(!canRun) return;
    canRun = false;
    setTimeout(()=> {
      fn.apply(this, arguments);
      canRun = true;
    }, delay)
  }
}
```


### Promise
```js
class Promise {
  callbacks = [];
  state = 'pending'; // 状态
  value = null; // 保存结果
  constructor(fn) {
    fn(this._resolve.bind(this))
  }

  then(onFulfilled) {
    if (this.state === 'pending') { // 在 resolve 之前，跟之前逻辑一样，添加到 callbacks 中
      this.callbacks.push(onFulfilled)
    } else { // 在 resolve 之后，直接执行回调没，返回结果了
      onFulfilled(this.value)
    }
    return this;
  }
  _resolve(value) {
    this.state = 'fulfilled'; // 改变状态
    this.value = value; // 保存结果
    this.callbacks.forEach(fn=> fn(value))
  } 
}

// Promise 应用
let p = new Promise(resolve=> {
  // setTimeout(()=> {
    console.log('done');
    resolve('2秒')
  // }, 2000)
}).then(tip=> {
  console.log('then1',tip);
}).then(tip=> {
  console.log('then2,', tip);
})
```


### 闭包
函数在词法作用域之外调用，仍能访问到内部的变量，会形成闭包

### Event Loop 事件循环
JavaScript 只有单线程，同步事件放入栈中依次处理，异步事件放入队列中，当主线程闲置时，会去查找事件队列是否有任务，如果有，会取出排在第一位的事件，并把这个事件对应的回调放入执行栈中，如此反复循环，直到事件处理完毕。

### 协程和线程


### 深拷贝实现原理


### 原型和继承


### call、apply 和 bind 的区别


### 路由的两种方式 history 和 hash 的区别


### 箭头函数和普通函数的区别
- 箭头函数代码简写清晰，优雅
- 箭头函数没有自己的this，它会捕获在定义时外层的this并继承，所以在定义时this已经被确定了不会再改变
- 箭头函数没有arguments
- 箭头函数没有原型


### 弱引用 WeakMap, WeakSet, 和普通 object 的区别


### 事件捕获和冒泡
事件捕获：从 document 到 target
事件冒泡：从 target 到 document


### 判断基本数据类型和引用类型的方法
判断基本数据类型，可以用 typeof 方法
判断引用类型用 instance


### 柯里化函数


### 强缓存和协商缓存
作用：减少资源加载和请求，从而提升网页的加载和渲染速度
浏览器缓存机制分两种：强缓存和协商缓存
基本原理：
- 浏览器在加载资源时，根据请求头的expires 和 cache-control 判断是否命中缓存，是则直接从缓存中读取资源，不会发请求到服务器
- 如果没有命中强缓存，则发送一个请求到服务器，通过 last-modified 和 etag 验证资源是否命中协商缓存，如果命中，服务器会将这个请求返回，但是不会返回这个资源的数据，依然是从缓存中读取资源
- 如果前面两者都没命中，直接从服务器加载资源

相同点：如果命中，都是从客户端缓存中加载资源，不会从服务器中加载资源
不同点：强缓存不发请求到服务器，协商缓存会发送请求到服务器

使用：
- 强缓存通过 expires 和 Cache-control 实现， expires是一个绝对时间，受限于本地时间；cache-control是想对时间，一般设置 max-age=xxx;(no-store不会缓存数据到本地),cache-control 优先级高于 expires
- 协商缓存命中会返回304，Last-Modified 表示本地文件最后修改日期，浏览器会在请求头加上 If-Modified-Since(上次返回的 Last-Modified 的值)，询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来，但是如果在本地打开了缓存文件，会造成 Last-Modified 被修改，所以 HTTP/1.1出现了 ETag
- 资源变化会导致ETag变化，保证每一个资源都是唯一的。 If-Modified-Match 的 header 会将上次返回的 ETag 发送给服务器，询问该资源的 ETag 是否有更新，有变动就会发送新的资源回来。ETag 优先级高于 Last-Modified

from memory cache 是页面刷新时从内存中取
form disk cache 页面tab关闭后从磁盘中取
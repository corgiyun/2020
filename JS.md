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
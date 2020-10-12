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


### Promise 原理

### v-model 的实现原理，和 .sync 修饰符的区别
原理：v-model 是语法糖，是v-bind和@change事件的结合，默认使用名为value的prop，以及名为 input 的事件。最后都是执行 addEventListener。
区别：


### 路由守卫有哪些
1、全局守卫
- router.beforeEach 全局前置守卫，进入路由之前调用
- router.beforeResolve 全局解析守卫，在beforeRouteEnter调用之后调用
- router.afterEach 全局后置钩子 进入路由之后
```js
// main.js 入口文件

import routre from './router' // 引入路由
router.beforeEach((to, from, next)=> {
  next();
})
router.beforeResolve((to, from, next)=> {
  next();
})
router.afterEach((to, from)=>{
  console.log('afterEach 全局后置钩子')
})

```

2、路由独享守卫
```js
const router = new VueRouter({
 routes: [
    {
      path: '/foo', 
      component:Foo,
      beforeEnter: (to, from, next)=> {
        // 参数用法什么的都一样，调用顺序在全局前置守卫后面，所以不会被全局守卫覆盖  
      }
    }
  ]
})
```
3、路由组件内部守卫
-  beforeRouteEnter 进入路由前, 在路由独享守卫后调用，不能获取组件实例 this，组件实例还没被创建
-  beforeRouteUpdate (2.2) 路由复用同一个组件时, 在当前路由改变，但是该组件被复用时调用 可以访问组件实例 this
-  beforeRouteLeave 离开当前路由时, 导航离开该组件的对应路由时调用，可以访问组件实例 this


### 通信方式有哪些
- props & emit
- v-modal & .sync
- $parent & $children
- eventBus
- provide & inject
- VueX

### 2.x 和 3.0是如何实现双向绑定
2.x使用 Object.defineProperty 来给实例data的属性添加setter/getter，并通过发布订阅模式来实现响应
由于defineProperty的限制，只能绑定对象的某个属性，vue需要递归遍历对象的所有属性进行绑定，功能上不是很完美
监听数组开销过大，只监听了数组初始化的长度，索引发现变化或索引被删除时，需要手动observe，使用$set去修改数组或者监听对象新属性
- Observer 用来监听拦截data的属性为监察者
- Dep用来添加订阅者，为订阅器
- Watcher 就是订阅者
监察者通过Dep向Watcher发布更新消息

3.0使用proxy进行双向绑定，proxy提供了可以定义对象基本操作的自定义行为的功能（如属性查找、赋值、枚举、函数调用），可以直接拦截整个对象，不再需要递归


### keep-alive 使用场景
主要用于保留组件状态或避免重新渲染
不会在函数式组件中正常工作，因为他们没有缓存实例


### VueX 是什么，解决了什么问题
是什么：状态管理工具，核心是store，响应式状态存储；核心概念 state, getters, mutations, actions, modules
解决了什么问题： 
- 管理多个组件共享状态
- 状态变更追踪
- 形成规范，方便管理维护


### mixin的使用场景和弊端
使用：可以全局混入或者单个组件混入，注入公共事件，方便统一使用和修改
弊端：命名冲突，滥用造成事件换乱，不易追溯问题的根源


### Vue 和 React 怎么选
- 个人开发习惯，模版写法还是 all in js
- 开发成本，团队成员技术栈情况，上手成本
- 项目体积量小选vue，上手快简单高效，react成本过大等


### $set是如何实现的
看文档

### 生命周期 & 做了什么事情
- beforeCreate: vue 实例的挂载元素 $el 和数据对象 data 都是undefined, 还未初始化
- created: 数据对象 data 初始化完成，$el 还未初始化，但是可以做一些副作用操作，发起请求不宜过多，避免造成页面空白时间过长
- beforeMount: data 和 $el 都初始化完成，模版编译阶段，把 data里的数据和模版编译成 HTML，此时还未挂载
- mounted: 实例已经挂载上去，模版中的 HTML 渲染到 HTML 页面中
- beforeUpdate: 虚拟DOM更新渲染前，可以更改状态，不会造成重新渲染
- updated: 由于数据修改导致虚拟DOM重新渲染，组件DOM已经更新，可以执行依赖 DOM的操作，避免在此钩子操作，易造成无限循环
- beforeDestroy: 实例销毁前，实例仍然可用
- destroyed: 实例销毁之后调用，所有的监听事件移除，子组件实例也会销毁


### Vue的路由实现：hash 模式和 history 模式

- hash模式：
在浏览器中符号 “#”，#以及#后面的字符称之为hash，用window.location.hash读取；
特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。
hash模式下，仅hash符号之前的内容会被包含在请求中，如 http://www.xiaogangzai.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误

- history模式：
history采用HTML5的新特性；且提供了两个新方法：pushState()，replaceState()可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。
history模式下，前端的URL必须和实际向后端发起请求的URL一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。
Vue-Router官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

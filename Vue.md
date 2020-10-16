### v-model 的实现原理，和 .sync 修饰符的区别
原理：v-model 是语法糖，是v-bind和@change事件的结合，默认使用名为value的prop，以及名为 input 的事件。最后都是执行 addEventListener。
区别：


### 路由守卫有哪些
- 全局守卫
- 路由独享守卫
- 路由组件内部守卫

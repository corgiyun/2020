### 生命周期
- componentWillMount() // 组件即将被渲染到页面之前触发，可以进行服务器请求等操作
- render() // 组件渲染阶段
- componentDidMount() // 组件已经被渲染到页面中后触发，此时页面有了真正的DOM元素，可以进行DOM相关的操作
- componentWillReceiveProps() // 组件接收到属性时触发
- shouldComponentUpdate() // 当组件接收到新属性，或者组件的状态发生改变时触发，组件首次渲染时并不会触发
- componentWillUpdate() // 组件即将更新
- componentDidUpdate() // 组件更新完成，页面中产生了新的DOM元素，可以进行DOM操作
- componentWillUnmount() // 组件被销毁时触发，清理定时器，取消Redux的订阅事件等

<!-- 16.3更新 -->
<!-- 为了更好的支持异步渲染，解决生命周期滥用可能导致的问题 -->
- componentWillMount() => UNSAFE_componentWillMount()
- componentWillReceiveProps() => UNSAFE_componentReceiveProps()
- componentWillUpdate() => UNSAFE_componentsWillUpdate()
<!-- 新的生命周期 -->
- static getDerivedStateFromProps(nextProps, prevState){}
- getSnapShotBeforeUpdate()

<!-- 16.8 -->
挂载阶段：
- constructor(props) // 实例化
- static getDerivedStateFromProps() // 从props中取state
- render // 渲染
- componentDidMount // 完成挂载

更新阶段：
- static getDerivedStateFromProps // 从 props 中获取 state
- shouldComponentUpdate // 判断是否需要重绘
- render // 渲染
- getSnapshotBeforeUpdate // 获取快照
- componentDidUpdate // 渲染完成后回调

卸载阶段：
- componentsWillUnmount

错误处理
- static getDerivedStateFromError // 从错误中获取state
- componentDidCatch // 捕获错误并进行处理


### 类组件和函数式组件的区别
- 函数式组件代码精简，纯函数，无副作用；类组件重复代码过多
- 函数式组件用钩子管理state, 使用生命周期
- 函数式组件没有this；打包压缩方便，class组件不好压缩；
- 函数式组件逻辑块更清晰，代码可读性好
- 命令式和声明式，减少api，减少出错率，写法优雅，生态更加蓬勃，心智模式更加‘函数式’



### React 工作原理
页面拆分成一个个组件，每个组件是一个状态机，组件内部通过state来维护组件状态的变化，当组件状态发生变化时，React 通过diff算法，虚拟DOM技术更新真实DOM，提升渲染效率。


### key 的作用
- 生成唯一标识，每个key对应一个组件，不会重复渲染；
- key相同，若组件属性有变化，则只更新组件对应的属性，没有变化就不更新。key不相同，则销毁该组件重新创建
总结：使diff算法更高效，提升渲染效率


### Redux工作流程
view dispatch 拦截 action, 然后执行对应 reducer 并更新到 store 中，最终 view 会根据 store 数据的改变执行界面的刷新渲染操作
- action 就是 view 发出的通知，表示 state 要发生变化了
- store 接收到 action 后，必须给出一个新的 state, 这样 view 才能发生变化，这种 state 的计算过程就叫做 reducer
- reducer 指定了应用状态的变化如何响应 action 并发送到 store 的，描述如何改变数据



### hooks API
- useState
- useEffect
- useMemo
- useCallback
- useContext
- useRef
- useReducer


### setState 中的回调函数作用是什么
state更新完毕, 执行操作


### diff 算法
- 是一种通过同层的树节点进行比较的高效算法，避免了对树进行逐层搜索遍历，时间复杂度只有O(n)
- 将DOM抽象为虚拟DOM，通过新旧虚拟DOM对比差异（diff） 最终只把变化的部分重新渲染，提高渲染效率


### props 和 state 的区别
- props 是传递给组件的，state 是在组件内部定义管理的
- props 只读不可修改，state 可以修改


### 高阶组件


### useEffect 第二个参数的影响
- 不传入第二个参数，组件每次render都会触发执行
- 第二个参数为空数组，只在组件挂载的时候执行一次
- 有第二个参数时，参数发生变化，就会触发执行


### React 和 Vue 的异同点

#### 相同点
- 都是采用 Virtual DOM 来减少性能消耗，提高渲染效率
- 推崇组件化开发，结构清晰易于复用
- 两个框架都专注于 UI层，路由管理、状态管理都由周边生态去处理
#### 不同点
- 监听数据变化的实现原理不同，Vue 使用 getter/setter 及其他函数劫持，React 默认通过比较引用（diff）的方式，如果不做优化会导致大量不必要的虚拟DOM的重新渲染
- 状态管理不同，Vue 使用data 管理状态，可以直接修改，React 使用 state管理，不可直接修改，需使用 setState
- 数据流不同，Vue 是双向数据流，React 是单向数据流
- 组合功能方式不同，Vue 中组合不同的功能用mixin，React使用 HOC
- 通信方式不同，Vue 中子组件给父组件通信倾向于使用事件方式，React使用回调函数
- 模版渲染方式不同，Vue 使用拓展的 HTML语法进行渲染，React是使用 JSX 渲染模版


### React 性能优化

#### shouldComponentUpdate()
判断该组件是否需要更新
#### PureComponent
把继承类从 Components 换成 PureComponent 即可，减少不必要的render。当组件更新时，如果组件的 props 和 state 都没发生改变，render方法就不会触发，省去了 Virtual DOM的生成和对比过程，从而达到提升性能。比较 Object.keys(state | props)的长度是否一致，做了浅比较。
- 易变数据不能使用一个引用
- 不变数据引用同一个引用
#### 复杂状态和简单状态不要共用一个组件

### 生命周期
- componentWillMount()
- componentDidMount()
- componentWillReceivedProps()
- shouldComponentUpdate()
- componentWillUpdate()
- componentDidUpdate()
- componentWillUnmount()

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
- constructor(props) 实例化
- static getDerivedStateFromProps() 从props中取state
- render 渲染
- componentDidMount 完成挂载

更新阶段：
- static getDerivedStateFromProps 从 props 中获取 state
- shouldComponentUpdate 判断是否需要重绘
- render 渲染
- getSnapshotBeforeUpdate 获取快照
- componentDidUpdate 渲染完成后回调

卸载阶段：
- componentsWillUnmount

错误处理
- static getDerivedStateFromError 从错误中获取state
- componentDidCatch 捕获错误并进行处理


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
dispatch 触发 action，effects 定义更新方法，触发 reducer return 一个新的 state, 触发渲染更新

### hooks API
- useState
- useEffect
- useMemo
- useCallback
- useContext
- useRef
- useReducer


### setState 中的回调函数作用是什么


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

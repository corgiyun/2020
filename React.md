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



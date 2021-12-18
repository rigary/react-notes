## Tip:

* useState 创建的 state 类似 class 组件的 this.setState，但是它不会把新的 state 和旧的 state 进行合并。
* useEffect 给函数组件增加了操作副作用的能力。它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API。
* 当你调用 useEffect 时，就是在告诉 React 在完成对 DOM 的更改后运行你的“副作用”函数。
* 默认情况下，React 会在每次渲染后调用副作用函数 —— 包括第一次渲染的时候。

## Hook 使用规则

* 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。
* 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中）

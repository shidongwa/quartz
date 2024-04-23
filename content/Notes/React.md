---
parent: "[[Fleeting MOC]]"
tags:
  - 🪴weedy
  - dailyNotes
date: 
draft: false
---
---

UI=f(state)     
UI 是视图，state 是应用状态，f 则是渲染过程。

----

# JSX

JSX 是 React.createElement的语法糖，是 React render()方法的返回值，不包括 React 组件生命周期管理，事件处理和状态管理。

# React 的渲染机制

React 组件的渲染机制，讲到虚拟 DOM 是真实 DOM 的抽象，React 开发者通过 JSX 等 API 声明组件树，React 内部则会生成对应的虚拟 DOM；组件 props、state、context 有变化时会触发协调过程，通过 Diffing 算法比对新旧虚拟 DOM，最终只针对差异部分调用 DOM API 改变页面。

# CSS-In-JS

CSS 组件化技术

* 例子：emotion。功能包括嵌套选择器、伪类选择器、样式中使用 props 数据
* LESS、SCSS、SASS是 CSS 预处理器

>[!info]
> CSS中子选择器和后代选择器
> `div > ul` 是子选择器；`div ul`是后代选择器。选择范围不一样

# React类组件生命周期
![[Pasted image 20240309065929.png]]

**挂载阶段**

1. 构造函数：设置 state 初始值，绑定 this 到类实例
2. getDerivedStateFromProps：设置 state
3. render：返回 JSX，组件的元素树，并最终渲染成 DOM 树
4. componentDidMount：DOM 树已创建，可以访问 DOM 元素。

**更新阶段**

1.  外部更新 props，内部set state或者 forceUpdate 触发更新
2. getDerivedStateFromProps
3. shouldComponentUpdate, 返回 false 时则不继续下一步的 render
4. render
5. getSnapshotBeforeUpdate
6. componentDidUpdate：操作 DOM，处理网络请求

**卸载阶段**

1. componentWillUnmount：清理定时器，取消事件订阅


# 用 hooks 定义函数组件的生命周期
![[Pasted image 20240309070408.png]]

**挂载阶段**

执行组件函数，把 useState，useMemo 等 hooks 挂载到FiberNode。组件函数的返回值为 JSX，渲染阶段会创建 FiberNode 树。提交阶段会依次执行 Effect。

**更新阶段**

调用 useState 或者 useReducer，组件进入更新状态。渲染阶段更新 hooks；提交阶段更新  真实DOM，清理和执行 Effect。

**卸载阶段**

清理 Effect

# useEffect 执行副作用，提交阶段执行

```
useEffect(() => {/* 省略 */; return () => {/* 省略 */};}, [status]);
//        ------------------------------------------     -------
//                       ^         -----------------        ^
//                       |                 ^                |
//                  副作用回调函数         清除函数         依赖值数组
```

useMemo / useCallback 用在渲染阶段，只要用于性能优化


# Redux
Redux 是 React应用状态管理框架，单向数据量的抽象。Redux全局只有一个 store，比较适合管理全局状态。

- 全局状态倾向于放到 Redux store 里；
- 局部状态倾向于放到 React state 里；
- 业务状态倾向于放到 Redux store 里；
- 交互状态倾向于放到 React state 里；
- 必要时，可以把外部状态同步到 Redux store 里。
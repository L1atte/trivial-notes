# Timeline of a React Component With Hooks

## introduction

了解函数式组件`hook`的执行顺序对正确地编写 React 十分有效，接下来我们将会了解到详细的 React hook 的执行顺序

## TL, DR

以下是简扼版的 React Component 时间线

- Mount
  - evaluate local variables （计算局部变量）
  - initialize hooks（初始化 hooks）
  - return React elements（`React.createElement()` 返回元素）
  - insert DOM nodes（插入 DOM 节点）
  - set DOM refs
  - `useLayoutEffect` setups（执行 `useLayoutEffect` 钩子）
  - DOM paint
  - `useEffect` setups（执行 `useEffect` 钩子）
- Update
  - evaluate local variable
  - run necessary hooks
  - return React elements（`React.createElement()` 返回元素）
  - update DOM nodes（更新 DOM 节点）
  - unset DOM refs
  - `useLayoutEffect` clean up（清除上一次 `useLayoutEffect` 副作用）
  - set DOM refs
  - `useLayoutEffect` setup（执行 `useLayoutEffect` 钩子）
  - DOM paint
  - `useEffect` clean up（清除上一次 `useEffect` 副作用）
  - `useEffect` setup（执行 `useEffect` 钩子）
- Unmount
  - `useLayoutEffect` clean up（清除 `useLayoutEffect` 副作用）
  - unset DOM refs
  - remove DOM nodes
  - `useEffect` clean up（清除 `useEffect` 副作用）
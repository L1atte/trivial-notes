# useEffect 的心智模型

## 每一次渲染都有它自己的 props 和 state

我们的组件第一次渲染的时候，从`useState()`拿到`count`的初始值`0`。当我们调用`setCount(1)`，React会再次渲染组件，这一次`count`是`1`。如此等等：

```js
// During first render
function Counter() {
  const count = 0; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}

// After a click, our function is called again
function Counter() {
  const count = 1; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}

// After another click, our function is called again
function Counter() {
  const count = 2; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}
```

## 每次渲染都有它自己的Effects

## 同步， 而非生命周期

## 每一次渲染都有它自己的…所有

## 那Effect中的清理又是怎样的呢？

## 同步， 而非生命周期

## 告诉React去比对你的Effects

## 关于依赖项不要对React撒谎

## 如果设置了错误的依赖会怎么样呢？

## 两种诚实告知依赖的方法

## 让Effects自给自足

## 解耦来自Actions的更新

## 为什么useReducer是Hooks的作弊模式

## 把函数移到Effects里

## 但我不能把这个函数放到Effect里
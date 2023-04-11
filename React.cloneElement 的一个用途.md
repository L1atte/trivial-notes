# React.cloneElement 的一个用途

## 问题描述：

```jsx
const a = <div>1</div>
// 现在我们只能操作 a 变量，如何通过 a 添加 onClick 方法
```

可以通过使用 React 的 `cloneElement` 函数来克隆 `a` 并添加 `onClick` 方法。`cloneElement` 函数会克隆并返回一个新的 React 元素，同时可以传递新的 props 给克隆的元素。

以下是如何使用 `cloneElement` 函数添加 `onClick` 方法：

```jsx
const handleClick = () => {
  console.log("clicked");
};

const aWithOnClick = React.cloneElement(a, { onClick: handleClick });
```


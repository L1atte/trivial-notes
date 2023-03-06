```react
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

当我们更新状态（state）的时候，React会重新渲染组件。每一次渲染都能拿到独立的 count 状态，这个状态值是函数中的一个常量

```react
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



在 `UseEffect` 中，如果遇到无限请求的问题，或者 socket 被频繁创建的问题。**解决方法不是移除依赖项**



由于浏览器无法判断是否在 js 代码中操作 dom，如果在操作 DOM 的同时 UI 线程也在进行渲染的话，就会发生不可预期的展示结果，因此 JS 线程与 UI 渲染线程是互斥的。

但是，可以通过人工干预的方法，将 js 代码放到 paint 之后执行。从而为 js 长任务“减重”，减轻渲染压力


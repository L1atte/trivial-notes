# React Hooks

## React 的心智负担源于：当前依赖的引用类型

## useState

```js
const [state, setState] = useState(initialState)
```

### setState 是稳定的

setState 函数是稳定的，它不会在组件重新渲染时发生变化。所以不需要在`useEffect`或`useCallback`等Hook的依赖列表中添加setState

### lazy state initialization

当我们传递给`useState`的 `initialState`是一个函数的时候，那么 React 只会在初始化时掉用它，并在下一次渲染时忽略它

```js
function Greeting({initialName = ''}) {
  function getName() {
    console.log('getName')
    return 'hello world'
  }
  
  // 如果传递的是函数，只会在初始化的时候调用一次 —— 只会在初始化的时候打印 getName
  const [name, setName] = React.useState(getName)
  // 如果传递的是普通值，每次渲染都会调用 —— 每次渲染都会打印 getName
  const [name, setName] = React.useState(getName())
  
  return (
    <div>{name}</div>
  )
}
```

## useReducer

## useMemo

## useCallback
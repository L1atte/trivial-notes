# Best Practices for React with TypeScript

## Function Components

我们可以像普通函数一样，接受一个 `props` 参数并返回一个 JSX 元素

```typescript
// Declaring type of props - see "Typing Component Props" for more examples
type AppProps = {
	message: string;
} // 如果需要 export 的话，使用 interface 以便继承(extend)

// Easiest way to declare a Function Component; return type is inferred.
const App = ({ message }: AppProps) => <div>{message}</div>;

// you can choose annotate the return type so an error is raised if you accidentally return some other type
const App = ({ message }: AppProps): JSX.Element => <div>{message}</div>;

// you can also inline the type declaration; eliminates naming the prop types, but looks repetitive
const App = ({ message }: { message: string }) => <div>{message}</div>;
```



# Hooks

### useState

可以使用泛型为 `useState` 提供类型

```typescript
const [user, setUser] = useState<User | null>(null);

// later...
setUser(newUser);
```


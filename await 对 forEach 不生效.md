# await 对 forEach 不生效

## example：

```js
async function fn() {
	await [1, 2, 3].forEach(async i => {
		await new Promise(r => {
			setTimeout(() => {
				console.log("setTimeout");
				r();
			});
		});
	});
}

console.log("start");
await fn();
console.log("end");

```

```js
Log:
start
end
setTimeout
setTimeout
setTimeout
```

## 原因分析：

`Array.prototype.forEach` 返回 `undefined`

因此 `await [1, 2, 3]...` 并不会生效 （实际上等于 `await undefined`）

## 建议方法

采用 `map` + `Promise.all`

```js
async function fn() {
	await Promise.all(
		[1, 2, 3].map(async i => {
			await new Promise(r => {
				setTimeout(() => {
					console.log("setTimeout");
					r();
				}, 1000);
			});
		}),
	);
}

console.log("start");
await fn();
console.log("end");

```


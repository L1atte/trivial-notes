# Retry `async` and `await` expression

Here are some example to explain the behavior about `async` and `await` expression

```js
/**
 * explain the 'await' expression only call the promise and don't call the 'then' function
 * because the log is that ------------- 'start' -> 'promise' -> 'end' -> 'then'
 */
async function fn() {
	await new Promise(r => {
		console.log("promise");
		r();
	}).then(() => console.log("then"));
}

console.log("start");
fn();
console.log("end");
```


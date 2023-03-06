

- 可迭代对象（Iterable object）必须实现 `Symbol.iterator` 方法
- `Symbol.iterator`必须返回一个符合 **迭代器协议** 的对象
- 迭代器协议要求对象必须有`next()`方法，它返回一个 `{done: Boolean, value: any}` 对象，这里 `done:true` 表明迭代结束，否则 `value` 就是下一个值

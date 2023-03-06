# jsbridge 设计

H5 调用 Native

```typescript
JSBridge.putData('userInfo', {
	username: 'name',
	age: 20
})
```

H5 从 Native 获取数据

```
JSBridge.getData('userInfo', function(data) {
	console.log(data)
})
```

当前项目

H5 调用 Native

```
browser2Client('methodName', params)
```

H5 获取数据

```
useRegisterBrowserMethod('method', (data) => {
	doSth(data)
})
```



```javascript
const browser2Client = (methodName, params) => {
	const formatted = JSON.stringify(params)
  
  const result = window[methodName].(formatted)
  return JSON.parse(result)
}

const registerHandler = (methodName, callback) => {
  window[methodName] = function(param) {
    return callback(param)
  }
}
```



### Register handler
# Vite static asset 踩坑问题

## 图片资源没有被正常的打包进 output 文件夹

待写

### 相关 issue：

- https://github.com/vitejs/vite/issues/10863
- https://github.com/vitejs/vite/pull/7837

## `windicss` 的 config 中 url 引入的 background image 没有被正常打包进 output 文件夹

### 问题日志：

```shell
./src/assets/image/line_bg.webp referenced in /@windicss/windi.css didn't resolve at build time, it will remain unchanged to be resolved at runtime
```

### `windi.config.ts`

```js
backgroundImage: {
  "line-bg": "url('./src/assets/image/line_bg.webp')",
},
```

### 解决方法：

`windi.config.ts`

```js
import { resolve } from "path";
backgroundImage: {
  "line-bg": resolve("./src/assets/image/line_bg.webp"),
},
```

### 相关 issue：

- https://stackoverflow.com/questions/75601350/tailwind-and-vite-warnings-didnt-resolve-at-build-time-it-will-remain-unchan
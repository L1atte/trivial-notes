# bug

```javascript
changes: {
  data, // 存放插入的数据
}
// 目前问题出在 changes.slice(start, end) 的 start 上
// 错因：changesWithOldAndNewData.length = 1，slice(1, 1)
// new 的时候b

// 删除是正常的
// 删除两个的时候，chganges.length = 2
// 上一个改动数组长度是1，slice(1, 2)。是没问题的

// 删除的时候
// changesWithOldAndNewData.length = 0，是正确的

// 添加的时候
// changesWithOldAndNewData.length = 0，是正确的
// changes_news.length = 1
// [{
// data: {}
// insertBeforeKey: 100
// key: "996e1dfa-d793-decb-80e4-cf8c3429bb2d"
// type: "insert"
// [[Prototype]]: Object
// }]

// 因此，changesWithOldAndNewData = changes_news
// changesWithOldAndNewData.length = 1
```

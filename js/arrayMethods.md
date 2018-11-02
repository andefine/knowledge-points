# 数组常用原生方法

## `Array.prototype.splice`[mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

```javascript
arr.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

## `Array.prototype.sort`[mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

```javascript
arr.sort([compareFunction])
```

- 没有参数

把所有元素转成字符串，按照`Unicode`编码顺序排。**注意：数字`80`和`9`比较的话，`80`会排到`9`前面，因为会转成字符串进行比较**

- `compareFunction`作为参数

  + 返回值小于`0`，`a`的下标小于`b`，即`a`排到`b`前面
  + 返回值等于`0`，`a`和`b`下标不变
  + 返回值大于`0`，`a`的下标大于`b`，即`a`排到`b`后面

```javascript
arr.sort((a, b) => {
  if (a < b) {
    return -1
  }
  if (a > b) {
    return 1
  }
  return 0
})
```

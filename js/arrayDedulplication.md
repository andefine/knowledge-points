### 1. 暴力解决，for套for，比较后面是否会出现重复值，出现直接删除，这个会改变原数组
```javascript
function (arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) {
        arr.splice(j, 1)
        j-- // 在删除后面的重复元素时，下标需要向前移一位
      }
    }
  }
}
```

### 2. 创建一个新数组，遍历原数组，使用indexOf判断是否存在于新数组中，不存在则push到新数组中
```javascript
function (arr) {
  let res = []

  for (let i = 0; i < arr.length; i++) {
    if (res.indexOf(arr[i]) === -1) {
      res.push(arr[i])
    }
  }

  return res
}
```

### 3. 遍历数组，同样使用indexOf，针对原数组比较下标，如果indexOf返回的下标和当前下标相等，则将该值push到新数组
```javascript
function (arr) {
  let res = []

  arr.forEach((cur, index, arr) => {

    // Array.prototype.indexOf将返回第一个找到元素的下标，那么判断条件中两者相等的话，说明当前值是第一次出现在数组中，push到新数组
    if (arr.indexOf(cur) === index) {
      res.push(cur)
    }

  })

  return res
}
```

### 4. 遍历数组，将数组中的值保存在对象的属性中，值都设为1，表示已经有该值。若该属性的值为 1，表示新数组中已经存在该值，则不用push到新数组中
```javascript
function (arr) {
  let obj = {}
  let res = []

  for (let i = 1; i < arr.length; i++) {
    if (!obj[arr[i]]) {
      obj[arr[i]] = 1
      res.push(arr[i])
    }
  }
  
  return res
}
```
### 5. 先排序，判断当前值和上一个值是否相等，不相等添加到新数组中
```javascript
function (arr) {
  let res = []
  let pre

  arr.sort()
  pre = arr[0]
  res.push(arr[0])

  // for循环从 1 开始
  for (let i = 1; i < arr.length; i++) {
    // 如果当前值和上一个值不相等，说明该该值第一次出现，push
    if (arr[i] !== pre) {
      arr.push(arr[i])
      pre = arr[i]
    }
  }

  return res
}
```

### 6. 使用ES6 Set
```javascript
[...new Set(arr)]
```
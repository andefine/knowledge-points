### 正则实现trim
```
function customTrim(str) {
  let reg = /^\s+|\s+$/g
  return str.replace(reg, '')
}
```
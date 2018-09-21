### 正则实现trim
```javascript
function customTrim(str) {
  let reg = /^\s+|\s+$/g
  return str.replace(reg, '')
}
```
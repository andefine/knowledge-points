# 各种需求

## 一个字符串，每隔几个字符插入一个字符

```javascript
const str = 'fhsdoifds'
const str2 = str.replace(/(.{2})/g, '$1:')
const str3 = str2.slice(0, str2.length - 1)
```

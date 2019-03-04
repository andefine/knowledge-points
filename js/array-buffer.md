# `ArrayBuffer`基本使用

```javascript
// 新建一个ArrayBuffer
const buf = new ArrayBuffer(4) // 参数为

// 获取视图，通过视频对ArrayBuffer进行操作(设值，取值)
const dv = new DataView(buf)

dv.setUint8(0, 0x01)
dv.setUint8(1, 0x02)
dv.setUint8(2, 0x03)
dv.setUint8(3, 0x04)

const b0 = dv.getUint8(0)
const b1 = dv.getUint8(1)
const b2 = dv.getUint8(2)
const b3 = dv.getUint8(3)

console.log(b0)
console.log(b1)
console.log(b2)
console.log(b3)
```
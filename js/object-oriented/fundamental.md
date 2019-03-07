# 面向对象基本用法
```javascript
class Person {
  constructor (name, age) {
    this.name = name
    this.age = age
  }

  sayName () {
    console.log(this.name)
  }
}

```
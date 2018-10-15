# 一些乱七八糟奇奇怪怪的需求

## 路由跳转之后无需禁止返回
- 在`router-view`上面添加`replace`属性
```html
<router-view replace/>
```
- 使用`router.replace`代替`router.push`
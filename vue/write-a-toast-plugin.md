# 写一个`vue`的`toast`插件

使用过`element-ui`的朋友应该都用过这个：
```javascript
this.$message('This is a message.')
```
很方便，一行`js`语句，就能在页面弹出一条提示信息。这种交互效果在平常的开发中是经常用到的。最近在写个小项目练习`vue`，同样有这样的需求，但是不想直接拿现成的来用，所以决定动动小手写一个╮(￣▽￣)╭。

## 先来一个`toast`组件
```html
<template>
  <div class="toast">{{message}}</div>
</template>

<script>
export default {
  props: {
    message: String
  }
}
</script>

<style>
.toast {
  position: fixed;
  left: 50%;
  top: 50%;
  border-radius: 20px;
  padding: 20px 50px;
  max-width: 80%;
  line-height: 1.4;
  color: #fff;
  background: rgba(0, 0, 0, .7);
  transform: translate(-50%, -50%);
}
</style>

```

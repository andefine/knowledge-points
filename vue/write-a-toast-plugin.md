# 写一个`vue`的`toast`插件

使用过`element-ui`的朋友应该都用过这个：
```javascript
this.$message('This is a message.')
```
很方便，一行`js`语句，就能在页面弹出一条提示信息。这种交互效果在平常的开发中是经常用到的。最近在写个小项目练习`vue`，同样有这样的需求，但是不想直接拿现成的来用，所以决定动动小手写一个╮(￣▽￣)╭。

## 先来一个`toast`组件

```html
// src/plugin/toast/toast.vue

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
*该项目是移动端项目，使用了vw进行适配，设计稿的宽度定为了1080px，所以你如果直接拷贝下面css中代码的话会可能会表现的很难看(这不重要)*

然后我们只需要在需要使用到的页面组件中引入使用就行了，这里不再赘述。但是这样的方式对于这种使用率极高并且使用场景大多在js交互中就显得非常麻烦，我们的目的是像文章开头中那样使用:
```javascript
this.$toast('I am a Toast')
```

## 组件如何变身为插件

### 回忆杀
当初我们学习`vue`阅读文档时是多么痛苦，理所当然的肯定见过下面这几行代码：
```html
<div id="app">
  {{ message }}
</div>
```
```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
依照文档的说明，这是在创建一个`Vue`实例，每个`Vue`程序都是这样开始的。

我们先来看一下`Vue`中插件的定义，文档: [Plugins](https://vuejs.org/v2/guide/plugins.html)。其中第四条就是我们接下来所使用的插件类型，在`Vue.prototype`上添加方法，这样我们就能向这样`this.$toast('...')`使用啦

在相同位置新建`index.js`
```javascript
import Vue from 'vue'

// 引入toast组件，作为 component options (组件选项)
import toastOptions from './toast.vue'

// 文档是这样说的：
// 使用基础 Vue 构造器，创建一个“子类”。参数是一个包含 组件选项 的对象。
// Vue.extend 文档位置 https://vuejs.org/v2/api/#Vue-extend
// 在这里 Vue.extend 会返回一个实例构造器，
// 这样我们才能在后面使用 new Toast() 去生成一个 toast 实例，
// 并将它插入到页面结构中。
// 这里可能需要面向对象的知识会比较好理解很多(其实我对面向对象的理解也是一团浆糊￣ω￣=)
const Toast = Vue.extend(toastOptions)

function showToast (options = {}) {
  const message = typeof options === 'string' ? options : options.message
  const duration = options.duration || 2000

  const toast = new Toast({
    el: document.createElement('div')
  })

  toast.message = message
  toast.show = true

  document.body.appendChild(toast.$el)

  setTimeout(() => {
    toast.show = false
  }, duration)
}

function registryToast () {
  Vue.prototype.$toast = showToast
}

export default registryToast
```

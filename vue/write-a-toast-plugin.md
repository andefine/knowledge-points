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
this.$toast('I am a toast')
```

## 组件如何变身为插件

我们先来看一下`Vue`中插件的定义，文档: [Plugins](https://vuejs.org/v2/guide/plugins.html)。其中第四条就是我们接下来所使用的插件类型，在`Vue.prototype`上添加方法，这样我们就能像这样`this.$toast('...')`使用啦

在相同位置新建`index.js`
```javascript
// src/plugin/toast/index.js

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
  // 下面这行的写法可以让我们在使用 `this.$toast()` 的时候，
  // 可以直接传入一个字符串，也可以传入一个配置对象
  const message = typeof options === 'string' ? options : options.message
  const duration = options.duration || 3000

  const instance = new Toast({
    el: document.createElement('div')
  })

  instance.message = message
  instance.show = true

  document.body.appendChild(instance.$el)

  setTimeout(() => {
    instance.show = false
  }, duration)
}

export default function () {
  Vue.prototype.$toast = showToast
}
```
`toast.vue`也作了一点修改，添加了过渡。没接触过的朋友可以去看看文档哦 [过渡和动画](https://vuejs.org/v2/guide/transitions.html)
```html
<template>
  <transition name="fade">
    <div class="toast" v-show="show">{{message}}</div>
  </transition>
</template>

<script>
export default {
  props: {
    message: String
  },
  data () {
    return {
      show: false
    }
  }
}
</script>

<style>
.toast {
  /* ...和上面一样 不写了 */
}
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-leave-to {
  opacity: 0;
}
</style>

```
整个过程就是新建一个`Toast`实例，将实例添加到`body`中，然后设置定时器，一段时间后隐藏`toast`。

接着在`main.js`中引入，在`new Vue()`之前`Vue.use()`
```javascript
// src/main.js
import toastPlugin from './plugin/toast'
Vue.use(toastPlugin)
```
*注意`Vue.use`是可以传入一个对象或一个函数的，可能我们大多看到的是传入类似`{install: function...}`这样的方式，当然也是可以直接传入函数的，所以我们在`src/plugin/toast/index.js`这里是直接导出一个函数的*

这个时候我们就可以直接在页面组件中直接使用`this.$toast()`啦：

![](https://github.com/andefine/knowledge-points/blob/master/vue/images/toast_01.gif?raw=true)

到这里这个`toast`插件已经是可以正常使用了，但是也存在着一些问题

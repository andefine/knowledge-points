# 自己写一个`vue`的`toast`插件

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

然后我们只需要在需要使用到的页面组件中引入使用就行了，再做的各位肯定都会，这里不再赘述。但是这样的方式对于这种使用率极高并且使用场景大多在js交互中就显得非常麻烦，我们的目的是像文章开头中那样使用:
```javascript
this.$toast('I am a toast')
```

## 组件变身插件

我们先来看一下`Vue`中插件的定义，文档:<a href="https://vuejs.org/v2/guide/plugins.html" target="_blank">Plugins</a>。其中第四条就是我们接下来所使用的插件类型，在`Vue.prototype`上添加方法，这样我们就能像这样`this.$toast('...')`使用啦

在相同位置新建`index.js`
```javascript
// src/plugin/toast/index.js

import Vue from 'vue'

// 引入toast组件，作为 component options (组件选项)
import toastComponent from './toast.vue'

// 文档是这样说的：
// 使用基础 Vue 构造器，创建一个“子类”。参数是一个包含 组件选项 的对象。
// Vue.extend 文档位置 https://vuejs.org/v2/api/#Vue-extend
// 在这里 Vue.extend 会返回一个实例构造器，
// 这样我们才能在后面使用 new Toast() 去生成一个 toast 实例，
// 并将它插入到页面结构中。
// 这里可能需要面向对象的知识会比较好理解很多(其实我对面向对象的理解也是一团浆糊￣ω￣=)
const Toast = Vue.extend(toastComponent)

// 这个就是我们要添加到 Vue.prototype 上的方法
function showToast (options = {}) {
  // 新建 toast 实例
  const instance = new Toast({
    el: document.createElement('div')
  })

  // 下面这行的写法可以让我们在使用 `this.$toast()` 的时候，
  // 可以直接传入一个字符串，也可以传入一个配置对象
  instance.message = typeof options === 'string' ? options : options.message
  const duration = options.duration || 3000

  instance.show = true

  // 将实例添加到 body 中
  document.body.appendChild(instance.$el)

  // 在显示的时长之后，隐藏 toast
  setTimeout(() => {
    instance.show = false
  }, duration)
}

export default function () {
  Vue.prototype.$toast = showToast
}
```
`toast.vue`也作了一点修改，添加了过渡。没接触过的朋友可以去看看文档哦 <a href="https://vuejs.org/v2/guide/transitions.html" target="_blank">过渡和动画</a>
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
*注意`Vue.use`是可以传入一个对象或一个函数的，可能我们大多看到的是传入类似`{ install: function () {} }`这样的方式，当然也是可以直接传入函数的，所以我们在`src/plugin/toast/index.js`这里是直接导出一个函数的*

这个时候我们就可以直接在页面组件中直接使用`this.$toast()`啦：

![](https://github.com/andefine/knowledge-points/blob/master/vue/images/toast_01.gif?raw=true)


## 进一步升级
到这里这个`toast`插件已经是可以正常使用了，而且可以**同时显示出多个`toast`**(从代码中也可以看出来，每次调用都会**新建一个实例，不过这样有问题**)。同时我们可能还有其他需求，就此我们从以下几点试着改进改进：

- 添加`toast`在页面中显示的位置，不用具体到像素级别，可以做到`top`、`middle`、`bottom`就行
- 显示`toast`的时候可能还需要有一个`icon`，来向用户表示不同信息
- 以上的代码中，每次调用`this.$toast`都会新建一个新的实例。平常的业务场景中，同时显示多个`toast`的需求应该不多，但是在使用过程中肯定是会在多个地方需要使用到的，那么使用多少次，我们岂不是就新建了这么多实例，该打该打。那么能不能二次使用呢

直接上改进过的代码：
```html
<template>
  <transition name="fade">
    <div :class="`toast--${position}`" v-show="show">
      <i class="toast__icon" :class="iconClass" v-if="iconClass"></i>
      <span class="toast__text">{{message}}</span>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    message: String,
    position: {
      type: String,
      default: 'middle'
    },
    iconClass: String
  },
  data () {
    return {
      show: false
    }
  }
}
</script>

<style lang="scss">
.toast {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: fixed;
  left: 50%;
  border-radius: 20px;
  padding: 20px 50px;
  max-width: 80%;
  line-height: 1.4;
  color: #fff;
  background: rgba(0, 0, 0, .7);
  transform: translate(-50%, -50%);
  &--top {
    @extend .toast;
    top: 25%;
  }
  &--middle {
    @extend .toast;
    top: 50%;
  }
  &--bottom {
    @extend .toast;
    top: 75%;
  }
  &__icon {
    font-size: 150px;
    color: #fff;
  }
}
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-leave-to {
  opacity: 0;
}
</style>
```
```javascript
import Vue from 'vue'
import toastComponent from './toast.vue'

const Toast = Vue.extend(toastComponent)

// 这个数组用来存放 Toast 实例。
// 但是注意，并不是所有已经新建的实例，
// 而是已经新建的并且已经隐藏起来的实例。
// 来看一下整体流程，大概就明白为什么这样做了。
// 首先是没有实例，当我们第一次调用 this.$toast 时，
// 就新建一个实例，当这个 toast 隐藏之后，
// 将这个实例 push 到这个数组中。
// 那么在我们下一次调用 this.$toast 时，如果这个数组为空，
// 说明之前的实例还没有隐藏，那我们只能重新新建一个实例；
// 另一种情况，只要数组中有一个值，说明已经有之前新建的实例，
// 而且已经隐藏，那我们便可以拿它重新使用，并不需要新建实例。
// 这样便达到可以同时显示多个 toast 而且不会新建多余实例
const instances = []

/**
 * 这个函数主要是获取一个可用实例
 *
 * @returns
 */
function getAnAvailibleInstance () {
  if (instances.length > 0) {
    const instance = instances[0]
    instances.shift()
    return instance
    // 其实上面三行可以写成：
    // return instances.shift()
    // 然这般甚骚
  }
  return new Toast({
    el: document.createElement('div')
  })
}

function removeDom (e) {
  if (e.target.parentNode) {
    event.target.parentNode.removeChild(event.target)
  }
}

function showToast (options = {}) {
  // console.log(instances)
  const instance = getAnAvailibleInstance()
  clearTimeout(instance.timer)

  instance.message = typeof options === 'string' ? options : options.message
  const duration = options.duration || 3000
  // 添加 toast 在页面中显示的位置和图标class 选项，
  // 当然页面结构和css都要作相应修改。
  // 我这里使用的阿里的iconfont，项目中需要导入相关文件。
  instance.position = options.position || 'middle'
  instance.iconClass = options.iconClass || ''

  document.body.appendChild(instance.$el)

  // 这里使用使用了 Vue.nextTick 全局api。
  // 因为我们改变数据，然后 Vue 更新 DOM 是异步的，
  // 使用 Vue.nextTick 的作用就是保证 DOM 更新
  // 完毕之后再执行 .then 中的回调(Vue.nextTick 支持 promise 风格哟)
  // 我试过不使用 Vue.nextTick 也行，但这样更严谨一点嘛
  // (其实是看 `mint-ui` 源码的)[狗头]
  Vue.nextTick()
    .then(() => {
      instance.show = true

      // 在显示时间结束之后，移除节点。
      // 并且将该实例重新 push 到 instances 数组中，
      // 这样 instances 数组中保存的就是已经隐藏的实例。
      // (其实这里我觉得也是有点问题的，是不是应该在
      // 这个 transition 结束之后再 push 呢？？？
      // 可是好像并没有什么好的办法做到这样，mint-ui 中也是这样做的。
      // 另外 mint-ui 在显示之后先是移除了 transitionend 监听器，
      // 不是很明白为什么要先来移除这一步。希望有大佬解惑一哈)
      instance.timer = setTimeout(() => {
        // 添加监听器，在 transition 结束之后移除节点
        instance.$el.addEventListener('transitionend', removeDom)
        instance.show = false
        instances.push(instance)
      }, duration)
    })
}

export default function () {
  Vue.prototype.$toast = showToast
}
```
很多解释都写在注释中了，这里就不啰嗦了。看效果

![](https://github.com/andefine/knowledge-points/blob/master/vue/images/toast_02.gif?raw=true)

**************
整个插件基本上算是完成了，但是可能仍然存在问题。写这个插件的过程中借鉴(*没错，其实是抄了一大堆\[手动狗头\]，程序员的事怎么叫抄袭呢，叫CV。o(\*////▽////\*)q*)了`noahlam`大佬的文章和`mint-ui`的源码，所以很多地方是一模一样的(不要喷我哟(￣▽￣)／)，很多注释或者解释其实都是对于`mint-ui`源码的解读。琢磨的挺多，于是发现了不少，写篇文章记录一下，希望能给正在读此篇文章的读者有些帮助，当然可能仍然存在问题，解释也可能有误，我也还有很多未解的疑惑。文章中如果有错误的地方，或者代码仍有优化的空间，或者解释有误，请大佬务必指出，不能给读者造成错误认知。在此感谢大佬的文章和`mint-ui`库

> noahlam: <a href="https://juejin.im/post/5af55f906fb9a07aae153c1c" target="_blank">从零开始徒手撸一个vue的toast弹窗组件</a>

> mint-ui 中的 <a href="https://github.com/ElemeFE/mint-ui/tree/master/packages/toast/src" target="_blank">toast</a>

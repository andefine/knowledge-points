# `scss`中使用`BEM`

## 灵活的`&`父选择器标识符

```scss
.header {
  height: 100px;
  &__search {
    height: 20px;
  }
}
```
编译结果：
```css
.header {
  height: 100px;
}
.header__search {
  height: 20px;
}
```

**在`scss`中使用了我们熟悉的嵌套结构，好处就是清晰，便于后期维护。再看编译结果，嵌套结构并没有生成后代选择器，所以便不会产生权重差异带来的问题。**

## 强大的`@extend`指令
```html
<div class="nav">
  <div class="nav__item"></div>
  <div class="nav__item"></div>
  <div class="nav__item nav__item--active"></div>
</div>
```
假设页面结构如上，被选中的`item`不仅有和其他`item`的相同，还有另外的样式，我们在元素上就需要添加这两个类名，这就真的又丑又长了。如果只想在元素上写一个类名呢：
```scss
.nav {
  height: 100px;
  &__item {
    height: 20px;
    width: 20px;
    background: #aaa;
    &--active {
      @extend .nav__item;
      background: #fff;
    }
  }
}
```
编译结果：
```css
.nav {
  height: 100px;
}
.nav__item, .nav__item--active {
  height: 20px;
  width: 20px;
  background: #aaa;
}
.nav__item--active {
  background: #fff;
}
```
**这样，`nav__item--active`也有了`nav__item`的所有样式，元素上只写上`nav__item--active`了**

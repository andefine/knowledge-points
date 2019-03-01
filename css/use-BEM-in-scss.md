# `scss`中使用`BEM`

```scss
.header {
  height: 100px;
  &__search {
    height: 20px;
    &--active {
      color: #ccc;
    }
  }
  &__nav {
    height: 40px;
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
.header__search--active {
  color: #ccc;
}
.header__nav {
  height: 40px;
}
```

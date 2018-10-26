# :active伪类在苹果的手机端不起作用
[:active pseudo-class doesn't work in mobile safari](https://stackoverflow.com/questions/3885018/active-pseudo-class-doesnt-work-in-mobile-safaris)

## 问题
给a标签添加`:active`样式，在苹果手机上点击时没有效果

## 解决方法

- 在`body`元素上添加一个空的`ontouchstart`事件
```html
<body ontouchstart>
<body/>
```

- 上面的方法可能不奏效，那就在需要使用`:active`的元素上添加
```html
<a ontouchstart>
<a/>
```

- 使用`:hover`替代`:active`
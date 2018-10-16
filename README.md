## js

### 知识点
- [undefined和null](./js/undefined-null.md)
- require和import

### 常见方法
- [数组去重](./js/arrayDedulplication.md)

### ES6
- [解构赋值]()

### 其他
- 代码风格
  + [JavaScript Standard Style](https://standardjs.com/)
  + [Airbnb JavaScript Style](http://airbnb.io/javascript/)
- 模板引擎
  * [art-template](http://aui.github.io/art-template/)
- 书籍
  + 《编写可维护的JavaScript》



## 正则(regular expressions)
- [正则实现trim](./regExp/regExp.md)

## vue
- [vue-cli 3.0基本使用方式](./vue/vue-cli-3.0-usage.md)
- [各种各样的需求篇](./vue/demands.md)

## npm (node package manager)
### 常用命令
  - `npm init`
    + `npm init -y` 可以跳过向导，快速生成
  - `npm install`
    + 一次性把 `dependencies` 选项中的依赖项全部安装  
    + 简写 `npm i`
  - `npm install 包名`
    + 只下载
    + `npm i 包名`
  - `npm install --save 包名`
    + 下载并且保存到依赖项( `package.json` 文件中的 `dependencies` 选项)。`--save` 放到`包名`前后都可以
    + `npm i -S 包名`
  - `npm unstall 包名`
    + 只删除，如果有依赖项会依然保存
    + `npm un 包名`
  - `npm unstall --save 包名`
    + 删除的同时也会把依赖信息也去除
    + `npm un -S 包名`


## Node.js
- [Node一些资源](./node/resource.md)

## [glob匹配](https://github.com/andefine/knowledge-points/issues/1)


## 小程序
- [小程序中使用sass](./minApp/useSass.md)


## 工具

### sourceTree
- 安装免登录(随便搜一篇文章)
- 进入sourceTree之后一定要设置一下，“工具”->“选项”->“一般”->“SSH客户端”，改成 OpenSSH
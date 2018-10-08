# vue-cli 3.0基本使用方式

## 全局安装@vue/cli
vue-cli是之前的包名，当你使用`npm i -g vue-cli`安装的包版本号依然还是`2.9.6`。 当前它的名字已经改成了`@vue/cli`，所以安装要换名字：
```shell
npm i -g @vue/cli
```
官方提示，如果存在旧版本，请将旧版本`vue-cli`从全局卸载，可能是命令有冲突啥的，反正卸载更安全：`npm un -g vue-cli`
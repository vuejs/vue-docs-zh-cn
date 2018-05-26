# @vue/cli-plugin-babel

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-babel/README.md)

> vue-cli 的 babel 插件

## 配置

默认使用 Babel 7 + `babel-loader` + [@vue/babel-preset-app](../vue-babel-preset-app/README.md)，但是可以通过 `.babelrc` 配置使用任何其它 Babel 预设选项或插件。

默认情况下，`babel-loader` 会排除 `node_modules` 依赖内部的文件。如果希望显性编译一个依赖的模块，你需要将其添加入 `vue.config.js` 中的 `transpileDependencies` 选项：

``` js
module.exports = {
  transpileDependencies: [
    // 可以是字符串或正则表达式
    'my-dep',
    /other-dep/
  ]
}
```

## 缓存

[cache-loader](https://github.com/webpack-contrib/cache-loader) 默认开启，被缓存的东西存储在 `<projectRoot>/node_modules/.cache/babel-loader`。

## 并行执行

[thread-loader](https://github.com/webpack-contrib/thread-loader) 会在多核机器上默认开启。你可以在 `vue.config.js` 中设置 `parallel: false` 将其关闭。

## 在已创建的项目中安装

``` sh
vue add @vue/babel
```

## 注入的 webpack-chain 规则

- `config.rule('js')`
- `config.rule('js').use('babel-loader')`
- `config.rule('js').use('cache-loader')`

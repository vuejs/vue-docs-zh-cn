# @vue/cli-plugin-typescript

> vue-cli 的 typescript 插件

使用 TypeScript + `ts-loader` + [fork-ts-checker-webpack-plugin](https://github.com/Realytics/fork-ts-checker-webpack-plugin) 实现线程外的快速类型检查。

## 配置

TypeScript 可以通过 `tsconfig.json` 进行配置。

这个插件可以配合 `@vue/cli-plugin-babel` 使用。当使用 Babel 时，该插件将会输出 ES2015 并将其它基于浏览器目标的自动的 polyfill 委托给 Babel。

默认情况下，`ts-loader` 只应用在 `src` 和 `test` 目录下的文件。如果希望显性编译一个依赖的模块，你需要在 `vue.config.js` 中配置 webpack：

``` js
module.exports = {
  chainWebpack: config => {
    config
      .rule('ts')
        .include
          .add(/module-to-transpile/)
  }
}
```

## 注入的命令

如果在项目创建的时候选择了 [TSLint](https://palantir.github.io/tslint/)，则会注入 `vue-cli-service lint`。

## 缓存

[cache-loader](https://github.com/webpack-contrib/cache-loader) 默认开启，被缓存的东西存储在 `<projectRoot>/node_modules/.cache/cache-loader`。

## 并行执行

[thread-loader](https://github.com/webpack-contrib/thread-loader) 会在多核机器上默认开启。你可以在 `vue.config.js` 中设置 `parallel: false` 将其关闭。

## 在已创建的项目中安装

``` sh
npm install -D @vue/cli-plugin-typescript
vue invoke typescript
```

## 注入的 webpack-chain 规则

- `config.rule('ts')`
- `config.rule('ts').use('ts-loader')`
- `config.rule('ts').use('babel-loader')` (当配合 `@vue/cli-plugin-babel` 使用时)
- `config.rule('ts').use('cache-loader')`
- `config.rule('ts').use('thread-loader')`
- `config.plugin('fork-ts-checker')`

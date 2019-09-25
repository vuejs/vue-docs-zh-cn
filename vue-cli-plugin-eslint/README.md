# @vue/cli-plugin-eslint

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-eslint/README.md)

> vue-cli 的 eslint 插件

## 注入的命令

- **`vue-cli-service lint`**

  ```
  使用：vue-cli-service lint [options] [...files]

  选项：

    --format [formatter] 指定格式器 (默认值：codeframe)
    --no-fix             不修复错误
    --max-errors         指定使构建失败的错误数量 (默认值：0)
    --max-warnings       指定使构建失败的警告数量 (默认值：Infinity)
  ```

  校验并修复文件中的错误。如果没有指定文件，则会校验 `src` 和 `test` 中的所有文件。

  也支持其它 [ESLint CLI 的选项](https://eslint.org/docs/user-guide/command-line-interface#options)。

## 配置

ESLint 可以通过 `.eslintrc` 或 `package.json` 中的 `eslintConfig` 字段进行配置。

通过 `eslint-loader` 在每次保存时执行校验的选项是默认开启的，你也可以通过 `vue.config.js` 中的 `lintOnSave` 选项将其关闭。

``` js
module.exports = {
  lintOnSave: false
}
```

当设置为 `true` 时，`eslint-loader` 将会抛出校验错误作为警告。默认情况下警告只会记录在终端，并不会导致编译失败。

你可以使用 `lineOnSave: 'error'` 将校验错误显示在浏览器里的浮层中。这会强制 `eslint-loader` 总是抛出错误。这时也意味着校验错误会导致编译失败。

你也可以配置浮层同时展示警告和错误：

``` js
// vue.config.js
module.exports = {
  devServer: {
    overlay: {
      warnings: true,
      errors: true
    }
  }
}
```

当 `lintOnSave` 是 truthy 值时，`eslint-loader` 会被同时运用在开发环境和生产环境中。如果你在生产环境构建时禁用 `eslint-loader`，可以进行如下配置：

``` js
// vue.config.js
module.exports = {
  lintOnSave: process.env.NODE_ENV !== 'production'
}
```

## 在已创建的项目中安装

``` sh
vue add @vue/eslint
```

## 注入的 webpack-chain 规则

- `config.module.rule('eslint')`
- `config.module.rule('eslint').use('eslint-loader')`

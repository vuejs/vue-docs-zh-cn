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

## 在已创建的项目中安装

``` sh
vue add @vue/eslint
```

## 注入的 webpack-chain 规则

- `config.rule('eslint')`
- `config.rule('eslint').use('eslint-loader')`
- `config.module.rule('eslint')`
- `config.module.rule('eslint').use('eslint-loader')`

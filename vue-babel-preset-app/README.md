# @vue/babel-preset-app

这是所有 Vue CLI 项目默认的 Babel 预设选项。

## 包含的内容

- [babel-preset-env](https://github.com/babel/babel/tree/master/packages/babel-preset-env)
  - `modules: false`
    - 在 Jest 测试中会自动设置为 `'commonjs'`
  - [`useBuiltIns: 'usage'`](https://github.com/babel/babel/tree/master/packages/babel-preset-env#usebuiltins-usage)
    - 确保 polyfill 会按需导入
  - `targets` 的判断规则：
    - 为浏览器构建时使用 `package.json` 中的 `browserslist` 字段
    - 在 Node.js 中运行测试时设置为 `{ node: 'current' }`
- [@babel/plugin-transform-runtime](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-runtime)
  - 只为 polyfill 被 `babel-preset-env` 处理时开启供帮助用
- [动态导入语法](https://github.com/tc39/proposal-dynamic-import)
- [Object rest spread](https://github.com/tc39/proposal-object-rest-spread)
- [babel-preset-stage-2](https://github.com/babel/babel/tree/master/packages/babel-preset-stage-2)
- Vue JSX 语法支持
  - [@babel/plugin-syntax-jsx](https://github.com/babel/babel/tree/master/packages/babel-plugin-syntax-jsx)
  - [babel-plugin-transform-vue-jsx](https://github.com/vuejs/babel-plugin-transform-vue-jsx)
  - [babel-plugin-jsx-event-modifiers](https://github.com/nickmessing/babel-plugin-jsx-event-modifiers)
  - [babel-plugin-jsx-v-model](https://github.com/nickmessing/babel-plugin-jsx-v-model)

## 选项

- **modules**

  默认值：
  - `false` 当用 webpack 构建时
  - `'commonjs'` 当在 Jest 中运行测试时

  为 `babel-preset-env` 显性设置 `modules` 选项。请查阅 [babel-preset-env 文档](https://github.com/babel/babel/tree/master/packages/babel-preset-env#modules) 了解更多详情。

- **targets**

  默认值：
  - 当为浏览器构建时，由 `package.json` 中的 `browserslist` 字段决定
  - 在 Node.js 中运行测试时设置为 `{ node: 'current' }`

  为 `babel-preset-env` 显性设置 `targets` 选项。请查阅 [babel-preset-env 文档](https://github.com/babel/babel/tree/master/packages/babel-preset-env#targets) 了解更多详情。

- **useBuiltIns**

  默认值：`'usage'`

  为 `babel-preset-env` 显性设置 `useBuiltIns` 选项。请查阅 [babel-preset-env 文档](https://github.com/babel/babel/tree/master/packages/babel-preset-env#usebuiltins) 了解更多详情。

- **jsx**

  默认值：`true`。设为 `false` 可以禁用 JSX 支持。

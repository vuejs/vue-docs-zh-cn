# @vue/babel-preset-app

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/babel-preset-app/README.md)

这是所有 Vue CLI 项目默认的 Babel 预设选项。

## 包含的内容

- [babel-preset-env](https://github.com/babel/babel/tree/master/packages/babel-preset-env)
  - `modules: false`
    - 在 Jest 测试中会自动设置为 `'commonjs'`
  - [`useBuiltIns: 'usage'`](#usebuiltins)
    - 确保 polyfill 会按需导入
  - `targets` 的判断规则：
    - 为浏览器构建时使用 `package.json` 中的 `browserslist` 字段
    - 在 Node.js 中运行测试时设置为 `{ node: 'current' }`
- 默认包含 `Promsie` polyfill，这样它们也可以用于非转译依赖 (只用于必要的环境)
- [@babel/plugin-transform-runtime](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-runtime)
  - 只为 polyfill 被 `babel-preset-env` 处理时开启供帮助用
- [动态导入语法](https://github.com/tc39/proposal-dynamic-import)
- [Object rest spread](https://github.com/tc39/proposal-object-rest-spread)
- [babel-preset-stage-2](https://github.com/babel/babel/tree/master/packages/babel-preset-stage-2)
- Vue JSX 语法支持
  - [@babel/plugin-syntax-jsx](https://github.com/babel/babel/tree/master/packages/babel-plugin-syntax-jsx)
  - [babel-plugin-transform-vue-jsx](https://github.com/vuejs/babel-plugin-transform-vue-jsx)
  - ~~[babel-plugin-jsx-event-modifiers](https://github.com/nickmessing/babel-plugin-jsx-event-modifiers)~~ (在 Babel 7 修复之前暂时不可用)
  - ~~[babel-plugin-jsx-v-model](https://github.com/nickmessing/babel-plugin-jsx-v-model)~~ (在 Babel 7 修复之前暂时不可用)

## 选项

### modules

- 默认值：
  - `false` 当用 webpack 构建时
  - `'commonjs'` 当在 Jest 中运行测试时

为 `babel-preset-env` 显性设置 `modules` 选项。请查阅 [babel-preset-env 文档](https://github.com/babel/babel/tree/master/packages/babel-preset-env#modules) 了解更多详情。

### targets

- 默认值：
  - 当为浏览器构建时，由 `package.json` 中的 `browserslist` 字段决定
  - 在 Node.js 中运行测试时设置为 `{ node: 'current' }`

为 `babel-preset-env` 显性设置 `targets` 选项。请查阅 [babel-preset-env 文档](https://github.com/babel/babel/tree/master/packages/babel-preset-env#targets) 了解更多详情。

### useBuiltIns

- 默认值：`'usage'`

为 `babel-preset-env` 显性设置 `useBuiltIns` 选项。

The default value is `'usage'`, which adds imports to polyfills based on the usage in transpiled code. For example, if you use `Object.assign` in your code, the corresponding polyfill will be auto-imported if your target environment does not supports it.

Note that the usage detection does not apply to your dependencies (which are excluded by `cli-plugin-babel` by default). If one of your dependencies need polyfills, you have a few options:

1. **If the dependency is written in an ES version that your target environments do not support:** Add that dependency to the `transpileDependencies` option in `vue.config.js`. This would enable both syntax transforms and usage-based polyfill detection for that dependency.

2. **If the dependency ships ES5 code and explicitly lists the polyfills needed:** you can pre-include the needed polyfills using the [polyfills](#polyfills) option for this preset.

3. **If the dependency ships ES5 code, but uses ES6+ features without explicitly listing polyfill requirements (e.g. Vuetify):** Use `useBuiltIns: 'entry'` and then add `import '@babel/polyfill'` to your entry file. This will import **ALL** polyfills based on your `browserslist` targets so that you don't need to worry about dependency polyfills anymore, but will likely increase your final bundle size with some unused polyfills.

See [babel-preset-env docs](https://github.com/babel/babel/tree/master/packages/babel-preset-env#usebuiltins) for more details.

### polyfills

- Default: `['es6.promise']`

A list of [core-js](https://github.com/zloirock/core-js) polyfills to pre-include when using `useBuiltIns: 'usage'`. **These polyfills are automatically excluded if they are not needed for your target environments**.

Use this option when you have 3rd party dependencies that are not processed by Babel but have specific polyfill requirements (e.g. Axios and Vuex require Promise support).

### jsx

- 默认值：`true`。设为 `false` 可以禁用 JSX 支持。

### loose

- 默认值：`false`。设置为 `true` 时会生成更高效但少一些规范性的代码。

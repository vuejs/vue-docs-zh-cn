# @vue/babel-preset-app

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/babel-preset-app/README.md)

这是所有 Vue CLI 项目默认的 Babel 预设选项。

## 包含的内容

- [@babel/preset-env](https://new.babeljs.io/docs/en/next/babel-preset-env.html)
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

默认值是 `'usage'`，它会基于被转译代码的使用情况导入相应的 polyfill。例如，你在代码里使用了 `Object.assign`，那么如果你的目标环境不支持它，对应的 polyfill 就会被自动导入。

注意用法侦测并不适用于你的依赖 (它们默认被 `cli-plugin-babel` 排除在外)。如果你的一个依赖需要 polyfill，可以选择：

1. **如果这个依赖是用一个你的目标环境不支持的 ES 版本撰写的：**请把那个依赖添加到 `vue.config.js` 的 `transpileDependencies` 选项中。它会为这个依赖开启语法转换和基于用法的 polyfill 侦测。

2. **如果这个依赖交付 ES5 代码并显式地列出了需要的 polyfill：**你可以使用 [polyfills](#polyfills) 选项为这个预设选项预包含所需要的 polyfill。

3. **如果这个依赖交付 ES5 代码，但是使用了没有显性列出的 ES6+ 特性的 polyfill 需求 (例如 Vuetify)：**请使用 `useBuiltIns: 'entry'` 并将 `import '@babel/polyfill'` 添加到你的入口文件中。它会基于你的 `browserlist` 目标导入**所有的** polyfill，所以你无需再担心任何依赖中的 polyfill，不过因为它加入了一些未被使用的 polyfill，最终的包体积会增加。

查阅 [@babel/preset-env 文档](https://new.babeljs.io/docs/en/next/babel-preset-env.html#usebuiltins-usage)了解更多细节。

### polyfills

- 默认值：`['es6.promise']`

当使用 `useBuiltIns: 'usage'` 的时候预包含的一份 [core-js](https://github.com/zloirock/core-js) polyfill 的列表。**如果你的目标环境不需要，这些 polyfill 会自动被排除。**

当你拥有未被 Babel 处理但是需要特定的 polyfill 的第三方依赖时 (例如 Axios 和 Vuex 需要 Promise 的支持) 请使用这个选项。

### jsx

- 默认值：`true`。设为 `false` 可以禁用 JSX 支持。

### loose

- 默认值：`false`。设置为 `true` 时会生成更高效但少一些规范性的代码。

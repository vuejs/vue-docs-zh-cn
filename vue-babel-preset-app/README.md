# @vue/babel-preset-app

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/babel-preset-app/README.md)

这是所有 Vue CLI 项目默认的 Babel 预设选项。**注意：这个预设选项是仅用于通过 Vue CLI 创建的项目，并没有考虑外部的用例。**

## 包含的特性

### [@babel/preset-env](https://new.babeljs.io/docs/en/next/babel-preset-env.html)

`preset-env` 会基于你的浏览器目标自动决定要运用的语法转换和 polyfill。查阅[浏览器兼容性](https://cli.vuejs.org/zh/guide/browser-compatibility.html)章节了解更多。

- `modules: false`
  - 在 Jest 测试中会自动设置为 `'commonjs'`
- [`useBuiltIns: 'usage'`](#usebuiltins)
- `targets` 的判断规则：
  - 为浏览器构建时使用 `package.json` 中的 `browserslist` 字段
  - 在 Node.js 中运行测试时设置为 `{ node: 'current' }`
- 默认包含 `Promsie` polyfill，这样它们也可以用于非转译依赖 (只用于必要的环境)

### Stage 3 及以下

只支持下列 stage 3 及以下的特性 (object rest spread 作为 `preset-env` 的一部分已经被支持)：

- [Dynamic Import Syntax](https://github.com/tc39/proposal-dynamic-import)
- [Proposal Class Properties](https://babeljs.io/docs/en/next/babel-plugin-proposal-class-properties.html)
- [Proposal Decorators (legacy)](https://babeljs.io/docs/en/next/babel-plugin-proposal-decorators.html)

如果你需要更多的 stage 3 或以下的特性，可自行安装配置。

### Vue JSX 语法支持

- [@babel/plugin-syntax-jsx](https://github.com/babel/babel/tree/master/packages/babel-plugin-syntax-jsx)
- [babel-plugin-transform-vue-jsx](https://github.com/vuejs/babel-plugin-transform-vue-jsx)
- ~~[babel-plugin-jsx-event-modifiers](https://github.com/nickmessing/babel-plugin-jsx-event-modifiers)~~ (在 Babel 7 修复之前暂时不可用)
- ~~[babel-plugin-jsx-v-model](https://github.com/nickmessing/babel-plugin-jsx-v-model)~~ (在 Babel 7 修复之前暂时不可用)

### [@babel/plugin-transform-runtime](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-runtime)

`transform-runtime` 会避免在每个文件里内联帮助代码 (helper)。因为 polyfill 会被 `babel-preset-env` 处理，所以该选型只对帮助代码有效。

## 选项

- 支持所有来自 [@babel/preset-env](https://babeljs.io/docs/en/next/babel-preset-env.html) 的选型，还为部分选型提供了更智能的默认值。

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

如果你在构建一个库或 web component 而不是一个应用，可能需要将其设置为 `false`，让使用它的应用来决定这些 polyfill。

注意用法侦测并不适用于你的依赖 (它们默认被 `cli-plugin-babel` 排除在外)。如果你的一个依赖需要 polyfill，可以选择：

1. **如果这个依赖是用一个你的目标环境不支持的 ES 版本撰写的：**请把那个依赖添加到 `vue.config.js` 的 `transpileDependencies` 选项中。它会为这个依赖开启语法转换和基于用法的 polyfill 侦测。

2. **如果这个依赖交付 ES5 代码并显式地列出了需要的 polyfill：**你可以使用 [polyfills](#polyfills) 选项为这个预设选项预包含所需要的 polyfill。

3. **如果这个依赖交付 ES5 代码，但是使用了没有显性列出的 ES6+ 特性的 polyfill 需求 (例如 Vuetify)：**请使用 `useBuiltIns: 'entry'` 并将 `import '@babel/polyfill'` 添加到你的入口文件中。它会基于你的 `browserlist` 目标导入**所有的** polyfill，所以你无需再担心任何依赖中的 polyfill，不过因为它加入了一些未被使用的 polyfill，最终的包体积会增加。

查阅 [@babel/preset-env 文档](https://new.babeljs.io/docs/en/next/babel-preset-env.html#usebuiltins-usage)了解更多细节。

### polyfills

- 默认值：`['es6.array.iterator', 'es6.promise', 'es6.object.assign', 'es7.promise.finally']`

当使用 `useBuiltIns: 'usage'` 的时候预包含的一份 [core-js](https://github.com/zloirock/core-js) polyfill 的列表。**如果你的目标环境不需要，这些 polyfill 会自动被排除。**

当你拥有未被 Babel 处理但是需要特定的 polyfill 的第三方依赖时 (例如 Axios 和 Vuex 需要 Promise 的支持) 请使用这个选项。

### jsx

- 默认值：`true`。设为 `false` 可以禁用 JSX 支持。

### loose

- 默认值：`false`。设置为 `true` 时会生成更高效但少一些规范性的代码。

### entryFiles

- 默认值：`[]`

为多页面仓库使用 `entryFiles` 来确保每个入口文件都注入了 polyfill。

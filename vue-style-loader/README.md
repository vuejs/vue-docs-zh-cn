# Vue Style Loader [![Build Status](https://circleci.com/gh/vuejs/vue-style-loader/tree/master.svg?style=shield)](https://circleci.com/gh/vuejs/vue-loader/tree/master) [![npm package](https://img.shields.io/npm/v/vue-style-loader.svg)](https://www.npmjs.com/package/vue-style-loader)

[英文原版](https://github.com/vuejs/vue-style-loader)

这个库 fork 自 [`style-loader`](https://github.com/webpack/style-loader)。和 `style-loader` 一样，你可以在 `css-loader` 之后将其链起来，并通过它将 CSS 以样式标记的方式动态注入到文档中。不过因为这个库已经在 `vue-loader` 的默认依赖中，多数情况下你是不需要自行配置该 loader 的。

## 选项

- **manualInject** (3.1.0+):

  类型：`boolean`。当从一个非 Vue 文件导入样式的时候，默认情况下样式会作为该导入内容的一个副作用被注入。当 `manualInject` 为真多时候，被导入的样式对象会暴露一个 `__inject__` 方法，供您自行选择恰当的时机手动调用。如果这个方法在服务端被调用，则会接收一个待添加样式的 `ssrContext` 的参数。

  ``` js
  import styles from 'styles.scss'

  export default {
    beforeCreate() { // 或者为此创建一个混入 (mixin)
      if(styles.__inject__) {
        styles.__inject__(this.$ssrContext)
      }
    }

    render() {
      return <div class={styles.heading}>Hello World</div>
    }
  }
  ```

  注意当 `vue-style-loader` 用在从一个 `*.vue` 文件内导入样式的时候，这个行为是自动开启的。暴露该选项只是为了高阶的用法。

- **ssrId** (3.1.0+):

  类型：`boolean`。为被注入的 `<style>` 标签添加 `data-vue-ssr-id` 特性，哪怕它现在不在 Node.js 中。它可以用来预渲染 (而不是服务端渲染) 而避免在水合 (hydration) 过程中重复的样式注入。

## 和 `style-loader` 的不同

### 对服务端渲染的支持

当以 `target: 'node'` 为目标打包时，所有被渲染的组件会被收集和暴露为 Vue 的渲染上下文对象中的 `context.styles`，你可以将其简单的内联到你的 `<head>` 标记中。如果你在构建一个 Vue SSR 应用，你可能也希望使用这个 loader 从 JavaScript 文件导入 CSS。

### 其它

- 不支持 URL 模式和引用计数模式。也移除了 `singletion` 和 `insertAt` 查询选项。它总是自动选取最合理的样式插入机制。如果你需要这些能力，你可能应该使用原始的 `style-loader` 取而代之。

- 修复了针对 chrome:// 的相对根 URL 被解析的问题，以及为被注入 Chrome 的 `<style>` 标记制作 source map URL 的问题。

## 协议

MIT

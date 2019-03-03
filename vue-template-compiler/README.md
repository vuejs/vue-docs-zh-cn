# Vue Template Compiler

[英文原版](https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler/README.md)

> 这个包是自动生成的。如果想创建 pull request 请查阅 [src/platforms/web/entry-compiler.js](https://github.com/vuejs/vue/tree/dev/src/platforms/web/entry-compiler.js)。

这个包可以被用来将 Vue 2.0 的模板预编译为渲染函数以避免运行时不必要的编译开销和 CSP 的限制。大多数情况下你应该使用 [`vue-loader`](https://github.com/vuejs/vue-loader)，你只在为特殊需求撰写构建工具时需要独立使用它。

## 安装

``` bash
npm install vue-template-compiler
```

``` js
const compiler = require('vue-template-compiler')
```

## API

### `compiler.compile(template, [options])`

编译一个模板字符串并返回编译好的 JavaScript 代码。返回结果的格式如下：

``` js
{
  ast: ?ASTElement, // 由模板元素解析为的抽象语法树 (AST)
  render: string, // 主渲染函数的代码
  staticRenderFns: Array<string>, // 可能存在的静态子树的渲染代码
  errors: Array<string> // 可能存在的模板语法错误
}
```

注意返回的函数代码使用了 `with` 因此无法用在严格模式的代码中。

#### 选项

你有机会在编译过程中嵌入钩子从而支持自定义模板特性。**然而要注意，在注入自定义编译时模块之后，你的模板将不会在基于标准内建模块通过构建工具构建出来的版本，诸如 `vue-loader` 和 `vueify` 等中正常工作。**

可选的 `options` 对象可以包含以下内容：

- `modules`

一个编译器模块的数组。对于编译器模块的细节，请参阅 [flow 声明](https://github.com/vuejs/vue/blob/dev/flow/compiler.js#L39-L51)中的 `ModuleOptions` 类型以及[内建模块](https://github.com/vuejs/vue/tree/dev/src/platforms/web/compiler/modules)。

- `directives`

  一个对象，其键是指令的名称，其值是一个函数，可以转换一个模板 AST 的结点。例如：

  ``` js
  compiler.compile('<div v-test></div>', {
    directives: {
      test (node, directiveMeta) {
        // transform node based on directiveMeta
      }
    }
  })
  ```

  默认情况下，一个编译时指令会提取自身，这个指令在运行时就不出现了。如果你希望该指令也能够被一个运行时的定义处理，请在转换函数中返回 `true`。

  可以参阅一些[内建编译时指令](https://github.com/vuejs/vue/tree/dev/src/platforms/web/compiler/directives)的实现细节。

- `preserveWhitespace`

  默认为 `true`。这意味着编译好的渲染函数会保留所有 HTML 标签之间的空格。如果设置为 `false`，则标签之间的空格会被忽略。这能够略微提升一点性能但是可能会影响到内联元素的布局。

---

### `compiler.compileToFunctions(template)`

和 `compiler.compile` 相似，但是会直接返回实例化的函数：

``` js
{
  render: Function,
  staticRenderFns: Array<Function>
}
```

只用于通过预配置构建的运行时，因此它不会接受任何编译时的选项。另外这个方法内使用了 `new Function()` 所以不兼容 CSP。

---

### `compiler.ssrCompile(template, [options])`

> 2.4.0+

和 `compiler.compile` 相同，但是会生成针对服务端渲染的渲染函数代码，其优化了部分模板到字符串的串联以提升服务端渲染的性能。

在 `vue-loader@>=12` 这是默认使用的，并且可以通过 [`optimizeSSR`](https://vue-loader.vuejs.org/zh-cn/options.html#optimizessr) 选项禁用。

---

### `compiler.ssrCompileToFunctions(template)`

> 2.4.0+

和 `compiler.compileToFunction` 相同，但是生成的是针对服务端渲染的渲染函数代码，其优化了部分模板到字符串的串联以提升服务端渲染的性能。

---

### `compiler.parseComponent(file, [options])`

解析一个 SFC (单文件组件，或 `*.vue` 文件) 解析为一个描述器 (参考 [flow 声明](https://github.com/vuejs/vue/blob/dev/flow/compiler.js) 中的 `SFCDescriptor` 类型)。用于诸如 `vue-loader` 和 `vueify` 等 SFC 构建工具。

#### 选项

#### `pad`

`pad` 在你把提取的内容通过管道传递给其它预处理器时是很有用的，因为在遇到任何语法错误的时候，你需要核对行数或字符索引。

- 使用 `{ pad: "line" }` 后，针对被提取的每个内容块，都会在其内容之前加入和其源文件上该内容块之前的内容行数相对应个数的空行，以确保内容的行号和源文件对应。
- 使用 `{ pad: "space" }` 后，针对被提取的每个内容块，都会在其内容之前加入和其源文件上该内容块之前的内容字符数相对应个数的空格，以确保内容的字符位置和源文件对应。

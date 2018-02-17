## 处理静态资源

- [相对路径导入](#相对路径导入)
  - [URL 转换规则](#url-转换规则)
- [`public` 文件夹](#public-文件夹)
  - [何时使用 `public` 文件夹](#何时使用-public-文件夹)

### 相对路径导入

当你在 JavaScript、CSS 或 `*.vue` 文件中使用相对路径引用一个静态资源时，该资源将会被包含进入 webpack 的依赖图中。在其编译过程中，所有诸如 `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL **都会被解析为一个模块依赖**。

例如，`url(./image.png)` 会被翻译为 `require('./image.png')`，而：

``` html
<img src="../image.png">
```

将会被编译到：

``` js
createElement('img', { attrs: { src: require('../image.png') }})
```

在其内部，我们通过 `file-loader` 用哈希版本和正确的公共基础路径来决定最终的文件路径，再用 `url-loader` 将小于 10kb 的资源有条件的内联，以减少 HTTP 请求的数量。

#### URL 转换规则

- 如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，它将会被保留为原有的样子。

- 如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于你的文件系统中的目录结构进行解析。

- 如果 URL 以 `~` 开头，其后的任何内容都会作为一个模块请求被解释。这意味着你甚至可以引用 node 模块中的资源：

  ``` html
  <img src="~/some-npm-package/foo.png">
  ```

- 如果 URL 以 `@` 开头，它也会作为一个模块请求被解释。它的用处在于 Vue CLI 默认会设置一个指向 `<projectRoot>/src` 的别名 `@`。

### `public` 文件夹

任何放置在 `public` 文件夹的静态资源都会被简单的复制，而不经过 webpack。你需要通过绝对路径来引用它们。

注意我们推荐将资源作为你的模块依赖图的一部分导入，这样它们会通过 webpack 的处理并获得如下好处：

- 脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。
- 文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
- 最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们到老版本。

`public` 目录提供的是一个**逃生通道**，当你通过绝对路径引用它时，你需要留意你的应用将会部署到哪里。如果你的应用没有部署在域名的根部，那么你需要为你的 URL 配置基础路径前缀：

- 在 `public/index.html` 中，你需要通过 `<%= webpackConfig.output.publicPath %>` 设置链接前缀：

  ``` html
  <link rel="shortcut icon" href="<%= webpackConfig.output.publicPath %>favicon.ico">
  ```

- 在模板中，你首先需要向你的组件传入基础 URL：

  ``` js
  data () {
    return {
      baseUrl: process.env.BASE_URL
    }
  }
  ```

  Then:

  ``` html
  <img :src="`${baseUrl}my-image.png`">
  ```

#### 何时使用 `public` 文件夹

- 你需要在构建输出中指定一个文件的名字。
- 你有上千个图片，需要动态引用它们的路径。
- 有些库可能和 webpack 不兼容，这时你除了将其用一个独立的 `<script>` 标签引入没有别的选择。

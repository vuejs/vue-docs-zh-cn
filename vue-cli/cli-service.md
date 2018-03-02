## CLI 服务

- [使用可执行程序](#使用可执行程序)
- [提供服务](#提供服务)
  - [配置代理](#配置代理)
- [构建](#构建)
  - [缓存和并行模式](#缓存和并行模式)
  - [构建为一个库或 Web Component](#构建为一个库或-web-component)
  - [DLL 模式](#dll-模式)
- [审查](#审查)
- [查阅所有可用的命令](#查阅所有可用的命令)

### 使用可执行程序

在一个 Vue CLI 项目中，`@vue/cli-service` 会安装一个名为 `vue-cli-service` 的可执行程序。你可以在 npm scripts 中直接执行 `vue-cli-service`、或从终端执行 `./node_modules/.bin/vue-cli-service` 访问这个命令。

这是你将会在项目的 `package.json` 中看到的默认预设选项：

``` json
{
  "scripts": {
    "serve": "vue-cli-service serve --open",
    "build": "vue-cli-service build"
  }
}
```

### 提供服务

```
Usage: vue-cli-service serve [options]

Options:

  --open    在服务器开启时打开浏览器
  --mode    指定环境模式 (默认值：`development`)
  --host    指定主机 (默认值：`0.0.0.0`)
  --port    指定端口 (默认值：`8080`)
  --https   使用 https (默认值：`false`)
```

`vue-cli-service serve` 会基于 [webpack-dev-server](https://github.com/webpack/webpack-dev-server) 开启一个开发服务器。同时也包括开箱即用的模块热替换 (即 HMR：hot-module-replacement)。

你可以使用 `vue.config.js` 文件中的 `devServer` 选项配置开发服务器的行为：

``` js
module.exports = {
  devServer: {
    open: process.platform === 'darwin',
    host: '0.0.0.0',
    port: 8080,
    https: false,
    hotOnly: false,
    proxy: null, // string | Object
    before: app => {
      // `app` 是一个 express 实例
    }
  }
}
```

除了这些默认值之外，我们也支持[所有的 `webpack-dev-server` 选项](https://webpack.js.org/configuration/dev-server/)。

#### 配置代理

`devServer.proxy` 可以是一个指向开发环境 API 服务器的字符串：

``` js
module.exports = {
  devServer: {
    proxy: 'http://localhost:4000'
  }
}
```

这会使得开发服务器将任何不认识的请求 (没有命中任何静态文件的请求) 代理到 `http://localhost:4000`。

如果你想要控制更多代理行为，你也可以使用一个 `path: options` 的配对对象。你可以在 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware#proxycontext-config) 看到完整的选项：

``` js
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: '<url>',
        ws: true,
        changeOrigin: true
      },
      '/foo': {
        target: '<other_url>'
      }
    }
  }
}
```

### 构建

```
Usage: vue-cli-service build [options] [entry|pattern]

Options:

  --mode    指定环境模式 (默认值：`production`)
  --dest    指定输出目录 (默认值：`dist`)
  --target  app | lib | wc | wc-async (默认值：`app`)
  --name    库或 web-component 模式下的名字 (默认值：`package.json` 中的 `name` 字段或入口文件名)
```

`vue-cli-service build` 会在 `dist/` 目录生成一个可用于生产环境的包，这个包会最小化 JS/CSS/HTML 并为了更好的缓存效果对包进行自动分块。分块的清单会内联到 HTML 中。

#### 缓存和并行模式

- `cache-loader` 会为 Babel/TypeScript 的编译默认开启。
- `thread-loader` 会为 Babel/TypeScript 的编译在多核机器下默认开启。

#### 构建为一个库或 Web Component

我们也有可能在你的项目中将任何组件构建成为一个库或 web component。请在[构建目标](./build-targets.md)查阅更多细节。

#### DLL 模式

如果你的应用有大量的依赖库，你可以开启 DLL 模式以提升构建的性能。DLL 模式会将你的依赖构建到一个独立的供应包，这个包会在未来的构建过程中被复用直到你的依赖发生变化。

想要开启 DLL 模式，请在 `vue.config.js` 中设置 `dll` 选项为 `true`：

``` js
// vue.config.js
module.exports = {
  dll: true
}
```

这会默认将**`package.json` 内 `dependencies` 字段列出的所有模块**构建到这个 DLL 包。正确列出你的依赖非常重要，否则它最终可能会引入不必要的代码。

如果你希望能更细粒度的控制向 DLL 包引入哪些模块，你也可以在 `dll` 选项中提供一个模块的数组：

``` js
// vue.config.js
module.exports = {
  dll: [
    'dep-a',
    'dep-b/some/nested/file.js'
  ]
}
```

### 审查

```
Usage: vue-cli-service inspect [options] [...paths]

Options:

  --mode    指定环境模式 (默认值：`development`)
```

你可以在一个 Vue CLI 项目中使用 `vue-cli-service inspect` 来审查 webpack 配置。请在[审查 webpack 配置](./webpack.md#inspecting-the-projects-webpack-config)查阅更多细节。

### 查阅所有可用的命令

有些 CLI 插件会为 `vue-cli-service` 注入额外的命令。比如 `@vue/cli-plugin-eslint` 注入了 `vue-cli-service lint` 命令。你可以运行下面的命令来查阅所有已注入的命令：

``` sh
./node_modules/.bin/vue-cli-service help
```

你也可以通过下面的命令来学习每个命令可用的选项：

``` sh
./node_modules/.bin/vue-cli-service help [command]
```

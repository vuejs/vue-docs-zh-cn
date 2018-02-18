## 配置 webpack

- [基础配置](#基础配置)
- [操作链](#操作链-高级)
- [修改插件选项](#修改插件选项)
- [审查配置](#审查项目的-webpack-配置)
- [以一个文件的方式使用解析好的配置](#以一个文件的方式使用解析好的配置)

### 基础配置

调整 webpack 配置最简单的方式就是在 `vue.config.js` 中的 `configureWebpack` 选项提供一个对象：

``` js
// vue.config.js
module.exports = {
  configureWebpack: {
    plugins: [
      new MyAwesomeWebpackPlugin()
    ]
  }
}
```

该对象将会被 [webpack-merge](https://github.com/survivejs/webpack-merge) 合并入最终的 webpack 配置。

如果你需要有条件的基于环境配置行为，或者想要直接修改配置，那就换成一个函数 (该函数会在环境变量被设置之后懒执行)。该方法的第一个参数会收到已经解析好的配置。在函数内，你可以直接修改配置，或者返回一个将会被合并的对象：

``` js
// vue.config.js
module.exports = {
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  }
}
```

### 操作链 (高级)

webpack 内部的配置是通过 [webpack-chain](https://github.com/mozilla-neutrino/webpack-chain) 维护的。这个库提供了一个 webpack 原始配置的上层抽象，使其可以定义具名的 loader 规则和具名插件，并有机会在后期进入这些规则并对它们的选项进行修改。

它允许我们更细粒度的控制其内部配置。这里有一些例子：

#### 编译一个依赖模块

默认情况下 Babel 配置会跳过：

``` js
// vue.config.js
module.exports = {
  chainWebpack: config => {
    config.module
      .rule('js')
        .include
          .add(/some-module-to-transpile/)
  }
}
```

#### 修改插件选项

``` js
// vue.config.js
module.exports = {
  chainWebpack: config => {
    config
      .plugin('html')
      .tap(args => {
        return [/* new args to pass to html-webpack-plugin's constructor */]
      })
  }
}
```

你需要熟悉 [webpack-chain 的 API](https://github.com/mozilla-neutrino/webpack-chain#getting-started) 并[阅读一些源码](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-service/lib/config)以便了解如何权衡这个选项的全部能量，但是它给了你比直接修改 webpack 配置中的值更生动且安全的方式。

### 审查项目的 webpack 配置

因为 `@vue/cli-service` 抽象了 webpack 配置，所以理解配置中包含的东西会比较困难，尤其是当你打算自行对其调整的时候。

`vue-cli-service` 暴露了 `inspect` 命令用于审查解析好的 webpack 配置。那个全局的 `vue` 可执行程序同样提供了 `inspect` 命令，这个命令只是简单的把 `vue-cli-service inspect` 代理到了你的项目中。

默认情况下该命令会打印到 stdout，当然为了更易于查阅你可以将其重定向到一个文件：

``` sh
vue inspect > output.js
```

注意它输出的并不是一个有效的 webpack 配置文件，而是一个用于审查的被序列化的格式。

你也可以审查某一个路径来缩小配置的范围：

``` sh
# 只审查第一条规则
vue inspect module.rules.0
```

### 以一个文件的方式使用解析好的配置

有些外部工具可能需要通过一个文件访问解析好的 webpack 配置，比如那些需要提供 webpack 配置路径的 IDE 或 CLI。在这种情况下你可以使用如下路径：

```
<projectRoot>/node_modules/@vue/cli-service/webpack.config.js
```

该文件会动态解析并输出 `vue-cli-service` 命令中使用的相同的 webpack 配置，包括那些来自插件甚至是你自定义的配置。

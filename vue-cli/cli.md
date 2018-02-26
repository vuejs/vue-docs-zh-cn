## CLI

- [安装](#安装)
- [使用](#使用)
- [创建一个新的项目](#创建一个新的项目)
- [预设选项](#预设选项)
- [零配置原型](#零配置原型)
- [在一个已有的项目中安装插件](#在一个已有的项目中安装插件)
- [审查 webpack 配置](#审查-webpack-配置)
- [拉取 2.x 模板](#拉取-vue-cli2x-模板-遗留)

### 安装

``` sh
npm install -g @vue/cli
vue create my-project
```

### 使用

```
Usage: vue <command> [options]

Commands:

  create [options] <app-name>      创建一个由 `vue-cli-service` 提供支持的新项目
  invoke <plugin> [pluginOptions]  在一个已有的项目中调用一个插件的生成器
  inspect [options] [paths...]     在一个基于 `vue-cli-service` 的项目中审查 webpack 配置
  serve [options] [entry]          在开发环境下零配置服务一个 `.js` 或 `.vue` 文件
  build [options] [entry]          在生产环境下领配置构建一个 `.js` 或 `.vue` 文件
  init <template> <app-name>       从一个远程模板生成一个项目 (遗留 API, 依赖 `@vue/cli-init`)
```

你也可以使用 `vue <command> --help` 查阅每个命令的更多使用细节。

### 创建一个新的项目

```
Usage: create [options] <app-name>

创建一个由 `vue-cli-service` 提供支持的新项目


Options:

  -p, --preset <presetName>       忽略提示符并使用已保存的预设选项
  -d, --default                   忽略提示符并使用默认预设选项
  -i, --inlinePreset <json>       忽略提示符并使用内联的 JSON 字符串预设选项
  -m, --packageManager <command>  在安装依赖时使用指定的 npm 客户端
  -r, --registry <url>            在安装依赖时使用指定的 npm registry (仅用于 npm 客户端)
  -f, --force                     覆写目标目录可能存在的配置
  -h, --help                      输出使用帮助信息
```

``` sh
vue create my-project
```

<p align="center">
  <img width="682px" src="https://raw.githubusercontent.com/vuejs/vue-cli/dev/docs/screenshot.png">
</p>

#### 预设选项

在你选择了特性之后，你可以选择性的将其保持为预设选项，这样你就可以在未来的项目中反复使用了。如果你想要删除一个已保存的预设选项，可以编辑 `~/.vuerc`。

### 零配置原型

你可以通过 `vue serve` 和 `vue build` 命令用一个单独的 `*.vue` 文件快速创建原型，但是它们需要你先安装一个额外的全局插件：

``` sh
npm install -g @vue/cli-service-global
```

`vue serve` 的缺点是它依赖全局安装的依赖，这可能在不同的电脑上是不完全一样的。因此这只推荐用于快速创建原型。

#### vue serve

```
Usage: serve [options] [entry]

在开发环境下零配置服务一个 `.js` 或 `.vue` 文件


Options:

  -o, --open  打开浏览器
  -h, --help  输出使用帮助信息
```

所有你需要的就是一个 `*.vue` 文件：

``` sh
echo '<template><h1>Hello!</h1></template>' > App.vue
vue serve
```

`vue serve` 使用和 `vue create` 创建的项目相同的默认设置 (webpack, babel, postcss 和 eslint)。它自动找出当前目录的入口文件——这个入口可以是 `main.js`、`index.js`、`App.vue` 或 `app.vue` 中的一个。你也可以显性的指定入口文件：

``` sh
vue serve MyComponent.vue
```

如有需要，你也可以提供 `index.html`、`package.json`，安装和使用本地依赖，甚至通过相应的配置文件配置 babel、postcss 和 eslint。

#### vue build

```
Usage: build [options] [entry]

在生产环境下领配置构建一个 `.js` 或 `.vue` 文件


Options:

  -t, --target <target>  构建目标 (app | lib | wc | wc-async，默认值：`app`)
  -n, --name <name>      库或 web component 的名字 (默认值为入口文件名)
  -d, --dest <dir>       输出目录 (默认值：`dist`)
  -h, --help             输出使用帮助信息
```

你也可以通过 `vue build` 将目标文件构建成为一个用于发布到生产环境的包：

``` sh
vue build MyComponent.vue
```

`vue build` 也提供了将组件构建成为一个库或 web component 的能力。请在[构建目标](./build-targets.md)查阅更多细节。

### 在一个已有的项目中安装插件

每个 CLI 插件都包含了一个 (创建文件的) 生成器和一个 (调整 webpack 核心配置和注入命令的) 运行时插件。当你使用 `vue create` 创建一个新项目时，就会根据你选择的特性预装一些插件。而当你想要在一个已有的项目中装入一个插件时，第一步可以简单的先将其安装：

``` sh
npm install -D @vue/cli-plugin-eslint
```

然后你可以调用插件的生成器以在你的项目中生成文件：

``` sh
# `@vue/cli-plugin-` 前缀可以省略
vue invoke eslint
```

额外的，你可以向插件传递选项：

``` sh
vue invoke eslint --config airbnb --lintOn save
```

这里推荐在运行 `vue invoke` 之前将你项目当前的状态提交，以便在生成文件之后回顾文件变更，并在必要的时候将这些变更恢复。

### 审查 webpack 配置

你可以在一个 Vue CLI 项目中使用 `vue inspect` 来审查 webpack 配置。请在[审查 webpack 配置](./webpack.md#inspecting-the-projects-webpack-config)查阅更多细节。

### 拉取 `vue-cli@2.x` 模板 (遗留)

因为使用了 `vue-cli@2.x` 相同的 `vue` 可执行程序，所以 `@vue/cli` 会将其覆写。如果你仍然需要遗留的 `vue init` 功能，你可以安装一个全局的桥：

``` sh
npm install -g @vue/cli-init
# 现在 `vue init` 可以完全和 `vue-cli@2.x` 一样工作了
vue init webpack my-project
```

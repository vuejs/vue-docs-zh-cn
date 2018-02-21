# Vue CLI

[英文原版](https://github.com/vuejs/vue-cli/)

## 目录

- [介绍](#介绍)
- [CLI](#cli)
- [CLI 服务](#cli-服务)
- [约定](#约定)
  - [Index 页面](#index-页面)
  - [处理静态资源](#处理静态资源)
  - [环境变量和模式](#环境变量和模式)
- [配置](#配置)
  - [webpack](#webpack)
  - [browserslist](#browserslist)
  - [开发服务器代理](#开发服务器代理)
  - [Babel](#babel)
  - [CSS](#css)
  - [ESLint](#eslint)
  - [TypeScript](#typescript)
  - [Unit Testing](#unit-testing)
  - [E2E Testing](#e2e-testing)
- [开发](#开发)

## 介绍

Vue CLI 是一个 Vue.js 快速开发的完整系统，提供：

- 通过 `@vue/cli` 搭建交互式项目脚手架。
- 通过 `@vue/cli` + `@vue/cli-service-global` 快速创建零配置原型。
- 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  - 可升级；
  - 基于 webpack 构建，并带有智能的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写你的应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需跳出。

## CLI

CLI 会被全局安装并在终端里提供 `vue` 命令：

``` sh
npm install -g @vue/cli
vue create my-project
```

请在 [CLI 文档](./cli.md)查阅其所有的可用命令。

## CLI 服务

`@vue/cli-service` 是一个安装在每个项目本地的依赖，通过 `@vue/cli` 创建。它包含了为你的项目加载其它插件、解析最终的 webpack 配置、提供 `vue-cli-service` 可执行程序等核心服务。如果你熟悉 [create-react-app](https://github.com/facebookincubator/create-react-app) 的话，`@vue/cli-service` 实际上等价于 `react-scripts`，但是会更灵活。

请在 [CLI 服务文档](./cli-service.md)查阅其所有的可用命令。

## 约定

### Index 页面

页面 `public/index.html` 是一个会经过 [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) 处理的模板。在构建过程中，资源链接将会自动注入其中。额外的，Vue CLI 也会自动插入资源暗示 (`preload/prefetch`)，`manifest/icon` 链接 (当使用 PWA 插件时)，并内联 webpack 运行时/ chunk 的 manifest 清单以优化性能。

### 处理静态资源

我们可以通过以下两种方式处理静态资源：

- 在 JavaScript 中导入或在 template/CSS 中通过相对路径引用。这些引用会被 webpack 处理。

- 放入 `public` 目录并通过绝对路径引用。这些资源会被简单的复制过去，并不通过 webpack。

请在[处理静态资源](./assets.md)查阅更多细节。

### 环境变量和模式

基于不同的目标环境定制应用的行为是一个常见的需求——比如你可能想让应用在 development / staging / production 环境下使用不同的 API endpoint 或资格认证。

Vue CLI 通过模式切换和 `.env` 文件为指定不同环境变量做了全面的支持。

请在[环境变量和模式](./env.md)查阅更多细节。

## 配置

从 `vue create` 创建的项目是可以“开箱即用”的。而插件是为和其它工具协作而设计的。所以多数情况下，所有你需要做的仅仅是在交互式提示符中选取你想要的功能。

然而，我们也明白尽可能照顾到每一个需求是非常重要的，同时一个项目的需求也会经常改变。通过 Vue CLI 创建的项目几乎允许你从每一个工具的角度进行配置，无需跳出。

### `vue.config.js`

在一个 Vue CLI 项目的根目录放置一个 `vue.config.js` 文件，可以配置该项目的很多方面。如果你在创建项目时选了一些功能，那么这个文件可能已经创建好了。

`vue.config.js` 应该导出一个对象，例如：

``` js
// vue.config.js
module.exports = {
  lintOnSave: true
}
```

请在[这里](./config.md)查阅完整的可配置选项列表。

### webpack

我们最常见的配置需求大概就是调整内部的 webpack 配置了。Vue CLI 为其提供了灵活的解决方案，无需跳出。

请在[这里](./webpack.md)查阅完整的细节。

### browserslist

你可能注意到了 `package.json` 中的 `browserslist` 字段指定了该项目的目标浏览器支持范围。这个值将会被用在 `babel-preset-env` 和 `autoprefixer` 中以自动判断需要的 JavaScript polyfill 和 CSS 浏览器前缀。

请在[这里](https://github.com/ai/browserslist)查阅如何指定浏览器支持范围。

### 开发服务器代理

如果你的前端应用和后端 API 服务器并没有运行在相同的主机上，你在开发时会需要将 API 请求代理到 API 服务器。这个问题可以通过 `vue.config.js` 文件中的 `devServer.proxy` 选项来配置。

请在[配置代理](./cli-service.md#configuring-proxy)查阅更多细节。

### Babel

Babel 可以通过 `.babelrc` 或 `package.json` 中的 `babel` 字段进行配置。

所有 Vue CLI 应用使用了 `@vue/babel-preset-app`，它包含了 `babel-preset-env`、JSX 支持和为最小化包体积做的优化配置。请在[其文档](../vue-babel-preset-app/README.md)查阅相关细节和预设选项。

### CSS

Vue CLI 项目自身支持了 [PostCSS](http://postcss.org/)、[CSS Modules](https://github.com/css-modules/css-modules) 以及包含 [Sass](https://sass-lang.com/)、[Less](http://lesscss.org/) 和 [Stylus](http://stylus-lang.com/) 在内的预处理器。

请在[这里](./css.md)查阅更多 CSS 相关的配置细节。

### ESLint

ESLint 可以通过 `.eslintrc` 或 `package.json` 文件中的 `eslintConfig` 字段进行配置。

请在 [@vue/cli-plugin-eslint](../vue-cli-plugin-eslint/README.md) 查阅更多细节。

### TypeScript

TypeScript 可以通过 `tsconfig.json` 进行配置。

请在 [@vue/cli-plugin-typescript](../vue-cli-plugin-typescript/README.md) 查阅更多细节。

### Unit Testing

- #### Jest

  请在 [@vue/cli-plugin-unit-jest](../vue-cli-plugin-unit-jest/README.md) 查阅更多细节。

- #### Mocha (via `mocha-webpack`)

  请在 [@vue/cli-plugin-unit-mocha](../vue-cli-plugin-unit-mocha/README.md) 查阅更多细节。

### E2E Testing

- #### Cypress

  请在 [@vue/cli-plugin-e2e-cypress](../vue-cli-plugin-e2e-cypress/README.md) 查阅更多细节。

- #### Nightwatch

  请在 [@vue/cli-plugin-e2e-nightwatch](../vue-cli-plugin-e2e-nightwatch/README.md) 查阅更多细节。

## 开发

- [参与贡献指南 (英)](./CONTRIBUTING.md)
- [插件开发指南](./plugin-dev.md)

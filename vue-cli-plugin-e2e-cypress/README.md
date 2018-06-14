# @vue/cli-plugin-e2e-cypress

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-e2e-cypress/README.md)

> vue-cli 的 e2e-cypress 插件

这个插件使用 [Cypress](https://www.cypress.io/) 添加 e2e 测试支持。

Cypress 为运行 e2e 测试提供一套富交互界面，但是目前只支持在 Chromium 上运行测试。如果你有对多浏览器进行 e2e 测试的强烈需求，可以考虑选用基于 Selenium 的 [@vue/cli-plugin-e2e-nightwatch](../vue-cli-plugin-e2e-nightwatch/README.md)。

## 注入的命令

- **`vue-cli-service test:e2e`**

  使用 `cypress run` 运行 e2e 测试。

  默认它通过一个可视化界面启动 Cypress 的交互模式。如果你想要以无头模式 (例如在 CI 的场景下) 运行测试，你可以加入 `--headless` 选项。

  这个命令会自动以生产环境模式开启一个服务器来运行 e2e 测试。如果你连续运行多次测试而不会每次都重启服务器，你可以在一个终端运行 `vue-cli-service serve --mode production` 启动服务器，然后使用 `--url` 选项指定服务器运行 e2e 测试。

  选项：

  ```
  --headless 不带可视化界面在无头模式下运行
  --mode     指定开发服务器应该运行的环境。(默认：生产环境)
  --url      通过给定的 URL 运行 e2e 测试，而不是自动开启开发服务器
  -s, --spec (只适用于无头模式) 运行一个明确的 spec 文件。默认是 "all"
  ```

  额外的：

  - 在 GUI 模式下，[支持所有 Cypress CLI `cypress open` 的选项](https://docs.cypress.io/guides/guides/command-line.html#cypress-open)
  - 在 `--headless` 模式下，[支持所有 Cypress CLI `cypress run` 的选项](https://docs.cypress.io/guides/guides/command-line.html#cypress-run)

## 配置

我们已经预配置了 Cypress 在 `<projectRoot>/test/e2e` 放置大多数的 e2e 测试的相关文件。你可以查阅[如何通过 `cypress.json` 配置 Cypress](https://docs.cypress.io/guides/references/configuration.html#Options)。

## 环境变量

Cypress 不会和 `vue-cli` 为你的[应用代码](https://cli.vuejs.org/zh/guide/mode-and-env.html#在客户端侧代码中使用环境变量)一样为你的测试文件加载 `.env` 文件。Cypress 支持一些[定义环境变量](https://docs.cypress.io/guides/guides/environment-variables.html#)的方式，但是最简单的一种就是使用 `.json` 文件 (`cypress.json` 或 `cypress.env.json`) 定义环境变量。注意这些变量是可以通过 `Cypress.env` 函数访问到的，而不是惯用的 `process.env` 对象。

## 在已创建的项目中安装

``` sh
vue add @vue/e2e-cypress
```

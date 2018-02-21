# @vue/cli-plugin-unit-jest

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-unit-jest/README.md)

> vue-cli 的 unit-jest 插件

## 注入的命令

- **`vue-cli-service test`**

通过 Jest 运行单元测试。默认的文件匹配规则是：

  - 任何 `test/unit` 中以 `.spec.(js|ts)` 结尾的文件；
  - 任何 `__tests__` 目录中的 js/ts 文件。

  使用：`vue-cli-service test [options] <regexForTestFiles>`

  支持所有的 [Jest 命令行选项](https://facebook.github.io/jest/docs/en/cli.html)。

## 配置

Jest 可以通过你项目根目录中的 `jest.config.js` 文件或 `package.json` 文件中 `jest` 字段的进行配置。

## 在已创建的项目中安装

``` sh
npm install -D @vue/cli-plugin-unit-jest
vue invoke unit-jest
```

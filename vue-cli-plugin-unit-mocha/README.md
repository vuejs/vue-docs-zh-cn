# @vue/cli-plugin-unit-mocha

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-unit-mocha/README.md)

> vue-cli 的 unit-mocha 插件

## 注入的命令

- **`vue-cli-service test:unit`**

  通过 [mocha-webpack](https://github.com/zinserjan/mocha-webpack) + [chai](http://chaijs.com/) 运行单元测试。

  **注意该测试会运行在带有 JSDOM 浏览器环境模拟的 Node.js 中**

  ```
  使用：vue-cli-service test:unit [options] [...files]

  选项：

    --watch, -w   以监听模式运行
    --grep, -g    只运行匹配 <pattern> 的测试
    --slow, -s    将测试阀值减慢，单位是毫秒
    --timeout, -t 设置超时阀值，单位是毫秒
    --bail, -b    第一个测试失败后仍然保释
    --require, -r 在运行测试之前引入给定的模块
    --include     在测试包中包含给定的模块
  ```

  默认文件匹配规则是：`test/unit` 内所有以 `.spec.(ts|js)` 结尾的文件。

  也支持所有的 [mocha-webpack 命令行选项](http://zinserjan.github.io/mocha-webpack/docs/installation/cli-usage.html)。

## 在已创建的项目中安装

``` sh
vue add @vue/unit-mocha
```

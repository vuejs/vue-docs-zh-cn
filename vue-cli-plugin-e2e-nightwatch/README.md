# @vue/cli-plugin-e2e-nightwatch

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-e2e-nightwatch/README.md)

> vue-cli 的 e2e-nightwatch 插件

## 注入的命令

- **`vue-cli-service test:e2e`**

  通过 [NightwatchJS](https://nightwatchjs.org) 运行 e2e 测试

  选项：

  ```
  --url        通过给定的 URL 运行 e2e 测试，而不是自动开启开发服务器
  --config     使用自定义 nightwatch 配置文件 (覆写内部的)
  -e, --env    指定测试所运行的浏览器环境，多个浏览器用逗号隔开 (默认值：chrome)
  -t, --test   运行指定名字的测试
  -f, --filter 通过文件名 glob 过滤测试
  ```

  > 注意：这个插件当前使用了 Nightwatch v0.9.x。我们会在升级之前等待 Nightwatch 升级到稳定版本。

  额外的，[支持所有的 Nightwatch CLI 选项](https://github.com/nightwatchjs/nightwatch/blob/master/lib/runner/cli/cli.js)。

## 配置

我们已经为 Nightwatch 预置了默认通过 Chrome 运行。如果你希望在额外的浏览器上运行 e2e 测试，你需要在你的项目根目录下添加 `nightwatch.config.js` 或 `nightwatch.json` 来配置额外的浏览器。这个配置会合并到[内部 Nightwatch 配置](https://github.com/vuejs/vue-cli/blob/dev/packages/%40vue/cli-plugin-e2e-nightwatch/nightwatch.config.js).

作为替代方案，你可以通过 `--config` 选项指定一个自定义的配置文件来完全替代内部的配置。

请咨询 Nightwatch 文档了解[配置选项](http://nightwatchjs.org/gettingstarted#settings-file)以及如何[设置浏览器驱动](http://nightwatchjs.org/gettingstarted#browser-drivers-setup).

## 在已创建的项目中安装

``` sh
vue add e2e-nightwatch
```

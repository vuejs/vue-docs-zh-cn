# @vue/cli-plugin-pwa

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-pwa/README.md)

> vue-cli 的 pwa 插件

该插件加入的 service worker 只会在生产环境下 (即只在运行 `npm run build` 或 `yarn build` 时) 开启。在开发环境下开启 service worker 并不推荐，因为它会导致之前的缓存资源被使用而未包含最新的本地改变。

取而代之的是，在开发环境下引入 `noopServiceWorker.js`。这个 service worker 文件会重置之前在相同主机和端口注册过的任何 service worker，有效地达到了 no-op 的目的。

如果你需要本地测试一个 service worker，构建应用并在构建目录运行一个简单的 HTTP 服务器。这里推荐使用浏览器隐私模式以避免浏览器缓存带来的问题。

## 配置

配置是通过 `vue.config.js` 的 `pwa` 属性或 `package.json` 中的 `"pwa"` 字段处理的。

- **pwa.workboxPluginMode**

  这个选项允许你在底层的 [`workbox-webpack-plugin`](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin) 所支持的两种模式之间进行选择。

  - `'GenerateSW'` (默认值)，每次重新构建你的 web app 时都会创建一个新的 service worker 文件。

  - `'InjectManifest'` 允许你以一个现成的 service worker 文件开始，然后创建一份文件拷贝，并把“precache manifest”注入其中。

  这份“[该使用哪个插件？](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin#which_plugin_to_use)”的指南会帮助你在两者之间做出选择。

- **pwa.workboxOptions**

  这些选项会传入底层的 `workbox-webpack-plugin`。

  关于所支持的值的更多的信息，请查阅 
  [`GenerateSW`](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin#full_generatesw_config) 或 [`InjectManifest`](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin#full_injectmanifest_config) 的指南。

- **pwa.name**

  - 默认值：`package.json` 的 "name" 字段

    在生成的 HTML 中用作 `apple-mobile-web-app-title` meta 标签的值。注意你需要编辑 `public/manifest.json` 来配合它。

- **pwa.themeColor**

  - 默认值：`'#4DBA87'`

- **pwa.msTileColor**

  - 默认值：`'#000000'`

- **pwa.appleMobileWebAppCapable**

  - 默认值：`'no'`

    这里的默认值是 `'no'` 因为 iOS 11.3 之前都不支持 PWA。更多详情请查阅[这篇文章](https://medium.com/@firt/dont-use-ios-web-app-meta-tag-irresponsibly-in-your-progressive-web-apps-85d70f4438cb)。

- **pwa.appleMobileWebAppStatusBarStyle**

  - 默认值：`'default'`

- **pwa.assetsVersion**

  - 默认值：`''`

    该选项会为图标和 manifest 文件的 URL 添加 `?v=<pwa.assetsVersion>`。以便在需要的时候应对浏览器缓存。

- **pwa.manifestPath**

  - 默认值：`'manifest.json'`

    应用的 manifest 文件路径。

- **pwa.iconPaths**

  - 默认值：

    ```js
    {
      favicon32: 'img/icons/favicon-32x32.png',
      favicon16: 'img/icons/favicon-16x16.png',
      appleTouchIcon: 'img/icons/apple-touch-icon-152x152.png',
      maskIcon: 'img/icons/safari-pinned-tab.svg',
      msTileImage: 'img/icons/msapplication-icon-144x144.png'
    }
    ```

    改变这些值可以为图标设置不同的路径。

### 配置示例

```js
// Inside vue.config.js
module.exports = {
  // ...其它 vue-cli 插件选项...
  pwa: {
    name: 'My App',
    themeColor: '#4DBA87',
    msTileColor: '#000000',
    appleMobileWebAppCapable: 'yes',
    appleMobileWebAppStatusBarStyle: 'black',

    // 配置 workbox 插件
    workboxPluginMode: 'InjectManifest',
    workboxOptions: {
      // InjectManifest 模式下 swSrc 是必填的。
      swSrc: 'dev/sw.js',
      // ...其它 Workbox 选项...
    }
  }
}
```

## 在已创建的项目中安装

``` sh
vue add @vue/pwa
```

## 注入的 webpack-chain 规则

- `config.plugin('workbox')`

# @vue/cli-plugin-pwa

[英文原版](https://github.com/vuejs/vue-cli/tree/dev/packages/\@vue/cli-plugin-pwa/README.md)

> vue-cli 的 pwa 插件

<!-- todo translation -->
The service worker added with this plugin is only enabled in the production environment (e.g. only if you run `npm run build` or `yarn build`). Enabling service worker in a development mode is not a recommended practice, because it can lead to the situation when previously cached assets are used and the latest local changes are not included.

Instead, in the development mode the `noopServiceWorker.js` is included. This service worker file is effectively a 'no-op' that will reset any previous service worker registered for the same host:port combination.

If you need to test a service worker locally, build the application and run a simple HTTP-server from your build directory. It's recommended to use a browser incognito window to avoid complications with your browser cache.

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

<!-- todo translation -->
- **pwa.assetsVersion**

  - Default: `''`

    This option is used if you need to add a version to your icons and manifest, against browser’s cache. This will append `?v=<pwa.assetsVersion>` to the URLs of the icons and manifest.

- **pwa.manifestPath**

  - Default: `'manifest.json'`

    The path of app’s manifest.

- **pwa.iconPaths**

  - Defaults:

    ```js
    {
      favicon32: 'img/icons/favicon-32x32.png',
      favicon16: 'img/icons/favicon-16x16.png',
      appleTouchIcon: 'img/icons/apple-touch-icon-152x152.png',
      maskIcon: 'img/icons/safari-pinned-tab.svg',
      msTileImage: 'img/icons/msapplication-icon-144x144.png'
    }
    ```

    Change these values to use different paths for your icons.

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

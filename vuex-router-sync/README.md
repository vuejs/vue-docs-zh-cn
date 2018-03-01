# Vuex Router Sync [![CircleCI](https://circleci.com/gh/vuejs/vuex-router-sync.svg?style=svg)](https://circleci.com/gh/vuejs/vuex-router-sync)

[英文原版](https://github.com/vuejs/vuex-router-sync/)

> 将 Vue Router 当前的 `$route` 同步到 Vuex store 作为其 state 的一部分。

### 使用

``` bash
# 最新版只在 vue-router >= 2.0 工作
npm install vuex-router-sync

# 如果想用在 vue-router < 2.0 请使用：
npm install vuex-router-sync@2
```

``` js
import { sync } from 'vuex-router-sync'
import store from './vuex/store' // Vuex store 实例
import router from './router' // Vue Router 实例

const unsync = sync(store, router) // 完成同步，返回一个解除同步的回调函数

// 启动你的应用……

// 在撤下应用/Vue 的时候 (比如你只在你的应用的某一个部分使用了 Vue.js，现在你从这部分内容离开且想要销毁和回收 Vue 的组件和资源)
unsync() // 从 store 解除对路由器的同步
```

你可以可选的设置一个 Vuex 的自定义模块名：

```js
sync(store, router, { moduleName: 'RouteModule' } )
```

### 它是如何工作的？

- 它会在 store 中添加一个 `route` 模块，该模块的 state 体现了当前的路由：

  ``` js
  store.state.route.path   // 当前路径 (string)
  store.state.route.params // 当前参数 (object)
  store.state.route.query  // 当前查询 (object)
  ```

- 当其路由器导航到一个新的路由时，这个 store 的 state 会被更新。

- **`store.state.route` 是不可变的，因为它是从 URL 获取的，这是其实际的来源。**你不应该去尝试通过改变路有对象来切换导航，而应该调用 `$router.push()` 或 `$router.go()`。注意你可以在当前路径执行 `$router.push({ query: {...}})` 来更新查询字符串。

### 协议

[MIT](http://opensource.org/licenses/MIT)

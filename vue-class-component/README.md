# vue-class-component

[英文原版](https://github.com/vuejs/vue-class-component/)

> 为撰写 class 形式的 Vue 组件而实现的 ECMAScript / TypeScript 装饰器 (decorator)。

[![npm](https://img.shields.io/npm/v/vue-class-component.svg)](https://www.npmjs.com/package/vue-class-component)

### 使用

**要求的环境**：[ECMAScript stage 1 的装饰器](https://github.com/wycats/javascript-decorators/blob/master/README.md).
如果你使用 Babel，则需要 [babel-plugin-transform-decorators-legacy](https://github.com/loganfsmyth/babel-plugin-transform-decorators-legacy)。
如果你使用 TypeScript，请开启 `--experimentalDecorators` 开关。

> 目前还不支持 stage 2 的装饰器，因为主流的语法转换器还是会将内容转换为老版本的装饰器。

注意事项：

1. `methods` 可以直接声明为 class 成员。

2. 计算属性可以声明为 class 属性访问器。

3. 初始的 `data` 可以声明为 class 属性 (如果你使用 Babel，则需要 [babel-plugin-transform-class-properties](https://babeljs.io/docs/plugins/transform-class-properties/))。

4. `data`、`render` 以及所有 Vue 的生命周期钩子也可以直接声明为 class 成员方法，但是你无法在其所在实例中直接调用它们。在你声明自定义方法的时候也应该回避这些保留的名字。

5. 其它选项可以传入修饰器函数。

### 示例

接下来的例子是在 Babel 中写的。TypeScript 的版本[在项目的 `example` 目录中](https://github.com/vuejs/vue-class-component/blob/master/example/App.vue)。

``` vue
<template>
  <div>
    <input v-model="msg">
    <p>prop: {{propMessage}}</p>
    <p>msg: {{msg}}</p>
    <p>helloMsg: {{helloMsg}}</p>
    <p>computed msg: {{computedMsg}}</p>
    <button @click="greet">Greet</button>
  </div>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component({
  props: {
    propMessage: String
  }
})
export default class App extends Vue {
  // initial data
  msg = 123

  // use prop values for initial data
  helloMsg = 'Hello, ' + this.propMessage

  // lifecycle hook
  mounted () {
    this.greet()
  }

  // computed
  get computedMsg () {
    return 'computed ' + this.msg
  }

  // method
  greet () {
    alert('greeting: ' + this.msg)
  }
}
</script>
```

另外 [vue-property-decorators](https://github.com/kaorun343/vue-property-decorator) 提供了 `@prop` 和 `@watch` 装饰器，也许你也会感兴趣。

### 使用混入 (Mixin)

vue-class-component 提供了 `mixins` 辅助函数，能够以 class 的形式使用[混入](https://cn.vuejs.org/v2/guide/mixins.html)。通过 `mixins` 辅助行数，TypeScript 可以推导出混入的类型并继承到组件类型上。

声明一个混入的例子：

``` js
// mixin.js
import Vue from 'vue'
import Component from 'vue-class-component'

// 你可以像声明一个组件一样声明混入。
@Component
export class MyMixin extends Vue {
  mixinValue = 'Hello'
}
```

使用一个混入的例子：

``` js
import Component, { mixins } from 'vue-class-component'
import MyMixin from './mixin.js'

// 使用 `mixins` 辅助函数代替 `Vue`。
// `mixins` 可以接收任何数量的参数。
@Component
export class MyComp extends mixins(MyMixin) {
  created () {
    console.log(this.mixinValue) // -> Hello
  }
}
```

### 创建自定义装饰器

你可以通过创建你自己的装饰器扩展这个库的功能。vue-class-component 提供了用来创建自定义修饰器的 `createDecorator` 辅助函数。`createDecorator` 接收一个回调函数作为其第一个参数，而该回调函数将会接收以下参数：

- `options`：Vue 组件选项对象。对这个对象的改动讲影响到提供的组件。
- `key`：该修饰符应用的属性或方法的键名。
- `parameterIndex`：被装饰的参数的索引值，如果该自定义装饰器作为一个参数被使用时。

创建 `NoCache` 装饰器的示例：

``` js
// decorators.js
import { createDecorator } from 'vue-class-component'

export const NoCache = createDecorator((options, key) => {
  // 组件选项应该传递给回调函数
  // 且对选项对象的更新而影响到该组建
  options.computed[key].cache = false
})
```

``` js
import { NoCache } from './decorators'

@Component
class MyComp extends Vue {
  // 这个计算属性不会被缓存
  @NoCache
  get random () {
    return Math.random()
  }
}
```

### 添加自定义钩子

如果你使用了相同的 Vue 组件，比如 Vue Router，你可能希望 class 形式的组件能够解析它们提供的钩子。在这种情况下，`Component.registerHooks` 允许你注册每个钩子：

```js
// class-component-hooks.js
import Component from 'vue-class-component'

// 以他们的名字注册路由器钩子
Component.registerHooks([
  'beforeRouteEnter',
  'beforeRouteLeave',
  'beforeRouteUpdate' // for vue-router 2.2+
])
```

```js
// MyComp.js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
class MyComp extends Vue {
  // 这个 class 组件现在处理了 beforeRouteEnter
  // 和 beforeRouteLeave 作为 Vue Router 的钩子
  beforeRouteEnter () {
    console.log('beforeRouteEnter')
  }

  beforeRouteLeave () {
    console.log('beforeRouteLeave')
  }
}
```

注意你需要在组件定义完成之前注册钩子。

```js
// main.js

// 确认在导入任何组件之前进行注册
import './class-component-hooks'

import Vue from 'vue'
import MyComp from './MyComp'

new Vue({
  el: '#app',
  components: {
    MyComp
  }
})
```

### Class 属性的注意事项

vue-class-component 会通过在底层将原本构造函数实例化来收集 class 的属性作为 Vue 示例数据。当我们可以像原生 class 一样定义实例数据时，我们有时需要知道它是如何工作的。

#### 属性里的 `this` 的值

如果你定义了一个箭头函数作为一个 class 的属性并在其中访问 `this`，它将不会正常工作。这是因为当初始化 class 属性的时候，`this` 只是一个 Vue 实例的代理对象：

```js
@Component
class MyComp extends Vue {
  foo = 123

  bar = () => {
    // 不会如预期更新属性。
    // `this` 的值不是真正的 Vue 实例。
    this.foo = 456
  }
}
```

在那种情况下，你可以简单的定义一个方法，而不是一个 class 属性，因为 Vue 会自动绑定该实例：

```js
@Component
class MyComp extends Vue {
  foo = 123

  bar () {
    // Correctly update the expected property.
    this.foo = 456
  }
}
```

#### `undefined` 将不会被响应

为了让 Babel 和 TypeScript 的装饰器行为保持一致，如果一个属性的初始值是 `undefined`，那么 vue-class-component 不会使其产生响应性。你应该用 `null` 作为初始值替而代之，或使用 `data` 钩子来初始化 `undefined` 的属性。

```js
@Component
class MyComp extends Vue {
  // 不会有响应性
  foo = undefined

  // 会有响应性
  bar = null

  data () {
    return {
      // 会有响应性
      baz: undefined
    }
  }
}
```

### 构建该示例

``` bash
$ npm install && npm run example
```

### 答疑

如有相关问题请移步[官方论坛](http://forum.vuejs.org)或[社区聊天室](https://chat.vuejs.org/)。仓库本身的 issue 列表**仅仅**会受理 bug 报告和新特性请求。

### 证书

[MIT](http://opensource.org/licenses/MIT)

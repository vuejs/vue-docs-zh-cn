# @vue/web-component-wrapper [![CircleCI](https://circleci.com/gh/vuejs/vue-web-component-wrapper.svg?style=shield)](https://circleci.com/gh/vuejs/vue-web-component-wrapper)

> 将一个 Vue 组件包裹并注册为一个自定义元素。

## 兼容性

**[需要 ES2015 classes](https://caniuse.com/es6-class)。不支持 IE11 及其以下版本。**

- **如果目标浏览器原生支持 ES2015 但不原生支持 Web Components：**

  你还需要 [Shady DOM + 自定义元素的 polyfill](https://github.com/webcomponents/webcomponentsjs/blob/master/webcomponents-sd-ce.js).

  请到 caniuse.com 查阅 [Custom Elements v1](https://caniuse.com/#feat=custom-elementsv1) 和 [Shadow DOM v1](https://caniuse.com/#feat=shadowdomv1) 的支持情况。

- **注意在使用 Shady DOM polyfill 封装 CSS 时**

  如果你打算使用 Shady DOM polyfill，我们推荐在你的 `*.vue` 文件中使用 [CSS Modules](https://vue-loader.vuejs.org/en/features/css-modules.html) 而不是 `<style scoped>`，因为它不会提供像 Shadow DOM 一样真正的样式封装，所以如果不使用哈希后的 class 名，外部的样式表可能会影响到你的组件。

- **如果目标浏览器不支持 ES2015：**

  你可能得重新考虑一下了，因为在这种情况下，最好不要使用 Web Components。

## 使用

- **`dist/vue-wc-wrapper.js`**：这个文件是 ES modules 格式的。是包的默认导出，可以在浏览器中以 `<script type="module">` 的方式使用。

- **`dist/vue-wc-wrapper.global.js`**：这是在还不支持 `<script type="module">` 的浏览器中使用的旧版本的 `<script>` (暴露全局变量 `wrapVueWebComponent`)。

``` js
import Vue from 'vue'
import wrap from '@vue/web-component-wrapper'

const Component = {
  // 任何组件选项
}

const CustomElement = wrap(Vue, Component)

window.customElements.define('my-element', CustomElement)
```

它可以和异步组件一起工作——你可以传递一个返回 Promise 的异步组件工程函数，这个函数只会在页面上第一个该自定义元素实例被创建的时候调用。

``` js
const CustomElement = wrap(Vue, () => import(`MyComponent.vue`))

window.customElements.define('my-element', CustomElement)
```

## 接口代理细节

### 属性

- 该 Vue 组件中所有的 `props` 声明会作为其属性暴露在自定义元素上。Kebab-case 的 prop 会转换为 camelCase 属性，与其在 Vue 内的转换方式相似。

- 在自定义元素上设置属性 (property) 会将专递给内部 Vue 组件相应的 prop 更新。

- 在自定义元素上设置特性 (attribute) 会更新相应声明的 prop。特性会匹配为 kebab-case。例如，一个名为 `someProp` 的 prop 会拥有一个相应的特性名 `some-prop`。

- 以 `type: Boolean` 声明的 prop 相匹配的特性，会以下列规则自动转换为布尔值：

  - `""` 或和特性名相同的值：-> `true`

  - `"true"` -> `true`

  - `"false"` -> `false`

- 以 `type: Number` 声明的 prop 相匹配的特性，如果其值可以解析为数字，则会自动转换为数字。

### 事件

在内部 Vue 组件触发的自定义事件会 `CustomEvent` 以发送到自定义元素。额外传入 `$emit` 的参数会暴露为一个 `event.detail` 数组。

### 插槽

插槽会同你预期的方式工作，包括具名插槽。它们同样会在被改变的时候更新 (通过 `MutationObserver`)。

但是我们不支持作用域插槽，因为它是 Vue 特有的概念。

### 生命周期

当我们从文档中移除该自定义元素的时候，Vue 组件表现为像在一个 `<keep-alive>` 中同时其 `deactivated` 钩子会被调用。当它被重新插入回文档时，其 `activated` 钩子会被调用。

如果你希望销毁内部的文档，你需要显性的执行：

``` js
myElement.vueComponent.$destroy()
```

## 致谢

特别感谢 @karol-f 早些时候为 [vue-custom-element](https://github.com/karol-f/vue-custom-element) 所做的工作！

## 协议

MIT

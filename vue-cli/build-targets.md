## 构建目标

### 应用

应用模式是默认的模式。在这个模式中：

- `index.html` 会带有注入的资源和资源暗示
- 供应的库会被分到一个独立包以便更好的缓存
- 小于 10kb 的静态资源会被内联在 JavaScript 中
- `public` 中的静态资源会被复制到输出目录中

### 库

你可以通过下面的命令对一个单独的入口构建为一个库：

```
vue-cli-service build --target lib --name myLib [entry]
```

```
File                     Size                     Gzipped

dist/myLib.umd.min.js    13.28 kb                 8.42 kb
dist/myLib.umd.js        20.95 kb                 10.22 kb
dist/myLib.common.js     20.57 kb                 10.09 kb
dist/myLib.css           0.33 kb                  0.23 kb
```

这个入口可以是一个 `.js` 或一个 `.vue` 文件。如果没有指定入口，则会使用 `src/App.vue`。

构建一个库会输出：

- `dist/myLib.common.js`：一个用来被打包器消费的 CommonJS 包 (不幸的是，webpack 目前还并没有支持 ES modules 输出格式的包)

- `dist/myLib.umd.js`：一个用来直接在浏览器中或通过 AMD loader 消费的 UMD 包

- `dist/myLib.umd.min.js`：压缩后的 UMD 构建版本

- `dist/myLib.css`：提取出来的 CSS 文件 (可以通过在 `vue.config.js` 中设置 `css: { extract: false }` 强制内联)

**在库模式中，Vue 是外置的。**这意味着包中不会有 Vue，甚至你在代码中导入了 Vue。如果这个库会通过一个打包器使用，它将会在打包器内尝试作为一个依赖加载 Vue；否则就会回退到一个全局的 `Vue` 变量。

### Web Component

> [兼容性](https://github.com/vuejs/vue-web-component-wrapper#compatibility)

你可以通过下面的命令对一个单独的入口构建为一个库：

```
vue-cli-service build --target wc --name my-element [entry]
```

This will produce a single JavaScript file (and its minified version) with everything inlined. The script, when included on a page, registers the `<my-element>` custom element, which wraps the target Vue component using `@vue/web-component-wrapper`. The wrapper automatically proxies properties, attributes, events and slots. See the [docs for `@vue/web-component-wrapper`](https://github.com/vuejs/vue-web-component-wrapper) for more details.

**Note the bundle relies on `Vue` being globally available on the page.**

This mode allows consumers of your component to use the Vue component as a normal DOM element:

``` html
<script src="https://unpkg.com/vue"></script>
<script src="path/to/my-element.js"></script>

<!-- use in plain HTML, or in any other framework -->
<my-element></my-element>
```

#### Bundle that Registers Multiple Web Components

When building a web component bundle, you can also target multiple components by using a glob as entry:

```
vue-cli-service build --target wc --name foo 'src/components/*.vue'
```

When building multiple web components, `--name` will be used as the prefix and the custom element name will be inferred from the component filename. For example, with `--name foo` and a component named `HelloWorld.vue`, the resulting custom element will be registered as `<foo-hello-world>`.

### Async Web Component

> [Compatibility](https://github.com/vuejs/vue-web-component-wrapper#compatibility)

When targeting multiple web components, the bundle may become quite large, and the user may only use a few of the components your bundle registers. The async web component mode produces a code-split bundle with a small entry file that provides the shared runtime between all the components, and registers all the custom elements upfront. The actual implementation of a component is then fetched on-demand only when an instance of the corresponding custom element is used on the page:

```
vue-cli-service build --target wc-async --name foo 'src/components/*.vue'
```

```
File                Size                        Gzipped

dist/foo.0.min.js    12.80 kb                    8.09 kb
dist/foo.min.js      7.45 kb                     3.17 kb
dist/foo.1.min.js    2.91 kb                     1.02 kb
dist/foo.js          22.51 kb                    6.67 kb
dist/foo.0.js        17.27 kb                    8.83 kb
dist/foo.1.js        5.24 kb                     1.64 kb
```

Now on the page, the user only needs to include Vue and the entry file:

``` html
<script src="https://unpkg.com/vue"></script>
<script src="path/to/foo.min.js"></script>

<!-- foo-one's implementation chunk is auto fetched when it's used -->
<foo-one></foo-one>
```

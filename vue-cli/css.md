## CSS

- [PostCSS](#postcss)
- [CSS Modules](#css-modules)
- [预处理器](#预处理器)
- [向预处理器 Loader 传递选项](#向预处理器-loader-传递选项)

### PostCSS

Vue CLI 内部使用了 PostCSS，并默认开启了 [autoprefixer](https://github.com/postcss/autoprefixer)。你可以通过 `.postcssrc` 或任何 [postcss-load-config](https://github.com/michael-ciniawsky/postcss-load-config) 支持的配置源来配置 PostCSS。

### CSS Modules

你可以通过 `<style module>` 以开箱即用的方式[在 `*.vue` 文件中使用 CSS Modules](https://vue-loader.vuejs.org/zh-cn/features/css-modules.html)。

任何名字以 `.module.(css|sass|scss|less|styl|stylus)` 结尾的文件都会作为独立的样式文件被 CSS Modules 处理。

如果你希望能够不带 `.module` 后缀使用 CSS Modules，你可以在 `vue.config.js` 中设置 `css: { modules: true }`。该选项不会影响 `*.vue` 文件。

如果你想要自定义 CSS Modules 类名的输出格式，你可以在 `vue.config.js` 中设置 `css: { localIdentName: [name]__[local]--[hash:base64:5]}` 这样的选项。

### 预处理器

你可以在创建项目的时候选择预处理器 (Sass/Less/Stylus)。如果当时没有选好，你也可以手动安装相应的 webpack loader。这些 loader 会被预配置好且被自动采集。例如，想要在一个已有的项目中添加 Sass，只需运行：

``` sh
npm install -D sass-loader node-sass
```

然后你就可以导入 `.scss` 文件，或在 `*.vue` 文件中这样使用它：

``` vue
<style lang="scss">
$color = red;
</style>
```

### 向预处理器 Loader 传递选项

有的时候你想要向 webpack 的预处理器 loader 传递选项。你可以使用 `vue.config.js` 中的 `css.loaderOptions` 选项。比如你可以这样向所有 Sass 样式传入共享的全局变量：

``` js
// vue.config.js
const fs = require('fs')

module.exports = {
  css: {
    loaderOptions: {
      sass: {
        data: fs.readFileSync('src/variables.scss', 'utf-8')
      }
    }
  }
}
```

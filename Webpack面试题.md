# Webpack 面试题

下面的题目都摘自各种 webpack 面试相关的文章，我只选择了感觉很重要的一些题，想要了解更多，可以查看本文最后的参考链接。

## webpack 有什么用

- 静态资源的合并/打包/压缩
- 解决加载慢，错综复杂的依赖关系

## 有哪些常见 loader 和 plugin，你用过哪些

loader：

- css：postcss-l+autoprefixer，style-l(js到<style>)，css-l(js中 import 的 css)，less-l，sass-l
- 图片/字体加载：url-loader，file-loader
- 图片压缩：image-webpack-loader
- js：babel-loader es6转es5，eslint-loader 检查js代码

plugin：

- webpack-dev-server 实时打包构建
- html-webpack-plugin 配置启动页
- clean-webpack-plugin 清理 dist 文件夹
- mini-css-extract-plugin 提取css为单独文件
- optimize-css-assets-webpack-plugin 压缩css

## loader 和 plugin 的区别是什么

loaders 是加载器。用来处理源文件的（JSX，Scss，Less..），一次处理一个，让 webpack 有能力加载处理非js文件，plugin 是插件。并不直接操作单个文件，它直接对整个构建过程起作用。

## 如何按需加载代码

import('文件路径').then【代码执行到时再加载】

## 如何提高 webpack 构建速度

- 用 webpack4
- 用 externals 提取常用库
- 提前打包第三方库：dllplugin
- happypack：多线程进行打包，提升 loader 解析速度
- webpack-parallel-uglify-plugin 并发压缩js，提升速度

## 如何利用 webpack 提升前端性能

1. 动态加载：路由懒加载【import()动态加载模块，返回结果是Promise】

    ```js
    function loadView(view) {
        return () => import( /* webpackChunkName: "view-[request]" */ `../components/${view}.vue`)
    }
    { path: '/home', component: loadView('Home') }
    ```

1. 第三方依赖用 cdn（vue，vue-router，element-ui）【webpack.config.js externals】
2. 压缩文件【uglifyjs-plugin/optimize-css-assets-plugin/image-webpack-loader】
3. 提取公共代码【CommonChunkPlugin / splitChunksPlugin】

## 参考

- [关于webpack的面试题总结](https://zhuanlan.zhihu.com/p/44438844)
- [使用webpack4提升180%编译速度](http://louiszhai.github.io/2019/01/04/webpack4/)
- [webpack4.0 各个击破](https://www.cnblogs.com/dashnowords/p/9545482.html)
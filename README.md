# front-end-interview（持续更新中...）

> 温馨提示：个人喜欢在点击链接的时候能跳转到新标签页，然而 github 貌似不支持？只能本页跳转。所以有跟我一样习惯的，就得按一下 CTRL + 点击链接实现。

## 介绍

根据网上各大前端面试题文章以及自我总结，沉淀下来的一套面试题合集，包含前端知识点 + 面试题。

为了更好的阅读体验，推荐各位看官在博客中查看：[Lance个人博客-面试系列](https://evestorm.github.io/posts/46036/)

## 大纲

### 1 HTML

根据以往面试经验，HTML部分很少有问到。所以这里仅列举一旦考则必问的面试题

#### 1.1 面试题

- [HTML面试题](./HTML面试题.md)

### 2 CSS

个人经验：这部分面试题一般笔试考知识点多一些（多背），面试考布局更多一些（多实践）。所以CSS我拆成了两部分，把布局单独拧出来了。

#### 2.1 知识点 + 部分面试题

- [CSS知识点](./CSS.md)

#### 2.2 布局

- [CSS布局面试题](./CSS布局.md)

### 3 JavaScript

这部分是面试重中之重，基本上啥都可能考。譬如比较经典的一个有关axios的面试题：

- 问：a、b、c三个请求，希望c在a、b获取数据后再发请求，要是你你会怎么做？
- 答：axios.all 先请求 a、b，再在 then 的第一个回调中请求 c ，巴拉巴拉...
- 问：那 axios.all 内部是通过什么实现的呢？
- 答：Promise.all，巴拉巴拉...
- 问：如果不用 Promise 该如何实现？
- 答：可以用 高阶函数 ，巴拉巴拉...

```js
// 示例（使用node读取文件做场景来说明）
let fs = require('fs')
let arr = []
function fn(data) {
  arr.push(data)
  if (arr.length === 2) {
    // get ab
    // todo c
  }
}
fs.readFile('./a.txt', 'utf8', (err, data) => {
  fn(data)
})
fs.readFile('./b.txt', 'utf8', (err, data) => {
  fn(data)
})
```

可以看出，往往 JS 面试题是层层深入的，需要你有坚实的基础，并且在平常开发时不仅满足会使用各种库和框架，还要深入了解原理。所以这一小节内容较多，各位做好抗压准备。

#### 3.1 知识点

- [JS知识点](./JavaScript知识点.md)

#### 3.2 面试题

- [JS笔试题](./JS笔试题.md)
- [JS面试题](./JS面试题.md)

### 4 浏览器/网络

浏览器这边主要考察兼容性，当然有时候也考察下HTTP缓存等内容，所以我把它们归到了一起方便复习。

- [浏览器](./浏览器.md)

### 5 框架 + 工具

#### 5.1 Vue

Vue没什么好说的，数据响应式（双向数据绑定）是一定会问到的，其它例如生命周期之类的也常常出现，具体查看下面链接：

- [Vue面试题](./Vue面试题.md)

#### 5.2 Webpack

个人在面试中很少被问到 webpack 的问题？唯一一次与 webpack 相关的还是在一次面试的笔试题中，有这样一道：

> 写出你知道的性能优化方案（不要写 webpack 工具能做到的）

虽然我自己没碰到过，但保不齐其他公司不爱问，所以还是放在这儿：

- [如何使用webpack4](https://evestorm.github.io/posts/47462/)
- [Webpack面试题](./Webpack面试题.md)

### 6 性能优化

性能优化现在应该是必考题了，我去几家公司面就有几家会问到，常考的知识点有「首屏优化」「重排重绘优化」以及「css3动画优化」等。不太了解这部分的童鞋可以看下面链接：

- [性能优化知识点](./性能优化知识点.md)
- [web性能优化-实践](https://evestorm.github.io/posts/47143/)

### 7 算法

这部分目前是我薄弱项，暂时不能给到大家更多帮助。不过对于面一般普通前端来说，也就顶多问一下常见的排序算法了。

这是我之前整理的常见排序算法：可以 [点击此处](https://evestorm.github.io/posts/59937/) 查看。

其他算法面试题会在后续更新...

## 简历模板

网上的简历模板一搜一大堆，不过大都既不实用也不好看。所以最后我再分享几个觉得不错的模板给大家参考：

### 纸质模板

- https://www.resumeviking.com/wp-content/uploads/2018/01/Dwight-Kavanagh-Resume-IT-QA-Analyst-11.pdf
  ![resume0](https://gitee.com/evestorm/various_resources/raw/master/%E7%AE%80%E5%8E%86/resume0.png)
- https://www.resumeviking.com/wp-content/uploads/2019/04/Emily_Carter_-_Resume_-_English_Teacher-9.pdf
  ![resume1](https://gitee.com/evestorm/various_resources/raw/master/%E7%AE%80%E5%8E%86/resume1.png)
- https://www.resumeviking.com/wp-content/uploads/2018/09/Karen_Philips_-_Resume_-_Web_Designer-4.pdf
  ![resume2](https://gitee.com/evestorm/various_resources/raw/master/%E7%AE%80%E5%8E%86/resume2.png)

上面两份模板都来自 [此网站](https://www.resumeviking.com/templates/) ，想寻找更满意的版本可以进去逛逛。

### 在线模板

- http://zhangwenli.com/cv/
  - 下载地址：https://github.com/Ovilia/cv
- https://html5up.net/read-only
  - 下载地址：上方链接右上角 Download

## 更多

本项目整合了大量下方资源的面试题内容，但毕竟是按照我自己技术栈整合的，所以如果你还想查缺补漏（例如 react 相关面试题等），可以点击下方链接了解更多：

- [前端开发面试题](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)
- [前端面试考点多？看这些文章就够了](https://juejin.im/post/5aae076d6fb9a028cc6100a9)
- [KieSun/Front-end-knowledge](https://github.com/KieSun/Front-end-knowledge)
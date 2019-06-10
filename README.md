# front-end-interview

## 介绍

根据网上各大前端面试题文章，自己总结出来的一套面试宝典，包含前端知识点+面试题。

为了更好的阅读体验，推荐各位看官在博客中查看：<a href="https://evestorm.github.io/tags/%E9%9D%A2%E8%AF%95/" target="_blank">Lance个人博客-面试系列</a>

## 大纲

### 1 HTML

根据以往面试经验，HTML部分很少有问到。所以这里仅列举一旦考则必问的面试题

#### 1.1 面试题

- <a href="./HTML面试题.md" target="_blank">HTML面试题</a>

### 2 CSS

个人经验：这部分面试题一般笔试考知识点多一些（多背），面试考布局更多一些（多实践）。所以CSS我拆成了两部分，把布局单独拧出来了。

#### 2.1 知识点+部分面试题

- <a href="./CSS.md" target="_blank">CSS知识点</a>

#### 2.2 布局

- <a href="./CSS布局.md" target="_blank">CSS布局面试题</a>

### 3 JavaScript

这部分是面试重中之重，基本上啥都可能考。譬如最经典的一个面试题：

问：a、b、c三个请求，希望c在a、b获取数据后再发请求，要是你你会怎么做？
答：axios.all 先请求 a、b，再在 then 的 resolve 回调中请求 c，巴拉巴拉...
问：那 axios.all 内部是通过什么实现的呢？
答：promise.all，巴拉巴拉...
问：如果不用 promise 该如何实现？
答：可以用 setTimeout，巴拉巴拉...
问：...

可以看出，往往 JS 面试题是层层深入的，需要你有坚实的基础，并且在平常开发时不仅满足会使用各种库和框架，还要深入了解原理。所以这一小节内容较多，各位做好抗压准备。

#### 3.1 知识点

- <a href="./JavaScript知识点.md" target="_blank">JavaScript知识点</a>

#### 3.2 面试题

- <a href="./JavaScript面试题.md" target="_blank">JavaScript面试题</a>

### 4 浏览器

- <a href="./浏览器.md" target="_blank">浏览器相关面试题</a>

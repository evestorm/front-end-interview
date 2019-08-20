# CSS知识点

## 盒模型

标准盒模型（W3C标准）：

一个块的总宽度 = **内容宽度** + margin(左右) + padding(左右) + border(左右)

怪异盒模型（IE标准）：

一个块的总宽度 = **width（包含 padding 和 border）** + margin(左右)

### 怪异盒模型触发条件

如果 html 文件最顶部 doctype 缺失，则在 ie6、7、8 将会触发怪异模式(quirks);

### 通过css来设置盒模型

```css
box-sizing: content-box; /* 标准模型 */
            border-box;  /* IE模型 */
            inherit;     /* 从父元素继承 */
```

### 应用场景

- 怪异盒模型（border-box）

> 比如我们想做一个内边距 10px ，边框为 2px ，最终包括边框宽度为  100px 的 div ，之前的做法是先算出内容宽 `width` 。但有点繁琐，需要人算。此时我们可以把 div 的 box-sizing 设置为 border-box ，我们就可以直接把 width 设置为 100px ，其余的 padding 和 border 按照给定好的值一一填入，就可以完成这一切工作，省去了人为的计算内容宽 content 的过程，减少计算量的同时减少了错误率。

## CSS选择符有哪些？哪些属性可以继承？

1. id选择器（ #my-id ）
2. 类选择器（ .my-class-name ）
3. 标签选择器（ div, h1, p ）
4. 相邻选择器（ h1 + p 紧贴在 h1 后面的第一个 p 标签。同级标签，之间不能有其他标签 ）
5. 子代选择器（ ul > li  ul 标签下的下一个层级的 li ，并不是所有 ）
6. 后代选择器（ li a ）
7. 通配符选择器（ * ）
8. 属性选择器（ a[rel = "external"] ）
9. 伪类选择器（ a:hover, li:nth-child ）

### 可继承属性

font-size font-family color, UL LI DL DD DT;

### 不可继承属性

border padding margin width height

## CSS优先级算法如何计算？

- 写在后面的覆盖写在前面的
- 描述越具体优先级越高 (div , div[class='restart'])
- 同权重 行内样式 > 内嵌样式 > 外部样式
- !important 优先级最高

## CSS3新增伪类有那些？

- `p:first-of-type` 属于其父元素的特定类型首个子元素 等同于 `:nth-of-type(1)`
- `p:last-of-type` 属于其父元素的特定类型的最后一个子元素  等同于 `:nth-last-of-type(1)`

- `::after` 在元素之前添加内容,也可以用来做清除浮动。
- `::before` 在元素之后添加内容
- `:enabled/:disabled` 控制表单控件的禁用状态。
- `:checked` 单选框或复选框被选中。

## display:none、visibility：hidden和opacity: 0的区别？

- display：none （不占空间，不能点击）（回流+重绘）
- visibility：hidden （占据空间，不可点击）（重绘）
- opacity: 0（占据空间，可以点击）（重建图层，性能较高）

更多：[分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/100)

## position 跟 display 、float 这些特性相互叠加后会怎么样？

- position 属性规定元素的定位类型
- display 属性规定元素应该生成的框的类型
- float 属性是一种布局方式，定义元素在哪个方向浮动。

优先级机制：position：absolute/fixed 优先级最高，有他们在时，float 不起作用，display 值需要调整。float 或者 absolute 定位的元素，只能是块元素或表格。

## 讲一讲 position 属性中的各种值

| **值**         | **是否脱标占有位置** | **可使用边偏移** | **描述**                                                     |
| -------------- | -------------------- | ---------------- | ------------------------------------------------------------ |
| static         | 不脱标，正常模式     | 不可以           | 自动（没有）定位（默认定位方式）                             |
| relative(自恋) | 不脱标，占有位置     | 可以             | 相对定位，相对于其原文档流的位置进行定位                     |
| absolute(拼爹) | 完全脱标，不占位置   | 可以             | 绝对定位，相对于其上一个已经定位（不为static）的父元素进行定位 |
| fixed(浏览器)  | 完全脱标，不占位置   | 可以             | 固定定位，相对于浏览器窗口进行定位（老IE不支持）             |
| inherit        |                      |                  | 规定从父元素继承 position 属性的值                           |

## CSS3 有哪些新特性？

### 过渡

```css
/* 语法 */
transition: CSS属性，过渡时间，效果曲线（默认ease），延迟时间（默认0）

/* demo1 */
/*宽度从原始值到制定值的一个过渡，运动曲线ease,运动时间0.5秒，0.2秒后执行过渡*/
transition: width .5s ease .2s

/* demo2 */
/* 分开写 */
transition-property: width;
transition-duration: 1s;
transition-timing-function: linear;
transition-delay: 2s;
```

### 动画

```css
/* 语法 */
animation：动画名称，动画时间，运动曲线（默认ease），动画延迟（默认0），播放次数（默认1），
是否反向播放动画（默认normal），是否暂停动画（默认running）

/* demo1 */
/* 执行一次logo2-line动画，运动时间2秒，运动曲线为 linear */
animation: logo2-line 2s linear;

/* demo2 */
/* 2秒后开始执行一次 logo2-line 动画，运动时间2秒，运动曲线为 linear */
animation: logo2-line 2s linear 2s;

/* demo3 */
/*无限执行 logo2-line 动画，每次运动时间2秒，运动曲线为 linear，并且执行反向动画*/
animation: logo2-line 2s linear alternate infinite;

还有一个重要属性：用来指定在动画执行之前和之后如何给动画的目标应用样式。
animation-fill-mode: none | forwards | backwards | both;
/*
动画分为 初始状态 等待期 动画执行期 完成期 四个阶段。

- none 表示 等待期和完成期，元素样式都为初始状态样式，不受动画定义（@keyframes）的影响
- forwards 表示等待期保持初始样式，完成期间保持最后一帧样式
- backwards 表示等待期为第一帧样式，完成期跳转为初始样式
- both 表示 等待期样式为第一帧样式，完成期保持最后一帧样式
*/  
```

参考：[如何理解animation-fill-mode及其使用？](https://segmentfault.com/q/1010000003867335)

### 形状转换

transform: 适用于2D或3D转换的元素 [(3d看这里)](https://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)
transform-origin: 转换元素的位置（围绕那个点进行转换,默认中心）。默认 x, y, z ： 50%, 50%, 0 对应 left, top

- 缩放 scale
- 位移 translate
- 旋转 rotate
- 倾斜 skew

```css
transform: rotate(30deg);
transform: translate(30px, 30px);
transform: scale(.8);
transform: skew(10deg, 10deg);
transform-origin: left top; /* 左上（默认中心）【改变元素变形的原点】 */
```


#### 资源

对 transition 、animation 和 transform 这块儿内容不熟的同学可以参考我的博客简单上手教程，基本上每个属性都有相应的 demo 展示：[Lance的个人博客-css分类](https://evestorm.github.io/categories/%E5%89%8D%E7%AB%AF/CSS/)

另外，上面的知识点参考于下方资源，可自取：

- [个人总结（css3新特性）](https://segmentfault.com/a/1190000010780991)
- [cubic-bezier贝塞尔在线预览](http://cubic-bezier.com/#.42,0,.58,1)
- [transition-timing-function | cssreference](https://cssreference.io/property/transition-timing-function/)
- [Understanding The CSS3 transition-timing-function Property](https://www.smashingmagazine.com/2014/04/understanding-css-timing-functions/)
- [翻译 | 深入理解CSS时序函数](https://segmentfault.com/a/1190000011019534)
- [css3动画详解【推荐】](http://koala.ink/posts/86be452a/)

### 选择器

#### 属性选择器

- div[index] { background: red }    表示 div 下面带 index 属性的背景为红色
- div[index=a] { background: red }  表示 div 下面的自定义属性 index=a 的 div 的颜色变成红色。
- div[index~=a] { background: red }    只要 div 中 index 属性带 a 这个单词的就生效。[a这个单词必须独立存在]
    - p[class~='lance'] 只会匹配到 class="xxx lance"，而不会匹配 class="xxlancexx"
- div[index*=a] { background: red }     只要 div 里面的 index 属性带 a 字母的就生效。
- div[index^=a] { background: red }   以 a 开头的
- div[index$=a] { background: red }    以 a 结尾
- div[index|=a] { background:red }    只要以 a- 开头的

当需要多个的时候： div[key1=value1][key2=value2][key3=value3].... { background:red }

#### 结构性伪类选择器

前提：假如 div 是下面 p 标签的父元素

- `p:nth-child(n)` div 下类型为 p 的第 n 个子元素，下标从 1 开始！！
    - `:nth-child(odd)` 奇数
    - `:nth-child(even)` 偶数
- `p:nth-last-child(n)` 从倒数，div 下类型为 p 的第 n 个子元素
- `p:last-child` div 下最后一个子元素如果是 p ，则生效，否则不生效
- `p:first-child` div 下第一个子元素如果是 p ，则生效，否则不生效【css2的】
- `p:first-of-type` div 下第一个类型为 p 的子元素【不需要 p 是这个 div 下的第一个子元素】
- `p:last-of-type` div 下最后一个类型为 p 的子元素

参考：[css选择器中:first-child与:first-of-type的区别](https://www.cnblogs.com/2050/p/3569509.html)

#### 伪元素选择器

1. `::first-letter`  文本的第一个单词或字（如中文、日文、韩文等）
2. `::first-line`    文本第一行；
3. `::selection`    可改变选中文本的样式；
4. `::before` 和 `::after` 在元素内部的开始火结束为止创建一个元素，该元素为行内元素，必须结合 content 属性使用 div::before { content: '开始' }

#### 目标伪类选择器

`:target` 目标伪类选择器， `:` 选择器可用于选取当前活动的目标元素

```css
:target {
    color: red;
    font-size: 30px;
}
```

### 阴影

```css
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
```

### 边框

- 边框图片 border-image

    ```css
    border-image: image-source image-height image-width image-repeat
    ```

- 圆角边框 border-radius

    ```css
    border-radius: 左上角，右上角，右下角，左下角
    ```

### CSS弹性布局（flex）

#### Flex容器属性

```css
主轴方向：水平排列（默认） | 水平反向排列 | 垂直排列 | 垂直反向排列
flex-direction: row | row-reverse | column | column-reverse;

换行：不换行（默认） | 换行 | 反向换行(第一行在最后面)
flex-wrap: nowrap | wrap | wrap-reverse;

flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
flex-flow: <flex-direction> || <flex-wrap>;

主轴对齐方式：起点对齐（默认） | 终点对齐 | 居中对齐 | 两端对齐 | 分散对齐
justify-content: flex-start | flex-end | center | space-between | space-around;

交叉轴对齐方式：拉伸对齐（默认） | 起点对齐 | 终点对齐 | 居中对齐 | 第一行文字的基线对齐
align-items: stretch | flex-start | flex-end | center | baseline;

多根轴线对齐方式：拉伸对齐（默认） | 起点对齐 | 终点对齐 | 居中对齐 | 两端对齐 | 分散对齐
align-content: stretch | flex-start | flex-end | center | space-between | space-around;
```

#### Flex项目属性

```css
顺序：数值越小越靠前，默认为0
order: <number>;

放大比例：默认为0，如果有剩余空间也不放大，值为1则放大，2是1的双倍大小，以此类推
flex-grow: <number>;

缩小比例：默认为1，如果空间不足则会缩小，值为0不缩小
flex-shrink: <number>;

项目自身大小：默认auto，为原来的大小，可设置固定值 50px/50%
flex-basis: <length> | auto;

flex-grow, flex-shrink 和 flex-basis 的简写，默认值为0 1 auto
两个快捷值：auto (1 1 auto) 和 none (0 0 auto)
flex:none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]

项目自身对齐：继承父元素（默认） | 起点对齐 | 终点对齐 | 居中对齐 | 基线对齐 | 拉伸对齐
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```

#### 应用

左右分栏、上下分栏。（检验下自己如何完成下列布局）

![上下分栏.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549370833162-7634582a-1731-4566-96e6-0632a2ea0cfa.png?x-oss-process=image/resize,w_416)![左右分栏.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549370843489-6c8f8d27-843d-4447-9e39-b74e665bde27.png?x-oss-process=image/resize,w_408)![内容结构板块.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549370859715-e41edd2d-53a8-4528-9365-4c0c3f9f1f40.png?x-oss-process=image/resize,w_398)

对 flex 不熟悉的同学可以参考下面网站进行学习：

- [30 分钟学会 Flex 布局](https://zhuanlan.zhihu.com/p/25303493)
- [flex网页布局首选方案 灵活应用 flex 弹性布局快速构建 web 结构](http://www.deathghost.cn/article/css/38)

不愿意看长篇大论的可以通过这个塔防游戏来学习：[Flexbox Defense](http://www.flexboxdefense.com/)

## li 与 li 之间有看不见的空白间隔是什么原因引起的？有什么解决办法？

### 场景

> 有时，在写页面的时候，会需要将 <li> 这个块状元素横排显示，此时就需要将 display 属性设置为 inline-block ，此时问题出现了，在两个 <li> 元素之间会出现大约 8px 左右的空白间隙

### 原因

浏览器的默认行为是把 inline 元素间的空白字符（空格换行 tab ）渲染成一个空格，也就是我们上面的代码 <li> 换行后会产生换行字符，而它会变成一个空格，当然空格就占用一个字符的宽度。

### 解决

给 ul 标签设置 `font-size: 0;` 并为 li 元素重新设置 `font-size: xxpx;`

## BFC(块级格式化上下文：block formatting context)

### 理解

BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。我们可以利用这些特性解决特定环境下的布局问题。

### 常见 BFC 应用

#### 清除元素内部浮动

> 计算BFC的高度时，自然也会检测浮动或者定位的盒子高度。

只要把父元素设为 BFC 就可以清理子元素的浮动了，最常见的用法就是在父元素上设置 `overflow: hidden` 样式，对于 IE6 加上 `zoom: 1` 就可以了。

![fu.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/260235/1549248451155-32c8a424-69d4-4e63-8892-3d6baa570934.jpeg)

#### 解决外边距合并问题

> 盒子垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻盒子的 margin 会发生重叠

属于同一个 BFC 的两个相邻盒子的 margin 会发生重叠，那么我们创建不属于同一个 BFC ，就不会发生 margin 重叠了。

![ma.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549249157981-50f0a18a-c021-4f4c-a1d2-9161d5b29200.png)

```html
<div class="container4 green" style="height: 500px;">
    <!-- 给其中一个 div 外面包一个 div ，然后通过触发外面这个 div 的 BFC ，就可以阻止这两个 div 的 margin 重叠 -->
    <div style="overflow: hidden;">
        <div class="blue w100 h100" style="margin-bottom: 20px;"></div>
    </div>
    <div class="red w200 h200" style="margin-top: 20px;"></div>
</div>
```

#### 两栏布局，实现左定宽+右宽自适应

> 普通流体元素BFC后，为了和浮动元素不产生任何交集，顺着浮动边缘形成自己的封闭上下文

![you.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549249227363-1b32cd7b-610e-4a7e-925c-05c6fc7ea76d.png?x-oss-process=image/resize,w_590)

```html
<div class="container" style="height: 300px; background-color: red;">
  <div style="width: 100px; height: 100px; float: left; background-color: blue;"></div>
  <div style="overflow: hidden; background-color: green;">我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容我是右边内容</div>
  <!-- 给右侧div添加 overflow: hidden即可 -->
</div>
```

> 注意：设置浮动后，**div 会被 float 覆盖，而文本却不会被 float 覆盖**，是因为** float 当初设计的时候**就是为了**使文本围绕在浮动对象的周围**。

参考: [\[布局概念\] 关于CSS-BFC深入理解](https://juejin.im/post/5909db2fda2f60005d2093db)

## 请解释一下为什么需要清除浮动？清除浮动的方式

浮动本质是用来做一些文字混排效果的，但是被我们拿来做布局用，则会有很多的问题出现。

由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响，为了解决这些问题，此时就需要在该元素中清除浮动。准确地说，并不是清除浮动，而是**清除浮动后造成的影响**

### 本质

清除浮动主要为了解决父级元素因为子级浮动引起内部高度为 0 的问题。

### 如何清除

设置一个 clearfix 类，在出现浮动问题的父元素上添加此类即可：

```css
.clearfix::after {
    content: "";
    display: block;
    clear: both;
    height: 0;
    visibility: hidden;
}
.clearfix {
    *zoom: 1;
}
```

## 什么是外边距合并？

外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。 合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

> 当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并：

![img](https://cdn.nlark.com/yuque/0/2019/gif/260235/1549267541505-1dfd9327-a171-40f4-bd87-eda5a347ff30.gif)

> 当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并：

![img](https://cdn.nlark.com/yuque/0/2019/gif/260235/1549267561084-700185b4-3e8b-4665-a5f4-0c9149fc0702.gif)

> 尽管看上去有些奇怪，但是外边距甚至可以与自身发生合并。
>
> 假设有一个空元素，它有外边距，但是
>
> 没有边框或填充
>
> 。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并：

![img](https://cdn.nlark.com/yuque/0/2019/gif/260235/1549267577840-ad07cab7-66c8-45f0-97a7-67702219cf70.gif)

> 如果这个外边距遇到另一个元素的外边距，它还会发生合并：

![img](https://cdn.nlark.com/yuque/0/2019/gif/260235/1549267597856-b20a453d-b664-40bb-8a30-8feec3b1e22d.gif)

**注释：**只有普通文档流中块框的垂直外边距才会发生外边距合并。**行内框、浮动框或绝对定位**之间的外边距不会合并。

## 浏览器是怎样解析CSS选择器的？

从右向左的，这样会提高查找选择器所对应的元素的效率

1. 样式系统从关键选择器开始匹配，然后左移查找规则选择器的祖先元素。
2. 只要选择器的子树一直在工作，样式系统就会持续左移，直到和规则匹配，或者是因为不匹配而放弃该规则。

## 抽离样式模块怎么写，说出思路，有无实践经验？

我们按照 CSS 的性质和用途，将 CSS 文件分成 「公共型样式」、「特殊型样式」、「皮肤型样式」，并以此顺序引用（按需求决定是否添加版本号）。

1. 公共型样式：包括了以下几个部分：“标签的重置和设置默认值”、“统一调用背景图和清除浮动或其他需统一处理的长样式”、“网站通用布局”、“通用模块和其扩展”、“元件和其扩展”、“功能类样式”、“皮肤类样式”。
2. 特殊型样式：当某个栏目或页面的样式与网站整体差异较大或者维护率较高时，可以独立引用一个样式：“特殊的布局、模块和元件及扩展”、“特殊的功能、颜色和背景”，也可以是某个大型控件或模块的独立样式。
3. 皮肤型样式：如果产品需要换肤功能，那么我们需要将颜色、背景等抽离出来放在这里。

参考：[CSS规范 - 分类方法](http://nec.netease.com/standard/css-sort.html)

## 元素竖向的百分比设定是相对于父容器的高度吗？

- 对于 height 属性来说是的。
- 对于 margin-top/bottom(padding-top/bottom) 来讲不是，而是相对于容器的宽度计算的

## 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

响应式网站设计（Responsive Web design）是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。
基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。

```css
@media screen and (max-width: 990px) {
    .container{
        background: orange;
    }
}

/* <head>里边 <link rel=”stylesheet” href=”xxx.css” media=”only screen and (max-width:480px)”> */
```

页面头部必须有 meta 声明的 viewport ：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

兼容 ie6-8 方案：使用 respond.js 插件：

```html
<!--[if lte IE 8]>
<script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
<![endif]-->
```

## 响应式和自适应设计的区别

其实 RWD 和 AWD 都是响应式设计，从外观上很难分辨，但他们自己运行机理不同，

RWD 是主动式的响应设计，AWD 是被动式的响应式设计。

- RWD 不管用户使用的是什么设备都是在服务器把数据推送到浏览器后，脚本或 CSS 自行侦测屏幕大小后执行对应的样式表内容，并且一直通过本地脚本在监听屏幕大小的变化，随时做出样式响应的变化，所以是主动的。
- AWD 是用户通过浏览器发送请求后，服务器根据请求中用户设备信息（request headers 的 user-agent）做出判断，调用已经在服务器里准备好的，适应对应设备样式文件+HTML内容+JS，返回给浏览器以这种方式响应不同设备。

```js
var deviceAgent = request.headers["user-agent"].toLowerCase();
var agentID = deviceAgent.match(/(iphone|ipod|ipad|android)/);
if (agentID) {
    console.log("手机访问");
} else {
    console.log("电脑访问");
}
```

![rwdawd.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549352188368-47bb3ec1-f6cb-4c9f-ab4b-253dd6816319.png)

## 你对 line-height 是如何理解的？如何单行文本垂直居中？多行文本呢？

行高是指一行文字的高度，具体说是两行文字间基线的距离。CSS 中起高度作用的是 height 和 line-height ，没有定义 height 属性，最终其表现作用一定是 line-height 。

### 单行文本垂直居中：（行高 == 元素高度）

```css
height:40px;
line-height:40px; /* 行高==高 */
```

### 多行文本垂直居中：（父容器table，子容器table-cell+vertical-align:middle）

```css
<div>
    <span>
        啊实打实大苏打啊实打实大师大师的啊实打实大苏打
    </span>
</div>

div {
    width: 300px;
    height: 200px;
    display: table;
    background-color: pink;
}
span {
    display: table-cell;
    vertical-align: middle;
}
```

## 设置元素浮动后，该元素的display值是多少？

自动变成了 display: block

## 让页面里的字体变清晰，变细用 CSS 怎么做？

```css
-webkit-font-smoothing: antialiased;
```

## overflow: scroll 时不能平滑滚动的问题怎么处理？

一个 div 使用了 `overflow:scroll;` ，在移动端可以滚动，但是无法平滑滚动（就像浏览网页那样）

开启滚动硬件加速：

```css
-webkit-overflow-scrolling: touch;
```

## 什么是 CSS 预处理器 / 后处理器？

### CSS 预处理器

CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。通俗的说，CSS 预处理器用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件

> Sass（SCSS）、LESS

### CSS 后处理器

CSS 后处理器 是对 CSS 进行处理，并最终生成 CSS 的 预处理器，它属于广义上的 CSS 预处理器。

我们很久以前就在用 CSS 后处理器 了，最典型的例子是 CSS 压缩工具（如 clean-css），只不过以前没单独拿出来说过。还有最近比较火的 Autoprefixer，以 Can I Use 上的 浏览器支持数据 为基础，自动处理兼容性问题。

> Rework、PostCSS

参考：[CSS预处理器和后处理器](https://blog.csdn.net/yushuangyushuang/article/details/79209752)

## 图片为什么有左右上下间隙,怎么去除：

原因：

- 左右：因为 img 是 inline-block 行内元素，行内元素之间有『换行（回车），空格，tab』时会产生左右间隙
- 上下：行内元素默认与父容器基线对齐，而基线与父容器底部有一定间隙，所以上下图片间有间隙

解决办法：

- 移除上下间隙：
    - img 本身设置 display: block;
    - 父元素设置 font-size: 0; （基线与字体大小有关，字体为零，基线间就没距离了）
    - img本身设置 vertical-align: bottom;（让 inline-block 的 img 与每行的底部对齐）
- 移除左右间距：
    - 行内元素间不要有换行，连成一行写消除间隙
    - 第一行结尾写上 `<!-- ，第二行开头跟上 -->` 。即利用注释消除间距
    - 父元素 font-size 设置 0
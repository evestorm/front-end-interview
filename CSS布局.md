# CSS布局

## 如何居中div？

### 行内元素水平居中

```css
#container {
    text-align: center;
}
```

### 单个 div（块级元素）水平居中

```css
#center {
  width: 200px;
  margin: 0 auto;
}
```

### 多个 div 水平居中

```css
/* 传统方案 */
#container {
    text-align: center;
}
#center {
    display: inline-block;
}

/* flex 布局方案 */
#container {
    justify-content: center;
    display: flex;
}
```

### 绝对定位的 div 居中（已知宽高）

#### 子绝父相 + margin

```css
#center {
  position: absolute;
  width: 300px;
  height: 300px;
  margin: auto; /* 0 auto 水平居中；auto 水平垂直居中 */
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: pink; /* 方便看效果 */
}
```

#### 子绝父相 + 负边距

```css
#center {
  position: absolute;
  width:500px;
  height:300px;
  top: 50%;
  left: 50%;
  margin: -150px 0 0 -250px;  /* 外边距为自身宽高的一半 */
  background-color: pink;     /* 方便看效果 */
}
```

#### 当被居中的元素是 inline or inline-block 元素

```css
#container {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
```

### 任意元素（未知宽高）

#### 子绝父相 + translate

```css
#container {
    position: relative;
}

/* 利用transform */
#center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

#### flex（需考虑兼容性）

```css
#container {
  display: flex;
  display: -webkit-flex; /* Safari仍旧需要使用特定的浏览器前缀 */
  align-items: center;    /* 垂直居中 */
  justify-content: center;  /* 水平居中 */
}
#center {
  width: 100px;
  height: 100px;
  background-color: pink;   /* 方便看效果 */
}
```

## 请解释一下CSS3的 Flexbox（弹性盒布局模型）,以及适用场景？

该布局模型的目的是提供一种更加高效的方式来对容器中的条目进行布局、对齐和分配空间。

在传统的布局方式中:

- block 布局是把块在垂直方向从上到下依次排列的；
- inline 布局则是在水平方向来排列。
- flex 弹性盒布局并没有这样内在的方向限制，可以由开发人员自由操作。

适用场景：弹性布局适合于移动前端开发，在 Android 和 iOS 上也完美支持。

参考：http://www.w3cplus.com/css3/flexbox-basics.html



### flex 是什么属性的缩写

flex 属性是 `flex-grow`、`flex-shrink` 和 `flex-basis` 的简写



### 如何使用 flex 实现三列等宽布局

父元素 display: flex，子元素 flex: 1

#### 解释

子元素的设置等同于：flex: 1 1 auto

flex的默认值是：0 1 auto

意思是项目默认有剩余空间也不放大（0），但空间不足会缩小（1）

现在改为了值 1 ，就可以放大了，所以三栏可以平分宽度

阅读：[flex设置成1和auto有什么区别](https://segmentfault.com/q/1010000004080910)

### 如何使用 flex 实现下列布局

![flex](https://gitee.com/evestorm/various_resources/raw/master/css/flex-left.png)

> HTML

```html
<div id="container">
  <div class="box box1">1</div>
  <div class="box box2">2</div>
  <div class="box box3">3</div>
  <div class="box box4">4</div>
  <div class="box box5">5</div>
</div>
<p>margin-right: auto</p>
```

> CSS

```css
#container {
  display: flex;
  justify-content: flex-end;
  background-color: lightyellow;
}
.box {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  width: 75px;
  background-color: springgreen;
  border: 1px solid #333;
}
.box1 {
  margin-right: auto;
}
p {
  width: 100%;
  text-align: center;
}
```

摘自：[css flex布局中妙用 margin: auto](https://juejin.im/post/5bde54ce51882516e840a8af)

## 用纯CSS创建一个三角形的原理是什么？

```css
把上、左、右三条边隐藏掉（颜色设为 transparent）
#demo {
    width: 0;
    height: 0;
    border-width: 20px;
    border-style: solid;
    border-color: transparent transparent red transparent;
}
```

## css多列等高如何实现？

### 原理

利用 `padding-bottom|margin-bottom` 正负值相抵； 

设置父容器设置超出隐藏（overflow:hidden），这样子父容器的高度就还是它里面的列没有设定 padding-bottom 时的高度， 当它里面的任一列高度增加了，则父容器的高度被撑到里面最高那列的高度， 其他比这列矮的列则会用它们的 padding-bottom 来补偿这部分高度差。

因为背景是可以用在 padding 占用的空间里的，而且边框也是跟随 padding 变化的，所以就成功的完成了一个障眼法。

### 实现

> HTML

```html
<ul class="Article">
  <li class="js-equalheight">
    <p>
      一家将客户利益置于首位的经纪商，
      为客户提供专业的交易工具一家将客户利益置于首位的经纪商
    </p>
  </li>
  <li class="js-equalheight">
    <p>一家将客户利益置于首位的经纪商，为客户提供专业的交易工具</p>
  </li>
  <li class="js-equalheight">
    <p>一家将客户利益置于首位的经纪商</p>
  </li>
</ul>
```

> CSS

```css
.Article {
  overflow: hidden;
}

.Article > li {
  float: left;
  margin: 0 10px -9999px 0;
  padding-bottom: 9999px;
  background: #4577dc;
  width: 200px;
  color: #fff;
}

.Article > li > p {
  padding: 10px;
}
```

## **css div 垂直水平居中，并完成 div 高度永远是宽度的一半（宽度可以不指定）**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      html,
      body {
        width: 100%;
        height: 100%;
      }

      .outer {
        width: 400px;
        height: 100%;
        background: blue;
        margin: 0 auto;

        display: flex;
        align-items: center;
      }

      .inner {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 50%;
        background: red;
      }

      .box {
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">
        <div class="box">hello</div>
      </div>
    </div>
  </body>
</html>
```



<iframe height="486" style="width: 100%;" scrolling="no" title="高度是宽度一半" src="https://codepen.io/JingW/embed/oNYJovB?height=486&theme-id=light&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/JingW/pen/oNYJovB'>高度是宽度一半</a> by JingW
  (<a href='https://codepen.io/JingW'>@JingW</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

来源：https://github.com/cttin/cttin.github.io/issues/2





## 有一个高度自适应的 div ，里面有两个 div ，一个高度 100px ，希望另一个填满剩下的高度。

> HTML

```html
<body>
    <div class="outer">
        <div class="A"></div>
        <div class="B"></div>
    </div>
</body>
```

> CSS

### 方案1

```css
.A {
    height: 100px;
    background-color: pink;
}

.B {
    background-color: blue;
    height: calc(100vh - 100px);
}
```

### 方案2

```css
/* 
  利用border-box将padding包含在高度内； 
*/
.outer {
    height: 100%;
    padding: 100px 0 0;
    box-sizing: border-box;
    background-color: pink;
}
/* 
  用负margin值将A子容器顶到页面顶部
*/
.A {
    height: 100px;
    margin: -100px 0 0;
    background: #BBE8F2;
}
/* 
  剩下的高度100%-100px就是B容器所谓的100%高了
*/
.B {
    height: 100%;
    background: #D9C666;
}
```

### 方案3

```css
.outer {
    height: 100%;
    padding: 100px 0 0;
    box-sizing: border-box;
    position: relative;
}
/*
  绝对定
*/
.A {
    height: 100px;
    background: #BBE8F2;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
}

.B {
    height: 100%;
    background: #D9C666;
}
```

### 方案4

```css
.outer {
    height: 100%;
    position: relative;
}

.A {
    height: 100px;
    background: #BBE8F2;
}

.B {
    background: #D9C666;
    width: 100%;
    position: absolute;
    top: 100px;
    left: 0;
    bottom: 0;
}
```

### 方案5

```css
.outer {
    height: 100%;
    background: red;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    -webkit-flex-direction: row;
}

.A {
    width: 100%;
    height: 100px;
    background: green;
}

.B {
    background: blue;
    flex: 1;
}
```

## rem布局知道吗？原理是什么？

rem是个相对单位，相对的是html根元素的font-size大小。这样一来我们就可以通过html上的字体大小来控制页面上所有以rem为单位的元素尺寸。

**示例代码**

例如在vue项目的index.html页中动态计算根元素字体大小

> index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no">
    ...
  </head>
  <body>
    <noscript>
      ...
    </noscript>
    <div id="app"></div>
    <script>
    ;(function(doc, win) {
      const docEl = doc.documentElement
      const resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize'
      const recalc = () => {
        const clientWidth = docEl.clientWidth
        // 把屏幕宽度划分成10份，那么每一份的宽度就等于1rem
        docEl.style.fontSize = clientWidth / 10 + 'px'
      }
      if (!doc.addEventListener) return
      win.addEventListener(resizeEvt, recalc, false)
      doc.addEventListener('DOMContentLoaded', recalc, false)
      recalc()
    })(document, window)
    </script>
  </body>
</html>
```

然后利用css预处理器定义一个mixin函数（下方用的stylus），用来将设计稿上的px转为对应的rem值

计算公式为：

页面元素的rem值 = 设计稿元素值（px）/（屏幕宽度 / 划分的份数）

> mixin.stylus

```css
/* 把px转为rem */
px2rem(designpx)
  $rem = 750 / 10;
  return (designpx / $rem)rem
```

最后在css中使用

> 某vue组件的style标签中

```css
/* 375设计稿上100%宽以及50高的导航条（iPhone6的dpr为2，所以px值得都乘以2） */
header
  width px2rem(750)
  height px2rem(100)
```

### rem布局的优缺点

优点：

能维持能整体的布局效果，移动端兼容性好，不用写多个css代码，而且还可以利用@media进行优化。

缺点：

开头要引入一段 js 代码，单位都要改成 rem（ font-size 可以用 px ），计算 rem 比较麻烦(可以引用预处理器，但是增加了编译过程，相对麻烦了点)。PC 和 Mobile 要分开。

参考：

- [用 rem 实现 WebApp 自适应的优劣分析](https://www.cnblogs.com/qieguo/p/5386565.html)
- [CSS3 的字体大小单位\[rem\]到底好在哪？](https://www.zhihu.com/question/21504656)
- [总结个人使用过的移动端布局方法](https://segmentfault.com/a/1190000010211016)

## 慎用 em 的原因

**em会叠加计算。**在这个机制下太容易犯错了，因为你不知道这段`css`指定的字号具体是多少。

> HTML

```html
<span>
    abc
    <span>def</span>
    abc
</span>
```

> CSS

```css
span { font-size: 1.5em; }
```

实际效果：

![abc.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549344767634-006a0ce5-2c0c-4087-97c3-c8e77933009d.png)

先要搞清楚 `em` 的计算原理，它是根据**当前元素的字号**按比例计算的。

外层 `span` 的字号是 `16px`（浏览器默认值），所以 `1.5em` 之后是 `24px` 。由于字号是继承的，导致内层 `span` 的字号继承过来是 `24px` ，再经过 `1.5em` 之后就成了 `36px` 。

所以，就算要用 `em` 的话，尽量不要用在继承属性（`font-size`）上，除非你真的清楚你在做什么！

## 两列布局（左列定宽，右列自适应）

> HTML

```html
<body>
  <div id="left">左列定宽</div>
  <div id="right">右列自适应</div>
</body>
```

### 1. 利用 float + margin

```css
#left {
    background-color: #f00;
    float: left;
    width: 100px;
    height: 500px;
}

#right {
    background-color: #0f0;
    height: 500px;
    margin-left: 100px; /*设置间隔，大于等于#left的宽度*/
}

原理：#left左浮动，脱离文档流，#right为了不被#left挡住，
设置margin-left大于等于#left的宽度达到视觉上的两栏布局
```

### 2. 使用 float + overflow（触发bfc）

```css
#left {
    background-color: #f00;
    float: left;
    width: 100px;
    height: 500px;
}

#right {
    background-color: #0f0;
    height: 500px;
    overflow: hidden; /*触发bfc达到自适应*/
}

原理：#left左浮动，#right触发bfc达到自适应
```

### 3. 使用 table 实现

```css
#parent {
    width: 100%;
    display: table;
    height: 500px;
}

#left {
    width: 100px;
    background-color: #f00;
}

#right {
    background-color: #0f0;
}
/*利用单元格自动分配宽度*/
#left, #right {
    display: table-cell;
}
```

### 4. 使用 position 实现

```css
#parent{
    position: relative;
}  /*父相*/

#left {
    position: absolute;  /*子绝*/
    top: 0;
    left: 0;
    background-color: #f00;
    width: 100px;
    height: 500px;
}

#right {
    position: absolute;  /*子绝*/
    top: 0;
    left: 100px;  /*值大于等于#left的宽度*/
    right: 0;
    background-color: #0f0;
    height: 500px;
}
```

### 5. 使用 flex 实现

```css
#parent {
    width: 100%;
    height: 500px;
    display: flex;
}

#left {
    width: 100px;
    background-color: #f00;
}

#right {
    flex: 1; /*均分了父元素剩余空间*/
    background-color: #0f0;
}
```

## 两列布局（一列不定宽，一列自适应）

### 1. float + overflow

```css
#left {
    margin-right: 10px;
    float: left;  /*只设置浮动,不设宽度*/
    height: 500px;
    background-color: #f00;
}

#right {
    overflow: hidden;  /*触发bfc*/
    height: 500px;
    background-color: #0f0;
}

原理：#left不设宽度左浮动，#right触发 bfc 达到自适应
```

### 2. flex 布局

```css
#parent{
    display: flex;
}

#left { /*不设宽度*/
    margin-right: 10px;
    height: 500px;
    background-color: #f00;
}

#right {
    height: 500px;
    background-color: #0f0;
    flex: 1;  /*均分#parent剩余的部分*/
}
```

## 三列布局（左中定宽，右侧自适应）

> HTML

```html
<body>
<div id="parent">
    <div id="left">左列定宽</div>
    <div id="center">中间定宽</div>
    <div id="right">右列自适应</div>
</div>
</body>
```

### 1. float + margin

```css
#parent{
    min-width: 310px;
} /*100+10+200,防止宽度不够,子元素换行*/

#left {
    margin-right: 10px;  /*#left和#center间隔*/
    float: left;
    width: 100px;
    height: 500px;
    background-color: #f00;
}

#center{
    float: left;
    width: 200px;
    height: 500px;
    background-color: #eeff2b;
}

#right {
    margin-left: 320px;  /*等于#left和#center的宽度之和加上间隔,多出来的就是#right和#center的间隔*/
    height: 500px;
    background-color: #0f0;
}
```

### 2. float + overflow

```css
#parent{
    min-width: 320px;
} /*100+10+200+20,防止宽度不够,子元素换行*/

#left {
    margin-right: 10px; /*间隔*/
    float: left;
    width: 100px;
    height: 500px;
    background-color: #f00;
}

#center{
    margin-right: 10px; /*在此定义和#right的间隔*/
    float: left;
    width: 200px;
    height: 500px;
    background-color: #eeff2b;
}

#right {
    overflow: hidden;  /*触发bfc*/
    height: 500px;
    background-color: #0f0;
}
```

### 3. 利用 position

```css
#parent {
    position: relative;
} /*父相*/

#left {
    position: absolute; /*子绝*/
    top: 0;
    left: 0;
    width: 100px;
    height: 500px;
    background-color: #f00;
}

#center {
    position: absolute; /*子绝*/
    left: 100px;        /*对应#left的width值*/
    top: 0;
    width: 200px;
    height: 500px;
    background-color: #eeff2b;
}

#right {
    position: absolute; /*子绝*/
    left: 300px;        /*对应#left和#center的width值之和*/
    top: 0;
    right: 0;
    height: 500px;
    background-color: #0f0;
}
```

### 4. 利用 table

```css
#parent {
    width: 100%;
    height: 520px; /*抵消上下间距10*2的高度影响*/
    margin: -10px 0;  /*抵消上下边间距10的位置影响*/
    display: table;
    /*左右两边间距无法消除,子元素改用padding设置盒子间距就没有这个问题*/
    border-spacing: 10px;  /*关键!!!设置间距*/
}

#left {
    display: table-cell;
    width: 100px;
    background-color: #f00;
}

#center {
    display: table-cell;
    width: 200px;
    background-color: #eeff2b;
}

#right {
    display: table-cell;
    background-color: #0f0;
}
```

### 5. 利用 flex

```css
#parent {
    height: 500px;
    display: flex;
}

#left {
    margin-right: 10px;  /*间距*/
    width: 100px;
    background-color: #f00;
}

#center {
    margin-right: 10px;  /*间距*/
    width: 200px;
    background-color: #eeff2b;
}

#right {
    flex: 1;  /*均分#parent剩余的部分达到自适应*/
    background-color: #0f0;
}
```

## 三列布局（两侧定宽，中间自适应）

### 圣杯布局 [详解](https://blog.csdn.net/konglei1996/article/details/50881391?utm_source=blogxgwz4)

> HTML

```html
<div class="container">
    <div class="center">中间</div>  <!-- 让中间第一，这样浮动的时候它会先占据100%宽 -->
    <div class="left">左边</div>
    <div class="right">右边</div>
</div>
```

> CSS

```css
.container {
  height: 500px;
  min-width: 400px;  /* 左边w*2+右边w */
  padding: 0 100px 0 150px; /* 1. 父容器左右留出固定padding */
  background-color: gray;
}

.left {
  margin-left: -100%; /* 4.1 让被挤到center下方的left左移整个容器的宽度，此时左上角与center重叠 */
  position: relative; /* 4.2 为了让left移到最左边,设置相对定位 */
  left: -150px; /* 4.3 然后相对自己左移自已宽度的单位，使自己移动到最左边 */
  height: 100%;
  float: left;  /* 2.1 左浮动 */
  width: 150px;
  background-color: rgba(233, 233, 0, .2);
}

.center {
  height: 100%;
  width: 100%;  /* 3. 让center占据剩下父容器的100%宽 */
  float: left;  /* 2.1 左浮动 */
  background-color: rgba(165, 12, 23, .4);
}

.right {
  margin-right: -100px; /* 5. 给在第二行的right设置一个负自己宽度的margin-right。让其最右边 */
  height: 100%;
  width: 100px;
  float: left;  /* 2.1 左浮动 */
  background-color: green;
}
```

### 双飞翼布局方法 [详情](http://m.10qianwan.com/web/350789.html)

> HTML

```html
<div class="container">
    <div class="center">
        <div>中间</div>
    </div>
    <div class="left">左边</div>
    <div class="right">右边</div>
</div>
```

> CSS

```css
.container {
    width: 100%;  /* 1. 整个容器宽100%自适应 */
    height: 500px;
    background-color: gray;
}

.left {
    width: 100px; /* 3.1 设置定宽 */
    margin-left: -100%; /* 5. 左移容器宽度100%个单位，就把自己排在了center前面 */
    background-color: rgba(233, 233, 0, .2);
}

.center {
    width: 100%;  /* 4. 中间容器100%，此时会把左右挤到第二行 */
    background-color: rgba(165, 12, 23, .4);
}

.center div {
    margin: 0 150px 0 100px;  
    /* 7. 此时center的左右分别有100和150宽与left，right重叠。所以让center子容器左右margin抵消 */
}

.right {
    width: 150px; /* 3.2 设置定宽 */
    margin-left: -150px;  /* 6. 同样左移自身宽度的单位，让right也回到第一行 */
    background-color: green;
}

.left, .center, .right {
    height: 100%;
    float: left; /*2. 子容器全部左浮动 */
}
```

## 布局的几种方式

1. 伸缩布局 flex
2. 流式布局 百分比
3. 响应式布局 媒体查询（超小屏设备时：流式布局）
   <!- 以上布局共同点：元素只能做到宽度的适配（排除图片）->
4. rem布局 宽度和高度都能做到适配（等比适配）

## Flex

### flex布局和传统布局的优势？

该布局模型的目的是提供一种更加高效的方式来对容器中的条目进行布局、对齐和分配空间。

在传统的布局方式中:

- block 布局是把块在垂直方向从上到下依次排列的
- inline 布局则是在水平方向来排列
- flex 弹性盒布局并没有这样内在的方向限制，可以由开发人员自由操作

#### 我有一段文字，但不知道多长，如何实现单行文本居中，多行文本居左显示？

flex，单行的时候文本不足会居中，多行会自动折行居左:

```css
display: flex;
justify-content: center;
```



## 如何解决移动端 Retina 屏 1px 像素问题

### 原理：媒体查询 + 变换缩放

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1.0"/>
    <style type="text/css">
      * { margin: 0; padding: 0; }
      #test {
        width: 100%;
        height: 1px;
        margin-top: 50px;
        background: black;
      }
      @media only screen and (-webkit-device-pixel-ratio:2 ) {
        #test {
          transform: scaleY(.5);
        }
      }
      @media only screen and (-webkit-device-pixel-ratio:3 ) {
        #test {
          transform: scaleY(.333333333333333333);
        }
      }
    </style>
  </head>
  <body>
    <div id="test"></div>
  </body>
</html>
```

### stylus 实现方案

> stylus

```css
/* 给 dpr 1.5 的设备设置 0.7 的缩放 */
@media (-webkit-min-device-pixel-ratio: 1.5), (min-device-pixel-ratio: 1.5)
  .border1px
    &::after
      -webkit-transform: scaleY(.7)
      transform: scaleY(.7)
/* 给 dpr 2.0 的设备设置 0.5 的缩放 */
@media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2)
  .border1px
    &::after
      -webkit-transform: scaleY(.5)
      transform: scaleY(.5)

/* 给需要1px下边框的元素添加一个伪元素，
使其宽100%；高为用户自定义 */
border1px($color)
  position: relative
  &:after
    display: block
    position: absolute
    left: 0
    bottom: 0
    width: 100%
    border-top: 1px solid $color
    content: ' '
```

> html

```html
<!-- 1. 给需要添加1像素边框的元素设置一个类作为标识，让媒体查询能够识别 -->
<header class="border1px"></header>

<style lang="stylus" scoped>
header
  ...
  border1px(black) /* 2. 通过border1px设置边框 */
</style>
```

## 其它

### 实现不使用 border 画出 1px 高的线，在不同浏览器的标准模式与怪异模式下都能保持一致的效果。

```html
 <div style="height:1px;overflow:hidden;background:red"></div>
```

### 已知如下代码，如何修改才能让图片宽度为 300px ？注意下面代码不可修改

```html
<img src="1.jpg" style="width:480px!important;">
```

答案：

```html
<!-- 方法一：max-width覆盖 -->
<img src="1.jpg" style="width:480px!important; max-width: 300px">
<!-- 方法二：scale按比例缩放 -->
<img src="1.jpg" style="width:480px!important; transform: scale(0.625, 1);" >
<!-- 方法三：相同属性直接覆盖 -->
<img src="1.jpg" style="width:480px!important; width:300px!important;">
<!-- 方法四：css动画样式优先级高于important特性 -->
<style>
img {
    animation: test 0s forwards;
}
@keyframes test {
    from {
        width: 300px
    }
    to {
        width: 300px;
    }
}
</style>
```

题目来源：[Daily-Interview-Question 第 60 题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/105)

### 了解box-sizing吗？

box-sizing 属性可以被用来调整这些表现:

- `content-box` 是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
- `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。
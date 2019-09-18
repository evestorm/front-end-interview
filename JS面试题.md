# JS面试题

## ES6

### let / const

#### 全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？

ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let、const、class命令声明的全局变量，不属于顶层对象的属性。只在一个块级作用域（Script）中，获取时不加 `window/global` 就好：

```js
let aa = 1;
const bb = 2;

console.log(aa); // 1
console.log(bb); // 2
```

答案链接：[关于 const 和 let 声明的变量不在 window 上](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/30)

### 箭头函数

#### 箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

4、不可以使用 new 命令，因为：

- 没有自己的 this，无法调用 call，apply。
- 没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__

new 过程大致是这样的：

```javascript
function newFunc(father, ...rest) {
  var result = {};
  result.__proto__ = father.prototype;
  var result2 = father.apply(result, rest);
  if (
    (typeof result2 === 'object' || typeof result2 === 'function') &&
    result2 !== null
  ) {
    return result2;
  }
  return result;
}
```

题目来源：[Daily-Interview-Question 第58题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/101)

### Promise

#### 10 个 Ajax 同时发起请求，全部返回展示结果，并且至多允许三次失败，说出设计思路

这个问题相信很多人会第一时间想到 `Promise.all` ，但是这个函数有一个局限在于如果失败一次就返回了，直接这样实现会有点问题，需要变通下。以下是两种实现思路

```js
// 以下是不完整代码，着重于思路 非 Promise 写法
let successCount = 0
let errorCount = 0
let datas = []
ajax(url, (res) => {
     if (success) {
         success++
         if (success + errorCount === 10) {
             console.log(datas)
         } else {
             datas.push(res.data)
         }
     } else {
         errorCount++
         if (errorCount > 3) {
            // 失败次数大于3次就应该报错了
             throw Error('失败三次')
         }
     }
})
// Promise 写法
let errorCount = 0
let p = new Promise((resolve, reject) => {
    if (success) {
         resolve(res.data)
     } else {
         errorCount++
         if (errorCount > 3) {
            // 失败次数大于3次就应该报错了
            reject(error)
         } else {
             resolve(error)
         }
     }
})
Promise.all([p]).then(v => {
  console.log(v);
});
```

## JS 垃圾回收机制

### 谈谈垃圾回收机制的方式及内存管理？

**题目及答案来源：** [2019 前端面试 | “HTML + CSS + JS”专题](https://juejin.im/post/5ce4171ff265da1bd04eb4f3#heading-5) JavaScript 基础部分，编号为 [js_11]

**一、垃圾回收机制——GC**

Javascript 具有自动垃圾回收机制（GC：Garbage Collecation），也就是说，执行环境会负责管理代码执行过程中使用的内存。

原理：垃圾收集器会定期（周期性）找出那些不在继续使用的变量，然后释放其内存。

通常情况下有两种实现方式：

- 标记清除
- 引用计数

**标记清除**： js 中最常用的垃圾回收方式就是标记清除。

当变量进入环境时，例如，在函数中声明一个变量，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。



**引用计数**的含义是跟踪记录每个值被引用的次数。

当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是 1。如果同一个值又被赋给另一个变量，则该值的引用次数加 1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减 1。当这个值的引用次数变成 0 时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾回收器下次再运行时，它就会释放那些引用次数为 0 的值所占用的内存。



**二、内存管理**

1. Javascript 引擎基础 GC 方案是

   （simple GC）：mark and sweep（标记清除），即：

   1）遍历所有可访问的对象；

   2）回收已不可访问的对象。

2. GC 的缺陷

   和其他语言一样，JavaScript 的 GC 策略也无法避免一个问题：GC 时，停止响应其他操作，这是为了安全考虑。而 Javascript 的 GC 在 100ms 甚至以上，对一般的应用还好，但对于 JS 游戏，动画对连贯性要求比较高的应用，就麻烦了。这就是新引擎需要优化的点：避免 GC 造成的长时间停止响应。

3. GC 优化策略

   - 1）分代回收（Generation GC）

     这个和 Java 回收策略思想是一致的。目的是通过区分“临时”与“持久”对象；多回收“临时对象”区（young generation），少回收“持久对象”区（tenured generation），减少每次需遍历的对象，从而减少每次 GC 的耗时。

   - 2）增量 GC

     这个方案的思想很简单，就是“每次处理一点，下次再处理一点”，如此类推。

**更多阅读：**

- [JS垃圾回收机制](https://evestorm.github.io/posts/20229/)

### 哪些操作会造成内存泄漏？

#### 意外的全局变量引起的内存泄漏

```js
function foo(arg) {
  bar = '全局变量'; // 没有声明变量 实际上是全局变量=>window.bar
}
```

原因：全局变量，不会被回收。

为什么不能泄漏到全局？平时不都会定义一些全局变量么：
全局变量根据定义无法被垃圾回收机制收集。需要特别注意用于临时存储和处理大量信息的全局变量。如果必须使用全局变量来存储数据，请确保将其指定为null或在完成后重新分配它。

解决：使用严格模式避免，函数内使用var定义，块内使用let、const。

#### 闭包引起的内存泄漏

```js
function bindEvent() {
  var obj = document.createElement("XXX");
  var unused = function () {
      console.log(obj,'闭包内引用obj obj不会被释放');
  };
  // obj = null;
}
```

原因：闭包可以维持函数内局部变量，使其得不到释放，造成内存泄漏。

解决：将事件处理函数定义在外部，解除闭包，或者在定义事件处理函数的外部函数中，删除对dom的引用。例如上述案例，手动解除引用 `obj = null`
 。

#### 被遗忘的定时器和回调函数

```js
var someResource = getData();
setInterval(function() {
    var node = document.getElementById('Node');
    if(node) {
        node.innerHTML = JSON.stringify(someResource));
        // 定时器也没有清除
    }
    // node、someResource 存储了大量数据 无法回收
}, 1000);
```

原因：当**不需要** `setInterval` 或者 `setTimeout` 时，**定时器没有被clear**，定时器的**回调函数以及内部依赖的变量都不能被回收**，造成内存泄漏。

解决：在定时器完成工作的时候，手动清除定时器

#### 没有清理 DOM 元素引用

```js
var refA = document.getElementById('refA');
document.body.removeChild(refA); // dom删除了
console.log(refA, "refA");  // 但是还存在引用能console出整个div 没有被回收
refA = null;
console.log(refA, "refA");  // 解除引用
```

原因: 保留了DOM节点的引用,导致GC没有回收

解决：refA = null

#### IE9以下浏览器环境导致的循环引用

```js
var element = document.getElementById('something');
var myObject = new Object();
myObject.element = element; // element属性指向dom
element.someThing = myObject; // someThing回指myObject 出现循环引用(两个对象一直互相包含 一直存在计数)。
```

原因：IE9以下 BOM 和 DOM 对象使用 C++ 以 COM 对象的形式实现的。COM 的垃圾收集机制采用的是引用计数策略，这种机制在出现循环引用的时候永远都释放不掉内存。

解决：不使用它们的时候，手动切断链接：`myObject.element = null; element.someThing = null;`

## 闭包

### 能讲下 JavaScript 的闭包么？

一个函数包含了另一个函数或另一个对象，里面这个对象或函数都可以使用外部函数的变量或参数，此时就形成了闭包。

### 你平常怎么用闭包的？

#### 匿名自执行函数避免全局污染（模块化开发）

创建了一个匿名立即执行函数，由于外部无法引用它内部的变量，因此在函数执行完后会立刻释放资源，不污染全局对象。

```js
const msg = (function () {
  const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
  const today = new Date()
  const msg = 'Today is ' + days[today.getDay()] + ', ' + today.getDate()
  return {
    getMsg() {
      return msg
    }
  }
}())

console.log(msg.getMsg())
```

#### 缓存数据

有时候我们需要获取一个处理过程很耗时的对象，并且往往会把这个复杂的处理过程封装成一个方法。但每次需要用到它时都需要调用它从而花费很长时间，那么我们就需要将计算出来的值存储起来，当调用这个函数的时候，首先在缓存中查找，如果找不到，则进行计算，然后更新缓存并返回值，如果找到了，直接返回查找到的值即可。闭包就可以做到这一点，因为它不会释放外部的引用，从而函数内部的值可以得以保留。

```js
var CachedSearchBox = (function () {
  var cache = {},
    count = [];
  return {
    attachSearchBox: function (dsid) {
      if (dsid in cache) { //如果结果在缓存中
        return cache[dsid]; //直接返回缓存中的对象
      }
      var fsb = new uikit.webctrl.SearchBox(dsid); //新建
      cache[dsid] = fsb; //更新缓存
      if (count.length > 100) { //保正缓存的大小<=100
        delete cache[count.shift()];
      }
      return fsb;
    },
    clearSearchBox: function (dsid) {
      if (dsid in cache) {
        cache[dsid].clearSelection();
      }
    }
  };
})();
```

#### 拿到正确的值

```js
for(var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i) //10个10
  }, 1000)
}
```

解决方案：for循环中声明10个自执行函数，保存当时的值到内部

```js
for (var i = 0; i < 10; i++) {
  (function(j) {
    setTimeout(() => {
      console.log(j)
    }, 1000);
  })(i)
}
```

#### 高级排他

场景：一个ul li列表，鼠标移入时高亮当前li标签，移除之前li标签的高亮状态。

常规写法：两层for循环，外层遍历li标签，给每个li添加 onmouseover 事件；里层for循环用来在高亮当前li标签之前，移除所有li标签高亮状态。

闭包写法：定义一个preIndex变量存储上次高亮li标签的索引，for循环内部使用立即执行函数+闭包保存index索引，这样就使li标签与index索引一一对应，然后在onmouseover事件触发后，根据preIndex找到之前高亮标签并移除高亮状态，并设置当前标签高亮，最后将preIndex设置为当前高亮标签的index索引。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
  * { margin: 0; padding: 0; list-style: none; }
  ul li { width: 100%; height: 20px; margin-bottom: 5px; background-color: grey; }
  li.active { background-color: yellow; }
  </style>
</head>
<body>
  <ul>
    <li class="active"></li>
    <li></li>
    <li></li>
  </ul>
  <script>
    window.onload = function() {
      let list = document.querySelectorAll('li')
      // 记录移动前选中li的对应索引
      let preActiveIndex = 0
      for (let i = 0; i < list.length; i++) {
        (function(j) {
          const li = list[i]
          li.onmouseover = function() {
            // 清除
            list[preActiveIndex].className = ''
            // 设置
            this.className = 'active'
            // 赋值
            preActiveIndex = j
          }
        })(i)
      }
    }
  // 常规写法
  // window.onload = function() {
  //   let list = document.querySelectorAll('li')
  //   for (let i = 0; i < list.length; i++) {
  //     const li = list[i]
  //     li.onmouseover = function() {
  //       for (let j = 0; j < list.length; j++) {
  //         list[j].className = ''
  //       }
  //       this.className = 'active'
  //     }
  //   }
  // }
  </script>
</body>
</html>
```

### 闭包会产生哪些问题？

闭包会使函数中的变量不能及时释放，造成内存消耗过大，从而导致网页的性能问题。不过目前浏览器引擎都基于 V8，而 V8 引擎有个 gc 回收机制，不用太过担心变量不会被回收。

## Event Loop

### setTimeout、Promise、Async/Await 的区别

这题主要是考察这三者在事件循环中的区别，事件循环中分为宏任务队列和微任务队列。
其中setTimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行；
promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；async函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。

**题目来源**：https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/33

## 概念性问题

### 观察者模式与发布订阅模式有什么区别

可以把观察者模式想象成顾客与微商的关系。顾客关注了微商的商品，微商就会记住顾客，每当微商有新产品时，就会直接**私聊**来通知所有关注了自己的顾客。这里的顾客就相当于观察者，微商就是被观察者。

而发布订阅模式可以看做是顾客通过「商城平台」关注了商家的商品，商家一旦上新就通过「商城平台」向关注了自己的顾客**群发**消息，这里的顾客就是订阅者，「商城平台」就是事件总线，商家就是发布者。

通过上面两个例子就能看出，观察者模式的模型跟发布/订阅者模型里，差距就差在有没有一个**中央的事件总线**。如果有这个事件总线，我们就可以认为是个发布订阅模型。如果没有，那么就可以认为是个观察者模型。因为其实它们都实现了一个关键的功能：发布事件-订阅事件并触发事件。

### 讲一下面向对象的三大特征？

封装继承和多态

不过这个回答到以后，肯定会问你具体啥意思的，不了解的同学可点击 [这个](https://mp.weixin.qq.com/s/tWg-Jf30i9ZK7o1mMYLe7w?) 链接查看。

### 什么是重写，什么是重载？

重写就是子类覆盖掉父类的方法
重载就是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

## 开放性提问

这部分就只放题目不给参考答案了，毕竟每个人经历不同，放这儿让你们有个准备吧。

- 你在平时写项目中遇到的最大的困难在哪，又是如何解决掉的？
- 你简历上的项目作品中，挑一个讲一下存在哪些技术难点？
- 在使用Vue过程中遇到过哪些印象深刻的问题，如何解决的？
- 未来3-5年对自己的计划？
- 为什么喜欢前端？
- 对自己做一个自我评价吧
- 谈谈你自己的不足，或者想提升的？
- 做的最成功的一件事？
- 有没有什么主动争取的东西？
- 你是怎么学前端的？
- 平时有关注一些博客什么的吗，讲一个最近看的？
- 你还有什么想问我的嘛？

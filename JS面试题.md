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

### 更多阅读

- [JS垃圾回收机制](https://evestorm.github.io/posts/20229/)

## 哪些操作会造成内存泄漏？

### 意外的全局变量引起的内存泄漏

```js
function foo(arg) {
  bar = "this is a hidden global variable"; // 没有用var
}
```

原因：全局变量，不会被回收。

为什么不能泄漏到全局？平时不都会定义一些全局变量么：
全局变量根据定义无法被垃圾回收机制收集。需要特别注意用于临时存储和处理大量信息的全局变量。如果必须使用全局变量来存储数据，请确保将其指定为null或在完成后重新分配它。

解决：使用严格模式避免，函数内使用var定义，块内使用let、const。

### 闭包引起的内存泄漏

```js
function bindEvent(){
    var obj=document.createElement("XXX")
    obj.onclick=function() {/* */}
}
// 解决
function bindEvent(){
    var obj=document.createElement("XXX")
    obj.onclick=function() {/* */}
    obj=null
}
```

原因：闭包可以维持函数内局部变量，使其得不到释放。

解决：将事件处理函数定义在外部，解除闭包，或者在定义事件处理函数的外部函数中，删除对dom的引用。

### 被遗忘的定时器和回调函数

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

原因：定时器中有dom的引用，即使dom删除了，但是定时器还在，所以内存中还是有这个dom。

解决：在定时器完成工作的时候，手动清除定时器

### DOM 引用

```js
var refA = document.getElementById('refA');
document.body.removeChild(refA); // dom删除了
console.log(refA, "refA");  // 但是还存在引用
能console出整个div 没有被回收
```

原因: 保留了DOM节点的引用,导致GC没有回收

解决：refA = null

### 写一个函数判断是否存在循环引用 ❌

### 如何统计用户的点击量 ❌

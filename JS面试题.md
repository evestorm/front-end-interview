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

## 讲一下 JS 垃圾回收机制

这部分可参考我的博客：[JS垃圾回收机制](https://evestorm.github.io/posts/20229/)

## 那些操作会造成内存泄漏？❌

内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。

垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。

闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

### 写一个函数判断是否存在循环引用 ❌

### 如何统计用户的点击量 ❌

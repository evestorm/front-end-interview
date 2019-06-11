# JS进阶题

## 对象

### 写一下浅/深拷贝

> 深拷贝和浅拷贝针对的是引用类型，JS中的变量类型分为值类型（基本类型）和引用类型；对值类型进行复制操作会对值进行一份拷贝，而对引用类型复制，则会进行地址的拷贝，最终两个变量指向同一份数据。对于引用类型，会导致a b指向同一份数据，此时如果对其中一个进行修改，就会影响到另外一个，有时候这可能不是我们想要的结果。

#### 浅拷贝

```js
// 实现一个浅拷贝，就是遍历源对象，然后在将对象的属性的属性值都放到一个新对象里就ok了

// 方法1：遍历
function copy(obj) {
  if (!obj || typeof obj !== 'object') return

  var newObj = obj.constructor === Array ? [] : {}
  for (var key in obj) {
    newObj[key] = obj[key]
  }
  return newObj
}
var a = {b: 'bb', c: 'cc',  d: {e: 'ee'}}
var b = copy(a)
console.log(b) // { b: 'bb', c: 'cc', d: { e: 'ee' } }

// 方法2：原生方法 Object.assign
var a = {a : 'old', b : { c : 'old'}}
var b = Object.assign({}, a)
b.a = 'new'
b.b.c = 'new'
console.log(a) // { a: 'old', b: { c: 'new' } }
console.log(b) // { a: 'new', b: { c: 'new' } }
```

#### 深拷贝

```js
// 方法1：转 JSON 再转回来
var obj1 = {a: {name: '小红'}, b: 2}
var obj2 = JSON.parse(JSON.stringify(obj1))
obj1.a.name = '被修改了'
obj2   //{"a":{"name":"小红"},"b":2}  《---没有被修改

// JSON方法的缺点：
//  不能复制 function、正则、Symbol
//  循环引用报错
//  相同的引用会被重复复制

// 方法2：递归的方法
function copy(obj) {
    // 递归退出条件
    // 拷贝对象不存在或不是数组或不是对象
    if (!obj || typeof obj !== 'object') return obj

    var newObj = obj.constructor === Array ? [] : {}
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            // 如果是数组或者对象
            if (typeof obj[key] === 'object') {
                // 递归
                newObj[key] = copy(obj[key])
            } else {
                // 否则直接返回
                newObj[key] = obj[key]
            }
        }
    }
    return newObj
}

var old = { a: 'old', b: { c: 'old' } }
var newObj = copy(old)
newObj.b.c = 'new'
console.log(old) // { a: 'old', b: { c: 'old' } }
console.log(newObj) // { a: 'old', b: { c: 'new' } }
```

参考：

- [浅探js深拷贝和浅拷贝](https://segmentfault.com/a/1190000016970483)
- [深拷贝的终极探索](http://www.cnblogs.com/zhangycun/p/9799787.html)

## JS垃圾回收机制

这部分可参考我的博客：[JS垃圾回收机制](https://evestorm.github.io/posts/20229/)

## Event Loop

同样可参考我的博客：[Event Loop 学习笔记](https://evestorm.github.io/posts/10505/)

## 综合题

### 编写一个可拖拽的 div

> HTML

```html
<div id="sw"></div>
```

> CSS

```css
#sw { position: absolute; }
```

> JS

```js
var flag = false
var position = null
var sw = document.querySelector("#sw")
sw.addEventListener("mousedown", function (e) {
    flag = true
    position = [e.clientX, e.clientY]
    console.log(e.clientX, e.clientY)
})
// 这里监听 document ，如果监听 sw 则会有快速拖动导致鼠标「脱离」 div 的 bug
document.addEventListener("mousemove", function (e) {
    if (!flag) return
    var x = e.clientX
    var y = e.clientY
    var moveX = x - position[0]
    var moveY = y - position[1]
    var left = parseInt(sw.style.left || 0)
    var top = parseInt(sw.style.top || 0)
    // 注意 style.left 带 px 单位
    sw.style.left = left + moveX + 'px'
    sw.style.top = top + moveY + 'px'
    position = [x, y]
})
document.addEventListener("mouseup", function() {
    flag = false
})
```

## 其它

### 那些操作会造成内存泄漏？❌

内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。

垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。

闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

### 写一个函数判断是否存在循环引用 ❌

### 为什么0.1+0.2不等于0.3？在什么场景下遇到这个问题，如何解决？

> 二进制模拟十进制进行计算时 的精度问题

```js
// 方法1：ES6的 Number.EPSILON ，这个值无限接近于0。0.1 + 0.2的精度误差在这个值的范围内
function numbersEqual(a,b) {
    return Math.abs(a - b) < Number.EPSILON
}
var a = 0.1 + 0.2, b=0.3
console.log(numbersEqual(a,b))    //true


// 方法2：parseFloat + 内置函数 toFixed
function formatNum(num, fixed = 10) {
    // a.toFixed(fixed) 先转为小数点10位的字符串 "0.3000000000"
    return parseFloat(a.toFixed(fixed)) // 然后通过parseFloat转为浮点数
}
var a = 0.1 + 0.2;
console.log(formatNum(a)) //0.3

// 方法3：内置函数toPrecision(中文：精确，精度)
// 参数是精度.比如 5.1234 ，传 2 返回 5.1 ，传 1 返回 5 ；0.2 + 0.1 传 2 返回 0.30
(0.1 + 0.2).toPrecision(10) == 0.3 // true
```

参考：

- [0.1 + 0.2不等于0.3？为什么JavaScript有这种“骚”操作？](https://juejin.im/post/5b90e00e6fb9a05cf9080dff)
- [JavaScript的设计缺陷?浮点运算：0.1 + 0.2 != 0.3](https://blog.csdn.net/nineteen73/article/details/51184387)

### 实现一个单例

```js
var SingleTest = (function () {
    var _instance = null
    SingleInstance.prototype._init = function(ops) {
        for (let i in ops) {
            this[i]=ops[i]
        }
    }
    function SingleInstance(args) {
        if (_instance == null) {
            _instance=this
        }
        _instance._init(args)
        return _instance
    }
    return SingleInstance
})()

var i1=new SingleTest({name:"lance1"})
var i2=new SingleTest({name:"lance2"})
console.log(i1===i2)  // 结果是true
console.log(i1.name)  // 结果是escapist3
```

### 如何统计用户的点击量 ❌

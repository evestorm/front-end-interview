# JS基础题

## 字符串

### 如何将字符串转化为数字，例如'12.3b'？

```js
parseFloat('12.3b')
// 12.3
```

### 判断一个给定的字符串是否是同构的

> 输入: s = "egg", t = "add"
> 输出: true
> 输入: s = "foo", t = "bar"
> 输出: false

```js
var isIsomorphic = function(s, t) {
    var i = 0
    while (i < s.length) {
      if (s.indexOf(s[i]) != t.indexOf(t[i])){
        return false
      }
      i++
    }
    return true
}
```

## 数组

### 判断数组的方法

```js
var arr = [1, 2, 3]

arr instanceof Array

Object.prototype.toString.call(arr) === '[object Array]'

Array.isArray(arr)

arr.constructor === Array // 前提是保证 constructor 不会被更改
// 不能使用 typeof 因为 它返回的是个 'object'
```

### JS数组去重

```js
var arr = [1, 1, "1", "1", NaN, NaN, {}, NaN]

// 方法1：indexOf去重（无法对 对象，NaN 识别去重）
// → indexOf 不认 NaN，遇到NaN就返回 -1
function myDistinct(arr) {
    var result = []
    for (var i = 0; i < arr.length; i++) {
        if (result.indexOf(arr[i]) === -1) {
            result.push(arr[i])
        }
    }
    return result
}
// 返回数组：[ 1, '1', NaN, NaN, {}, {}, NaN ]
// 原数组：[ 1, 1, '1', '1', NaN, NaN, {}, {}, NaN ]

// 方法2：filter + indexOf（对 对象 和 NaN 无效）
function myDistinct(arr) {
    var result = arr.filter((item, index, arr) => {
        // 重复的值的索引肯定大于第一次出现这个值的索引，不会被返回
        return arr.indexOf(item) === index
    });
    return result
}
// 返回数组：[ 1, '1', {}, {} ]
// 原数组：[ 1, 1, '1', '1', NaN, NaN, {}, {}, NaN ]

// 方法3：filter 升级版（解决了 对象 和 NaN 的问题）
function myDistinct(arr) {
    var obj = {}
    return arr.filter((item, index) => {
        // 由于对象的键值只能是字符串，导致 1 和 '1' 被认为是相同的值，所以下方使用 typeof item + item 拼成字符串作为 key 值来避免这个问题
        return obj.hasOwnProperty(typeof item + JSON.stringify(item)) 
              ? false 
              : obj[typeof item + JSON.stringify(item)] = true
    })
}
// 返回数组：[ 1, '1', NaN, {} ]
// 原数组：[ 1, 1, '1', '1', NaN, NaN, {}, {}, NaN ]

// 方法4：for循环（解决了 对象 和 NaN 的问题）
~function() {
    var arr = [1, 1, "1", "1", NaN, NaN, {}, NaN]

    var prop = Array.prototype
    prop.myDistinct = function myDistinct() {
        var hash = {}
        for (var i = 0; i < this.length; i++) {
            var temp = typeof this[i] + JSON.stringify(this[i])
            if (hash[temp]) {
                this[i] = this[this.length - 1]
                this.length--
                i--
                continue
            }
            hash[temp] = true
        }
        return this
    }
    console.log(arr.myDistinct())
}()
```

### 两个数组比较，判断是否有相同元素（交集）

```js
var a = [1, 2, 3, 4, 5, NaN]
var b = [2, 4, 6, 8, 10, NaN]

// 方法1：filter + indexOf（支持 NaN）
var c = a.filter(v => b.indexOf(v) > -1) // []
var aHasNaN = a.some(v => isNaN(v))
var bHasNaN = b.some(v => isNaN(v))
var c = a.filter(v => b.indexOf(v) > -1)
          .concat(aHasNaN && bHasNaN ? [NaN] : [])

// 若下面 a、b 情况，上面方法会导致结果为 [2, 2]，但按正常逻辑预期应该是 [2] )
var a = [1, 2, 2, 1, NaN]
var b = [2, NaN]

var aHasNaN = a.some(v => isNaN(v))
var bHasNaN = b.some(v => isNaN(v))
return a.filter(v => {
        var index = b.indexOf(v)
        if (index > -1) {
            b.splice(index, 1)
            return true
        }
    }).concat(aHasNaN && bHasNaN ? [NaN] : [])

// 方法2：es7 includes
var c = a.filter(v => b.includes(v))
```

### 如何实现数组的随机排序？

```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// 方法1：随机交换数组内的元素
function result(arr) {
    for (let i = 0; i < arr.length; i++) {
        // 随机索引【Math.random()返回一个浮点,  伪随机数在范围[0，1)】
        let randomIndex = parseInt(Math.random() * arr.length)
        // 存下当前正常索引值对应的数字
        let curNum = arr[i]
        // 将其重新赋值为随机索引对应的数字
        arr[i] = arr[randomIndex]
        // 将随机索引对应的数字替换为当前正常索引值对应的数字
        arr[randomIndex] = curNum
    }
    return arr
}

// 方法2：sort() 可以调用一个函数做为参数，如果这个函数返回的值为负数表示数组中的 a 项排在 b 项前
arr.sort(function() {
  return Math.random() - .5
})
console.log(arr)
```

## 随机数

### 如何获取0-9的随机数

- **Math.round(num)**：将 **num** 四舍五入取整
- **Math.floor(num)**：将 **num** 向下取整，即返回 **num** 的整数部分。当然我们也可以使用 **parseInt()** 方法代替。
- **Math.ceil(num)**：向上取整

```js
// (min, max)：
return Math.round(Math.random() * (max - min - 2) + min + 1)
// [min, max]：
return Math.round(Math.random() * (max - min) + min)
// (n, m]：
return Math.ceil(Math.random() * (max - min) + min)
// [n, m)：
return Math.floor(Math.random() * (max - min) + min)
```

## 原型 & 原型链

### JavaScript 中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

**hasOwnProperty**

hasOwnProperty 返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法不会检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。

使用方法： object.hasOwnProperty(proName) 其中参数 object 是必选项，一个对象的实例；proName 是必选项。一个属性名称的字符串值。 如果 object 具有指定名称的属性，那么 JavaScript 中 hasOwnProperty 函数方法返回 true，反之则返回 false。

### instanceof 的实现原理

> 思路：只要右边变量的 prototype 在左边变量的原型链上即可。

```js
function myInstanceOf(leftValue, rightValue) {
    let rightProto = rightValue.prototype // 取右表达式的 prototype 值
    leftValue = leftValue.__proto__ // 取左表达式的 __proto__ 值
    while (true) {
        if (leftValue === null) {
            return false
        }
        if (leftValue === rightProto) {
            return true
        }
        leftValue = leftValue.__proto__
    }
}
```

参考：[浅谈 instanceof 和 typeof 的实现原理](https://juejin.im/post/5b0b9b9051882515773ae714)

## setTimeout

### setTimeout的机制

等到当前脚本的同步任务和 "任务队列" 中已有的事件，全部处理完以后，才会执行 setTimeout 指定的任务。

参考：[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

## 综合题

### 如何将浮点数点左边的数每三位添加一个逗号，如12000000.11转化为『12,000,000.11』？

```js
var num = 12000000.11

// 方法1：利用 toLocaleString() 返回某语言系统下数字的表示字符串 IE6+
num.toLocaleString()

// 方法2
function toThousands(num) {
    if (typeof num !== 'number') return 0
    // 判断 num 是否小于 0 ，小于则设 flag 为 "-" 并且把 num 转为绝对值
    if (num < 0) {
        flag = "-"
        num = Math.abs(num)
    }
    // 转为数组 e.g. [ '12000000', '11' ]
    var arr = num.toString().split(".")
    // 分别把「.」左边和右边存起来
    var left = [...arr[0]]
    var right = ""
    // 如果 num 不是个整数的情况
    if (arr.length > 1) {
        right = "." + arr[1]
    }
    var count = left.length - 1
    // 操作左边整数部分，逆向遍历并且逢3前面加个「,」 ，最后 i-1
    while (count > 0) {
        // [1,2,0,0,0,0]
        // 例如如果数组长度为6，则一开始 count=5 ，不加
        // count=3 时，就需要在前面加个「,」
        if (count % 3 === 0) {
            left.splice(-count, 0, ',')
        }
        count--
    }
    // 正负符号 + 左边 + 小数点和右边
    return flag + left.join("") + right
}

// 方法3：正则
function toThousands(num) {
  return num && num
    .toString()
    .replace(/(\d)(?=(\d{3})+\.)/g, function($1, $2){
    return $2 + ','
  })
}
```

## 其它

### typeof 没定义的变量会报错吗？typeof let定义了的呢？

- 未声明的变量使用 typeof 返回字符串 "undefined"
- typeof 一个 let 定义的变量会因为暂时性死区报错 [ReferenceError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)（前提：let/const 未声明之前赋值或使用）

```js
var tmp = 123
if (true) {
  tmp = 'abc' // ReferenceError: tmp is not defined
  let tmp
}

console.log(typeof tmp) // ReferenceError: tmp is not defined
let tmp

let tmp
console.log(typeof tmp) // undefined 不会报错
```

### typeof 的值有哪些

7种数据类型（返回的都是字符串形式）：

string, number, function, object, undefined, boolean, symbol（表独一无二的值）

注意：

- null 和 数组 返回的都是 object
- NaN 返回的是 number

### getElementsByClassName 和 querySelectorAll 的区别

- 参数上：querySelectorAll 方法接收的参数是一个 CSS 选择符。而 getElementsBy 系列接收的参数只能是单一的 className、tagName 和 name

    ```js
    var c1 = document.querySelectorAll('.b1 .c')
    var c2 = document.getElementsByClassName('c')
    var c3 = document.getElementsByClassName('b2')[0].getElementsByClassName('c')
    ```

- 返回值上：querySelectorAll 返回的是一个 Static Node List，而 getElementsBy 系列的返回的是一个 Live Node List

参考：

- [querySelectorAll 方法相比 getElementsBy 系列方法有什么区别？](https://www.zhihu.com/question/24702250)
- [静态NodeList 和 动态NodeList的区别](https://segmentfault.com/a/1190000008829267)

### 原生JS添加类

- element.setAttribute("class", 'Lance')
- element.className = "lance awesome"
- 追加类：element.setAttribute("class", element.getAttribute("class") + " " + "lance")

### valueOf 和 toString

- toString(): 返回对象的字符串表示。
- valueOf(): 返回对象的字符串、数值或布尔值表示。

### 如何检查一个数字是否为整数？

> 将它对 1 进行取模，看看是否有余数。

```js
function isInt(num) {
  return num % 1 === 0
}

console.log(isInt(4)) // true
console.log(isInt(12.2)) // false
console.log(isInt(0.3)) // false
```
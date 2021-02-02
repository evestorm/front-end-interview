# JS

## JS类型

string，number，boolean，undefined，null，symbol（es6），BigInt（es10），object

### 值类型和引用类型的区别

两种类型的区别是：存储位置不同；

- 值类型存储在栈（stack）中，占空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引用类型存储在堆（heap）中,占据空间大、大小不固定。如果存在栈中，影响程序运行性能；引用类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

![js类型堆栈图.png](https://cdn.nlark.com/yuque/0/2019/png/260235/1549856966458-39b2f008-64fc-4753-936b-e513a5de46d2.png)

### JS的类型检测

- typeof （判断一个变量是什么类型）undefined object function boolean string number symbol
- instanceof （判断当前对象是不是某个类型）
    ```js
    <!-- 要检测的对象 instanceof 某个构造函数 -->

    function Car(make, model, year) {
        this.make = make;
        this.model = model;
    }

    var auto = new Car('Honda', 'Accord');

    console.log(auto instanceof Car);
    // expected output: true

    console.log(auto instanceof Object);
    // expected output: true
    ```
- Object.prototype.toString.call()（检测一个对象的类型）
    ```js
    console.log(Object.prototype.toString.call("Lance"));//[object String]
    ```

### === 和 == 的区别

`==` 在允许强制转换的条件下检查值的等价性，而 `===` 是在不允许强制转换的条件下检查值的等价性；

因此 `===` 常被称为「严格等价」。（"55" == 55 true, "55" === 55 false。p.s. 把字符串转为数值）

### 哪些非 boolean 值被强制转换为一个 boolean 时，它是 false ？

- `""`（空字符串）
- `0`, `-0`, `NaN` （非法的 `number` ）
- `null`, `undefined`

### {} 和 [] 的 valueOf 和 toString 的结果是什么？

{} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"

[] 的 valueOf 结果为 [] ，toString 的结果为 ""

### null，undefined 的区别？

- null 表示一个对象是「没有值」的值，也就是值为 "空"；
- undefined 表示一个变量声明了没有初始化(赋值)；

- undefined 的类型（typeof）是 undefined ；
- null 的类型（typeof）是 object ；

- JavaScript 将未赋值的变量默认值设为undefined；
- JavaScript 从来不会将变量设为 null 。它是用来让程序员表明某个用 var 声明的变量时没有值的。

在验证 null 时，一定要使用　=== ，因为 == 无法分别 null 和 undefined

```js
null == undefined // true
null === undefined // false
```

## DOM

### DOM 操作——怎样添加、移除、移动、复制、创建和查找节点？

```js
（1）创建新节点
    createDocumentFragment()    //创建一个DOM片段
    createElement()   //创建一个具体的元素
    createTextNode()   //创建一个文本节点
（2）添加、移除、替换、插入
    appendChild()
    removeChild()
    replaceChild()
    insertBefore() //在已有的子节点前插入一个新的子节点
（3）查找
    querySelector("ul") / querySelectorAll("ul li") // 查找单个元素 / 多个元素
    getElementsByTagName("div")
    getElementsByClassName()
    getElementById()
```

### DOM树操作

```js
// 添加新节点
var p1 = document.createElement('p')
p1.innerHTML = 'this is p1'
div1.appendChild(p1) // 添加新创建的元素

// 移动已有节点。注意，这里是“移动”，并不是拷贝
var p2 = document.getElementById('p2')
div1.appendChild(p2)

// 获取父元素
var div1 = document.getElementById('div1')
var parent = div1.parentElement

// 获取子元素
var div1 = document.getElementById('div1')
var child = div1.childNodes

// 删除节点
var div1 = document.getElementById('div1')
var child = div1.childNodes
div1.removeChild(child[0])
```

### offsetWidth/offsetHeight，clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别

- **offsetWidth/offsetHeight** 返回值包含 content + padding + border，效果与 e.getBoundingClientRect() 相同；
- **clientWidth/clientHeight** 返回值只包含 content + padding，如果有滚动条，也不包含滚动条；
- **scrollWidth/scrollHeight** 返回值包含 content + padding + 溢出内容的尺寸。

### 什么是 Virtual DOM，为何要用 Virtual DOM？

Virtual DOM 的概念有很多解释，分别是：一个对象，两个前提，三个步骤。

**一个对象**指的是 Virtual DOM 是一个基本的 JavaScript 对象，也是整个 Virtual DOM 树的基本。

**两个前提**分别是 JavaScript 很快和直接操作 DOM 很慢，这是 Virtual DOM 得以实现的两个基本前提。

得益于 V8 引擎的出现，让 JavaScript 可以高效地运行，在性能上有了极大的提高。

直接操作 DOM 的低效和 JavaScript 的高效相对比，为 Virtual DOM 的产生提供了大前提。

**三个步骤**指的是 Virtual DOM 的三个重要步骤，分别是：生成 Virtual DOM 树、对比两棵树的差异、更新视图。

1.生成 Virtual DOM 树：

 DOM 是前端工程师最常接触的内容之一，一个 DOM 节点包含了很多的内容，但是抽象出一个 DOM 节点却只需要三部分：**节点类型，节点属性、子节点**。所以围绕这三个部分，我们可以使用 JavaScript 简单地实现一棵 DOM 树，然后给节点实现渲染方法，就可以实现虚拟节点到真实 DOM 的转化。

![img](https://gitee.com/evestorm/various_resources/raw/master/dom/%E8%99%9A%E6%8B%9Fdom1.webp)

2.对比两棵树的差异：

比较两棵 DOM 树的差异是 Virtual DOM 算法最核心的部分，这也是我们常说的的  Virtual DOM 的 diff 算法。在比较的过程中，我们只比较同级的节点，非同级的节点不在我们的比较范围内，这样既可以满足我们的需求，又可以简化算法实现。

![img](https://gitee.com/evestorm/various_resources/raw/master/dom/%E8%99%9A%E6%8B%9Fdom2.webp)

比较“树”的差异，首先是要对树进行遍历，常用的有两种遍历算法，分别是深度优先遍历和广度优先遍历，一般的 diff 算法中都采用的是深度优先遍历。对新旧两棵树进行一次深度优先的遍历，这样每个节点都会有一个唯一的标记。在遍历的时候，每遍历到一个节点就把该节点和新的树的同一个位置的节点进行对比，如果有差异的话就记录到一个对象里面。

例如，上面的 div 和新的 div 有差异，当前的标记是 0，那么：patches[0] = [{difference}, {difference}, ...]。同理 p 是 patches[1]，ul 是 patches[3]，以此类推。这样当遍历完整棵树的时候，就可以获得一个完整的差异对象。

![img](https://gitee.com/evestorm/various_resources/raw/master/dom/%E8%99%9A%E6%8B%9Fdom3.webp)

在这个差异对象中记录了有改变的节点，每一个发生改变的内容也不尽相同，但也是有迹可循，常见的差异包括四种，分别是：

- 替换节点
- 增加/删除子节点
- 修改节点属性
- 改变文本内容

## 对象的原生方法

### Object.assign()

copy 对象的可枚举属性

> 语法：Object.assign(target, ...sources)
>
>参数：目标对象, ...源对象
>
> 返回值：目标对象

```js
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

### Object.create()

创建新对象

> 语法：Object.create(proto, [ propertiesObject ])
>
> 参数：新创建对象的原型对象, 用于指定创建对象的一些属性，（eg：是否可读、是否可写，是否可以枚举etc）

### Object.is()

用来判断两个值是否是同一个值

```js
Object.is('haorooms', 'haorooms');     // true
Object.is(window, window);   // true

Object.is('foo', 'bar');     // false
Object.is([], []);           // false

var test = { a: 1 };
Object.is(test, test);       // true

Object.is(null, null);       // true

// 特例
Object.is(0, -0);            // false
Object.is(-0, -0);           // true
Object.is(NaN, 0/0);         // true
```

### Object.keys / Object.values

返回给定对象的自身可枚举属性 / 值 的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

var obj = ['e', 's', '8']; // 等同于 { 0: 'e', 1: 's', 2: '8' };
Object.values(obj); // ['e', 's', '8']

//当把数字当做对象的键的时候，返回的数组以键的值升序排序
var obj = { 10: 'xxx', 1: 'yyy', 3: 'zzz' };
Object.values(obj); // ['yyy', 'zzz', 'xxx']
Object.values('es8'); // ['e', 's', '8']
```

### Object.entries()

**`Object.entries()`** 方法返回对象自身可枚举属性的键值对数组，其排列与使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。

```js
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// iterate through key-value gracefully
const obj = { a: 5, b: 7, c: 9 };
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}

const obj2 = { 10: 'xxx', 1: 'yyy', 3: 'zzz' };
Object.entries(obj2); // [['1', 'yyy'], ['3', 'zzz'], ['10', 'xxx']]
Object.entries('es8'); // [['0', 'e'], ['1', 's'], ['2', '8']]
```

## 数组

### 数组的遍历方法

- 标准for循环
- forEach((当前值, 当前索引,当前数组)=>{})
    - 无法中途退出循环，只能用 `return` 退出本次回调，进行下一次回调。
    - 它总是返回 undefined 值,即使你 return 了一个值。
- for-in（不推荐）会把继承链的对象属性都会遍历一遍，而且数组遍历不一定按次序
    - for-in 循环返回的是所有能通过对象访问的、可枚举的属性。
- for (variable of iterable)（ES6）可迭代 [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array) ，[Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Map)，[Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)，[String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 等（迭代的是值 value ）
    - 在 `for-of` 中如果遍历中途要退出，可以使用 `break` 退出循环。

#### ES5

- map (不改变原数组) 会给原数组中的每个元素都按顺序调用一次 callback 函数
- reduce (不改变原数组) 数组中的前项和后项做某种计算,并累计最终值。

    ```js
    // arr.reduce(function(total, currentValue, currentIndex, arr), initialValue)
    // callback 参数
    // (累积器, 当前元素, 当前元素索引, 当前数组)
    // initialValue:指定第一次回调 的第一个参数
    var wallets = [4, 7.8, 3]
    var totalMoney = wallets.reduce(function (countedMoney, curMoney) {
        return countedMoney + curMoney;
    }, 0)
    ```

- filter（不改变原数组）

    ```js
    var arr = [2, 3, 4, 5, 6]
    var morearr = arr.filter(function (number) {
        return number > 3
    }) // [4,5,6]
    ```
- every（不改变原数组）测试数组的所有元素是否都通过了指定函数的测试
    - 如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
    - 如果所有元素都满足条件，则返回  true 。
    ```js
    var arr = [1,2,3,4,5]
    var result = arr.every(function (item, index) {
        return item > 2
    }) // false
    ```
- some（不改变原数组）测试是否至少有一个元素通过 callback 中的条件.对于放在空数组上的任何条件，此方法返回 false 。
    - 如果有一个元素满足条件，则表达式返回 true , 剩余的元素不会再执行检测。
    - 如果没有满足条件的元素，则返回 false 。
    ```js
    // some(callback, thisArg)
    // callback:
    //    (当前元素, 当前索引, 调用some的数组)

    var arr = [1,2,3,4,5]
    var result = arr.some(function (item,index) {
        return item > 3
    }) // true
    ```

#### ES6

- find() & findIndex() 根据条件找到数组成员
    - find() 定义：用于找出第一个符合条件的数组成员，并返回该成员，如果没有符合条件的成员，则返回 undefined 。
    - findIndex() 定义：返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回 -1 。

    ```js
    <!-- 语法 -->
    let new_array = arr.find(function(currentValue, index, arr), thisArg)
    let new_array = arr.findIndex(function(currentValue, index, arr), thisArg)
    <!-- 这两个方法都可以识别NaN,弥补了indexOf的不足 -->
    <!-- find -->
    let a = [1, 4, -5, 10].find((n) => n < 0);
    <!-- 返回元素-5 -->
    let b = [1, 4, -5, 10,NaN].find((n) => Object.is(NaN, n));
    <!-- 返回元素NaN -->
    <!-- findIndex -->
    let a = [1, 4, -5, 10].findIndex((n) => n < 0);
    <!-- 返回索引2 -->
    let b = [1, 4, -5, 10,NaN].findIndex((n) => Object.is(NaN, n));
    <!-- 返回索引4 -->
    ```

- keys() & values() & entries() 遍历键名、遍历键值、遍历键名+键值
    - 三个方法都返回一个新的 Array Iterator 对象，对象根据方法不同包含不同的值。

    ```js
    // 语法
    array.keys()   array.values()   array.entries()

    for (let index of ['a', 'b'].keys()) {
        console.log(index);
    }
    // 0
    // 1

    for (let elem of ['a', 'b'].values()) {
        console.log(elem);
    }
    // 'a'
    // 'b'

    for (let [index, elem] of ['a', 'b'].entries()) {
        console.log(index, elem);
    }
    // 0 "a"
    // 1 "b"
    ```

#### for in 和 for of 区别

```js
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {  <-- 循环的是索引
  console.log(i); // 打印 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // 打印 0, 1, 2, "foo"
  }
}

for (let i of iterable) {  <-- 迭代的是值
  console.log(i); // 打印 3, 5, 7
}
```

参考：[JavaScript 数组遍历方法的对比](https://juejin.im/post/5a3a59e7518825698e72376b)

### JS数组有哪些方法

#### 改变原数组的方法（9个）

##### splice() 添加 / 删除数组元素

> splice() 方法**向/从数组中添加/删除**项目，然后返回被删除的项目
>
> array.splice(index,howmany,item1,.....,itemX)
>
> 1. index：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
> 2. howmany：可选。要删除的项目数量。如果设置为 0，则不会删除项目。
> 3. item1, ..., itemX： 可选。向数组添加的新项目。
>
> 返回值: 如果有元素被删除,返回包含被删除项目的新数组。

**删除元素**

```js
// 从数组下标0开始，删除3个元素
let a = [1, 2, 3, 4, 5, 6, 7]
let item = a.splice(0, 3) // [1,2,3]
console.log(a) // [4,5,6,7]

// 从最后一个元素开始删除3个元素，因为最后一个元素，所以只删除了7
let item = a.splice(-1, 3) // [7]
```

**删除并添加**

```js
// 从数组下标0开始，删除3个元素，并添加元素'添加'
let a = [1, 2, 3, 4, 5, 6, 7]
let item = a.splice(0,3,'添加') // [1,2,3]
console.log(a) // ['添加',4,5,6,7]

// 从数组最后第二个元素开始，删除3个元素，并添加两个元素'添加1'、'添加2'
let b = [1, 2, 3, 4, 5, 6, 7]
let item = b.splice(-2,3,'添加1','添加2') // [6,7]
console.log(b) // [1,2,3,4,5,'添加1','添加2']
```

**不删除只添加**

```js
let a = [1, 2, 3, 4, 5, 6, 7]
let item = a.splice(0,0,'添加1','添加2') // [] 没有删除元素，返回空数组
console.log(a) // ['添加1','添加2',1,2,3,4,5,6,7]

let b = [1, 2, 3, 4, 5, 6, 7]
let item = b.splice(-1,0,'添加1','添加2') // [] 没有删除元素，返回空数组
console.log(b) // [1,2,3,4,5,6,'添加1','添加2',7] 在最后一个元素的前面添加两个元素
```

##### sort() 数组排序

> 定义: sort() 方法对数组元素进行排序，并返回这个数组。
>
> 参数可选: 规定排序顺序的比较函数。
>
> 默认情况下 sort() 方法没有传比较函数的话，默认按字母升序，如果不是元素不是字符串的话，会调用 `toString()` 方法将元素转化为字符串的 Unicode (万国码)位点，然后再比较字符。

**不传参**

```js
// 字符串排列 看起来很正常
var a = ["Banana", "Orange", "Apple", "Mango"]
a.sort() // ["Apple","Banana","Mango","Orange"]

// 数字排序的时候 因为转换成Unicode字符串之后，有些数字会比较大会排在后面 这显然不是我们想要的
var a = [10, 1, 3, 20,25,8]
console.log(a.sort()) // [1,10,20,25,3,8];
```

_比较函数的两个参数：_

sort 的比较函数有两个默认参数，要在函数中接收这两个参数，这两个参数是数组中两个要比较的元素，通常我们用 a 和 b 接收两个将要比较的元素：

- 若比较函数返回值 <0 ，那么 a 将排到 b 的前面;
- 若比较函数返回值 =0 ，那么 a 和 b 相对位置不变；
- 若比较函数返回值 >0 ，那么 b 排在 a 将的前面；

**数字升降序**

```js
 var array =  [10, 1, 3, 4, 20, 4, 25, 8];  
 array.sort(function(a,b){
   return a-b;
 });
 console.log(array); // [1,3,4,4,8,10,20,25];

 array.sort(function(a,b){
   return b-a;
 });
 console.log(array); // [25,20,10,8,4,4,3,1];
```

##### pop() 删除一个数组中的最后的一个元素

> 定义: pop() 方法删除一个数组中的最后的一个元素，并且返回这个元素。

```js
let  a =  [1,2,3]
let item = a.pop()  // 3
console.log(a) // [1,2]
```

##### shift() 删除数组的第一个元素

> 定义: shift()方法删除数组的第一个元素，并返回这个元素。

```js
let  a =  [1,2,3]
let item = a.shift()  // 1
console.log(a) // [2,3]
```

##### push() 向数组的末尾添加元素

> 定义：push() 方法可向数组的末尾添加一个或多个元素，并返回新的长度。
>
> 参数: item1, item2, ..., itemX ,要添加到数组末尾的元素

```js
let  a =  [1,2,3]
let item = a.push('末尾', '233')  // 5
console.log(a) // [1,2,3,'末尾', '233']
```

##### unshift()

> 定义：unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。
>
> 参数: item1, item2, ..., itemX ,要添加到数组开头的元素

```js
let a = [1, 2, 3]
let item = a.unshift('开头', '开头2')  // 5
console.log(a) // [ '开头', '开头2', 1, 2, 3 ]
```

##### reverse() 颠倒数组中元素的顺序

> 定义: reverse() 方法用于颠倒数组中元素的顺序。

```js
let  a =  [1,2,3]
a.reverse()
console.log(a) // [3,2,1]
```

#### 不改变原数组的方法（8个）

##### slice() 浅拷贝数组的元素

> 定义： 方法返回一个从开始到结束（**不包括结束**）选择的数组的一部分浅拷贝到一个新数组对象，且原数组不会被修改。
>
> 语法：array.slice(begin, end);

```js
let a= ['hello','world']
let b=a.slice(0,1) // ['hello']

// 新数组是浅拷贝的，元素是简单数据类型，改变之后不会互相干扰。
// 如果是复杂数据类型(对象,数组)的话，改变其中一个，另外一个也会改变。
a[0] = '改变原数组'
console.log(a,b) // ['改变原数组','world'] ['hello']

let a = [{name:'OBKoro1'}]
let b = a.slice()
console.log(b,a) // [{"name":"OBKoro1"}]  [{"name":"OBKoro1"}]
// a[0].name='改变原数组'
// console.log(b,a) // [{"name":"改变原数组"}] [{"name":"改变原数组"}]
```

##### join() 数组转字符串

> 定义: join() 方法用于把数组中的所有元素通过指定的分隔符进行分隔放入一个字符串，返回生成的字符串。
>
> 语法: array.join(str)

```js
let a = ['hello','world']
let str = a.join() // 'hello,world'
let str2 = a.join('+') // 'hello+world'

let a = [['OBKoro1','23'],'test']
let str1 = a.join() // OBKoro1,23,test
let b = [{name:'OBKoro1',age:'23'},'test']
let str2 = b.join() // [object Object],test
// 对象转字符串推荐JSON.stringify(obj)

// 结论：
// join()/toString() 方法在数组元素是数组的时候，会将里面的数组也调用 join()/toString() ,
// 如果是对象的话，对象会被转为 [object Object] 字符串。
```

##### toLocaleString() 数组转字符串

> 定义: 返回一个表示数组元素的字符串。该字符串由数组中的每个元素的  toLocaleString() 返回值经调用  join() 方法连接（由逗号隔开）组成。

```js
let a = [{
    name: 'OBKoro1'
}, 23, 'abcd', new Date()]

console.log(a.join(","))
console.log(a.toString())
console.log(a.toLocaleString('en-us'))
console.log(a.toLocaleString('zh-cn'))

[object Object],23,abcd,Tue Feb 26 2019 11:47:03 GMT+0800 (中国标准时间)
[object Object],23,abcd,Tue Feb 26 2019 11:47:03 GMT+0800 (中国标准时间)
[object Object],23,abcd,2/26/2019, 11:47:03 AM
[object Object],23,abcd,2019/2/26 上午11:47:03
```

##### concat 合并数组

> 定义： 方法用于合并两个或多个数组，返回一个新数组。
>
> 语法：var newArr =oldArray.concat(arrayX,arrayX,......,arrayX)
>
> 参数：arrayX（必须）：该参数可以是具体的值，也可以是数组对象。可以是任意多个。

```js
let a = [1, 2, 3]
let b = [4, 5, 6]
//连接两个数组
let newVal = a.concat(b) // [1,2,3,4,5,6]
// 连接三个数组
let c = [7, 8, 9]
let newVal2 = a.concat(b, c) // [1,2,3,4,5,6,7,8,9]
// 添加元素
let newVal3 = a.concat('添加元素',b, c,'再加一个')
// [1,2,3,"添加元素",4,5,6,7,8,9,"再加一个"]
// 合并嵌套数组  会浅拷贝嵌套数组
let d = [1,2 ]
let f = [3,[4]]
let newVal4 = d.concat(f) // [1,2,3,[4]]
```

##### ES6扩展运算符 `...` 合并数组

```js
let a = [2, 3, 4, 5]
let b = [ 4,...a, 4, 4]
console.log(a,b) //  [2, 3, 4, 5] [4,2,3,4,5,4,4]
```

##### indexOf() 查找数组是否存在某个元素，返回下标

> 定义: 返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
>
> p.s. 字符串也有此方法，要注意当 'lance'.indexOf('') 一个空字符串时，返回0而不是-1
>
> 语法：array.indexOf(searchElement,fromIndex)
>
> 参数：
>
> searchElement (必须):被查找的元素
>
> fromIndex (可选):开始查找的位置(不能大于等于数组的长度，返回-1)，接受负值，默认值为0。
>
> 严格相等的搜索:
>
> 数组的 indexOf 搜索跟字符串的indexOf不一样,数组的indexOf使用严格相等`===`搜索元素，即**数组元素要完全匹配**才能搜索成功。
>
> **注意**：indexOf() 不能识别`NaN`

```js
let a=['啦啦',2,4,24,NaN]
console.log(a.indexOf('啦'))  // -1
console.log(a.indexOf('NaN'))  // -1
console.log(a.indexOf('啦啦')) // 0
```

##### lastIndexOf() 查找指定元素在数组中的最后一个位置

> 定义: 方法返回指定元素,在数组中的最后一个的索引，如果不存在则返回 -1。（从数组后面往前查找）
>
> 语法：arr.lastIndexOf(searchElement,fromIndex)
>
> 参数:
>
> searchElement(必须): 被查找的元素
>
> fromIndex(可选): 逆向查找开始位置，默认值数组的 `长度-1`，即查找整个数组。
>
> 关于fromIndex有三个规则:
>
> 1. 正值。如果该值大于或等于数组的长度，则整个数组会被查找。
> 2. 负值。将其视为从数组末尾向前的偏移。(比如-2，从数组最后第二个元素开始往前查找)
> 3. 负值。其绝对值大于数组长度，则方法返回 -1，即数组不会被查找。

```js
 let a = ['OB',4,'Koro1',1,2,'Koro1',3,4,5,'Koro1'] // 数组长度为10
 // let b=a.lastIndexOf('Koro1',4) // 从下标4开始往前找 返回下标2
 // let b=a.lastIndexOf('Koro1',100) //  大于或数组的长度 查找整个数组 返回9
 // let b=a.lastIndexOf('Koro1',-11) // -1 数组不会被查找
 let b = a.lastIndexOf('Koro1',-9) // 从第二个元素4往前查找，没有找到 返回-1
```

##### ES7 includes() 查找数组是否包含某个元素 返回布尔

> 定义： 返回一个布尔值，表示某个数组是否包含给定的值
>
> 语法：array.includes(searchElement,fromIndex=0)
>
> 参数：
>
> searchElement (必须):被查找的元素
>
> fromIndex (可选):默认值为 0 ，参数表示搜索的起始位置，接受负值。正值超过数组长度，数组不会被搜索，返回 false 。负值绝对值超过长数组度，重置从 0 开始搜索。
>
>
>
> **includes 方法是为了弥补 indexOf 方法的缺陷而出现的:**
>
> 1. indexOf 方法不能识别 `NaN`
> 2. indexOf 方法检查是否包含某个值不够语义化，需要判断是否不等于 `-1` ，表达不够直观

```js
let a = ['OB','Koro1',1,NaN]
// let b=a.includes(NaN) // true 识别NaN
// let b=a.includes('Koro1',100) // false 超过数组长度 不搜索
// let b=a.includes('Koro1',-3)  // true 从倒数第三个元素开始搜索
// let b=a.includes('Koro1',-100)  // true 负值绝对值超过数组长度，搜索整个数组
```

参考：[js 数组详细操作方法及解析合集](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)

## 字符串

### 字符串的方法

#### charAt 返回字符串字符

> 从一个字符串中返回指定字符
> 如果指定的 index 值超出了该范围，则返回一个空字符串

```js
var anyString = "Brave new world"
console.log(anyString.charAt(0)) // B
```

#### substring 返回字符串子集

> 返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。
>
> - 如果 `indexStart` 等于 `indexEnd`，`substring` 返回一个空字符串。
> - 如果省略 `indexEnd`，`substring` 提取字符一直到字符串末尾。
> - 如果任一参数小于 0 或为 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)，则被当作 0。
> - 如果任一参数大于 `stringName.length`，则被当作 `stringName.length`。
> - 如果 `indexStart` 大于 `indexEnd`，则 `substring` 的执行效果就像两个参数调换了一样。见下面的例子。

```js
var anyString = "Mozilla"
// 输出 "Moz"
console.log(anyString.substring(0,3))
console.log(anyString.substring(3,0))
console.log(anyString.substring(3,-3))
console.log(anyString.substring(3,NaN))
console.log(anyString.substring(-2,3))
console.log(anyString.substring(NaN,3))
```

#### replace 替换字符串

> 返回一个由替换值（replacement）替换一些或所有匹配的模式（pattern）后的新字符串。模式可以是一个字符串或者一个 [正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) ，替换值可以是一个字符串或者一个每次匹配都要调用的回调函数。

#### toLowerCase / toUpperCase 转换字母大小写

字母转为全小写或全大写

#### includes

> includes() 返回布尔值：表示是否找到了参数字符。

```js
let str = "qdywxs"
console.log(str.includes("y"))  //-->true
```

#### repeat

> 获取字符串重复 n 次。

```js
let s = "qdywxs"
console.log(s.repeat(3))  //-->qdywxsqdywxsqdywxs
```

#### startsWith

> 返回布尔值：表示参数字符串是否在源字符串的头部。

```js
console.log("swlance".startsWith("s"));  //-->true
console.log("swlance".startsWith("l"));  //-->false
```

#### endsWith

> 返回布尔值，表示参数字符串是否在源字符串的尾部。

```js
console.log("swlance".endsWith("e"));  //-->true
console.log("swlance".endsWith("o"));  //-->false
```

#### padStart/padEnd

在 ES 8 中String新增了两个实例函数`String.prototype.padStart`和`String.prototype.padEnd`，允许将空字符串或其他字符串添加到原始字符串的开头或结尾。

- `String.padStart(targetLength,[padString])`
  _targetLength：_当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。

  *padString：*(可选)填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断，此参数的缺省值为空格。

- `String.padEnd(targetLength,padString])` 参数释义同上。

```
'es8'.padStart(2);          // 'es8'
'es8'.padStart(5);          // '  es8'
'es8'.padStart(6, '1891');  // '189es8'
'es8'.padStart(14, 'coffe');  // 'coffecoffeces8'
'es8'.padStart(7, '0');     // '0000es8'

'es8'.padEnd(2);            // 'es8'
'es8'.padEnd(5);            // 'es8  '
'es8'.padEnd(6, '1891');    // 'es8189'
'es8'.padEnd(14, 'coffe');    // 'es8coffecoffec'
'es8'.padEnd(7, '9');       // 'es89999'
```

## 什么是 JavaScript 作用链域？

全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。

当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找， 直至全局函数，这种组织形式就是作用域链。

## 闭包

### 什么是闭包/对闭包的理解

函数中有另一个函数或有另一个对象，里面的函数或者是对象都可以使用外面函数中定义的变量或者参数，此时形成闭包。

> YouDontKnowJS对闭包的解释 —— 闭包就是函数能够记住并访问它的词法作用域，即使当这个函数在它的词法作用域之外执行时。由于这个性质，**闭包让我们能够从一个函数内部访问其外部函数的作用域**
>
> 闭包就是能够读取其他函数内部变量的函数。可以简单理解成“定义在一个函数内部的函数”

### 闭包的用途

- **保存**：缓存数据，延长作用域链
- **保护**：形成私有作用域，保护里面私有变量不受外界干扰，避免全局污染

> 缺点：**耗内存，耗性能**，函数中的变量不能及时释放

### 如何使用闭包

要使用闭包，只需要简单地将一个函数定义在另一个函数内部，并将它暴露出来。要暴露一个函数，可以将它返回或者传给其他函数。

**内部函数将能够访问到外部函数作用域中的变量**，即使外部函数已经执行完毕。

想要缓存数据的时候就用闭包，把想要缓存的数据放在外层函数和内层函数的中间位置。

### 闭包应用场景

#### li 节点的 onclick 事件都能正确的弹出当前被点击的 li 索引

```html
<ul id="testUL">
    <li> index = 0</li>
    <li> index = 1</li>
    <li> index = 2</li>
    <li> index = 3</li>
</ul>
<script type="text/javascript">
    var nodes = document.getElementsByTagName("li")
    for (var i = 0; i < nodes.length; i += 1) {
        nodes[i].onclick = (function (i) {
            return function () {     // <----重点是此处返回了个一个匿名函数，这个函数能访问
              // 立即执行函数作用域内的i这个变量，形成闭包
                console.log(i)
            } //不用闭包的话，值每次都是4
        })(i)
    }
</script>
```

#### 用闭包模拟私有方法

使用闭包定义能访问私有函数和私有变量的公有函数（也叫做模块模式）

```js
var counter = (function() {
  var privateCounter = 0
  function changeBy(val) {
    privateCounter += val
  }
  return {
    increment: function() {
      changeBy(1)
    },
    decrement: function() {
      changeBy(-1)
    },
    value: function() {
      return privateCounter;
    }
  }
})() // 立即执行函数

console.log(counter.value()) // logs 0
counter.increment()
counter.increment()
console.log(counter.value()) // logs 2
counter.decrement()
console.log(counter.value()) // logs 1
```

环境中包含两个私有项：名为 privateCounter 的变量和名为 changeBy 的函数。 它俩都无法在匿名函数外部直接访问。必须通过匿名包装器返回的对象的三个公共函数访问。

### 解决 循环+闭包 问题

直接 [点击此处](https://juejin.im/post/58f1fa6a44d904006cf25d22) 查看

## this

### 对 this 的理解

- this 总是指向函数的直接调用者（而非间接调用者）
- 如果有 new 关键字，this 指向 new 出来的那个对象
- 在事件中，this 指向触发这个事件的对象，特殊的是，IE 中的 attachEvent 中的 this 总是指向全局对象 window

**重要**：

- 普通函数的 this 指向是在函数的**执行期间**绑定的
- 箭头函数的 this 指向是在函数**创建期间**就绑定好了的，指向的是创建该箭头函数所在的作用域对象
- 一般不在事件（比如 onclick ）上传递箭头函数，使用 function 就好

### 判定 `this`

> 摘自 YouDontKnowJS

现在，我们可以按照优先顺序来总结一下从函数调用的调用点来判定 `this` 的规则了。按照这个顺序来问问题，然后在第一个规则适用的地方停下。

1. 函数是通过 `new` 被调用的吗（**new 绑定**）？如果是，`this` 就是新构建的对象。
   `var bar = new foo()`
2. 函数是通过 `call` 或 `apply` 被调用（**明确绑定**），甚至是隐藏在 `bind` *硬绑定* 之中吗？如果是，`this` 就是那个被明确指定的对象。
   `var bar = foo.call( obj2 )`
3. 函数是通过环境对象（也称为拥有者或容器对象）被调用的吗（**隐含绑定**）？如果是，`this` 就是那个环境对象。
   `var bar = obj1.foo()`
4. 否则，使用默认的 `this`（**默认绑定**）。如果在 `strict mode` 下，就是 `undefined`，否则是 `global` 对象。
   `var bar = foo()`

以上，就是理解对于普通的函数调用来说的 `this` 绑定规则 *所需的全部*。是的……几乎是全部。

## apply, call, bind 的区别

> apply, call, bind 本身存在于大 Function 构造函数的 prototype 中
>
> 所有的函数都是大 Function 的实例对象

![apply](https://cdn.nlark.com/yuque/0/2019/png/260235/1549791418436-78a38fa1-d824-45c0-8caa-7a6314637140.png)

apply, call, bind 方法都可以改变 this 的指向

- apply(对象, [参数1, 参数2, 餐数3, ...])
- call(对象, 参数1, 参数2, 餐数3,...)
- bind(对象,参数1, 参数2, 餐数3,...)
    - 函数名称.bind()---->返回值是复制之后的这个函数

### 区别

- apply，call 是调用的时候改变 this 指向，然后返回函数执行的结果。
    - 参数较多时用 apply ，参数较少时用 call
- bind 是复制一份函数并返回，并且这个函数的 this 指向变成了传入的第一个参数。

### 手写 bind、apply、call

#### call

```js
Function.prototype.call = function (context, ...args) {
  context = context || window;
  
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  context[fnSymbol](...args);
  delete context[fnSymbol];
}
```

#### apply

```js
Function.prototype.apply = function (context, argsArr) {
  context = context || window;
  
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  context[fnSymbol](...argsArr);
  delete context[fnSymbol];
}
```

#### bind

```js
Function.prototype.bind = function (context, ...args) {
  context = context || window;
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;
  
  return function (..._args) {
    _args = _args.concat(args);
    
    context[fnSymbol](..._args);
    delete context[fnSymbol];   
  }
}
```



## JavaScript 原型

### 什么是原型

> **实例对象**中有个属性 \_\_proto\_\_ ，是个对象，叫原型，不是标准的属性，浏览器使用的----->可以叫**原型对象**
>
> **构造函数**中有一个属性 **prototype** ，也是个对象，叫原型，是标准属性，程序员使用--->可以叫**原型对象**
>
> *实例对象的 \_\_proto\_\_ 和构造函数中的 prototype 相等---> true*
>
> *又因为实例对象是通过构造函数来创建的，构造函数中有原型对象prototype*
>
> *实例对象的 \_\_proto\_\_ 指向了构造函数的原型对象 prototype*

每个对象都会在其内部初始化一个属性，就是 prototype（原型）。原型就是 \_\_proto\_\_（IE8不支持，非标准属性） 或者是 prototype ，都是原型对象。

### 作用

- 共享数据，目的是：节省内存空间
- 实现继承，目的是：节省内存空间

## 原型链

### 什么是原型链

当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，如果原型还没有找到，那么会继续查找原型的原型，直到找到 Object.prototype 的原型时，此时原型为 null，查找停止。这种通过 通过原型链接的逐级向上的查找链被称为原型链。

**原型和原型链**
![prototype](https://cdn.nlark.com/yuque/0/2019/png/260235/1549711479179-356c282c-60db-41ea-82df-5ebfb9550785.png)


**原型链最终指向**
![原型链最终指向](https://cdn.nlark.com/yuque/0/2019/png/260235/1549711526339-9b043225-9ad9-4f88-b1b5-95da79cc4bf8.png)

### 分别使用原型链和 class 的方式实现继承

#### 1. 组合继承（原型链 + 借用构造函数）【不推荐】

```js
function Person(name, age) {
  this.name = name
  this.age = age
}
Person.prototype.sayHi=function () {}

function Student(name, age, weight) {
  // 借用构造函数
  Person.call(this, name, age)
  this.weight = weight
}
// 改变原型指向----继承
// 我们让 Student.prototype 指向一个 Person 的实例对象
// 这个对象的 __proto__ 指向的是 Person.prototype
// 所以我们就可以借助这个实例对象拿到 sayHi 方法，实现继承
Student.prototype = new Person()
Student.prototype.eat = function () {}
var stu = new Student("Lance", 20, 120)
var stu2 = new Student("Will", 200 , 110)

// 属性和方法都被继承了
```

由上面方案引出的问题：

##### 为什么不能 Student.prototype = Person.prototype

对象的赋值只是引用的赋值, 上面两者都指向同一个内存地址，只要随便通过 1 个途径就能修改该内存地址的对象，这样子类就无法单独扩展方法，因为会影响父类。

##### 单纯的原型链继承有什么缺陷

虽然改变了原型的指向，但属性在初始化的时候就已经固定了【Student.prototype = new Person("小明", 29, 90)】，如果是多个对象实例化，那么每个实例对象的属性的初始值就都是一样的。换句话说，无法向父类传递参数。

##### 单纯的借用构造函数继承有什么缺陷

只能继承父类构造函数里面的属性和方法【Person.call(this, name, age)】，但父类的 prototype（原型）上的属性和方法不能继承。

##### 组合继承的缺点

调用了两次父类的构造函数。第一次给子类的原型添加了父类构造函数中的属性方法，第二次又给子类的构造函数添加了父类的构造函数的属性方法，从而覆盖了子类原型中的同名参数。这种被覆盖的情况造成了性能上的浪费：

```js
function SuperType() {
    this.name = 'parent'
    this.arr = [1, 2, 3]
}

SuperType.prototype.say = function() {}

function SubType() {
    SuperType.call(this) // 第二次调用SuperType
}

SubType.prototype = new SuperType() // 第一次调用SuperType
```

#### 2. 寄生组合继承【推荐】

```js
function Person(name, age) {
    this.name = name
    this.age = age
    this.sayHi = function() {}
}

Person.prototype.eat = function() {}

function Student(name, age, weight) {
    Person.call(this, name, age)
    this.weight = weight
    this.study = function() {}
}

var F = function (){}
// 核心：因为是对父类原型的复制，所以不包含父类的构造函数，也就不会调用两次父类的构造函数造成浪费
F.prototype = Person.prototype // 创建了父类原型的浅复制
Student.prototype = new F();
// 上面三段js代码也可以用 Student.prototype = Object.create(Person.prototype) 代替
Student.prototype.constructor = Student // 修正原型的构造函数

var stu1 = new Student("Lance", 19, 120)
console.dir(stu1)
```

#### 3. class 实现继承

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
        // 类本身的方法
        this.sayHi = function() {}
    }
    // 这里的 eat 相当于 prototype 中的 eat
    eat() {}
}

// 关键点：extends super
class Student extends Person {
    constructor(name, age, weight) {
        super(name, age)
        this.weight = weight
        this.study = function() {}
    }
    run() {}
}

var stu = new Student("Jerry", 20, 100)
console.dir(stu)
```

### 原型链，proto 和 prototype 的区别

对象拥有 \_\_proto\_\_ 属性，函数拥有 prototype 属性。某个实例对象的 \_\_proto\_\_ 指向构造它的构造函数的 prototype 属性。所以：实例对象的 \_\_proto\_\_ 指向了构造函数的原型对象 prototype
：

```js
// 构造函数
function B(b) {
  this.b = b
}
// 实例对象
var b = new B('lc')
b.__proto__ === B.prototype  // true
```

参考：[**彻底理解什么是原型链，prototype和__proto__的区别。**](https://blog.csdn.net/lc237423551/article/details/80010100)

## 对象

> 函数（包括构造函数）是对象
>
> 对象不一定是函数
>
> 对象有 \_\_proto\_\_
>
> 函数有 prototype

### 创建对象的三种方式

#### 字面量的方式

```js
var obj = {
  name: "Lance",
  age: 20,
  sayHi: function(){}
}
```

#### 调用系统的构造函数

```js
var obj = new Object()
obj.name = 'Lance'
obj.age = 20
obj.sayHi = function(){}
```

#### 自定义构造函数

```js
function Person(name, age) {
  this.name = name
  this.age = age
  this.sayHi = function(){}
}
```

#### new 操作符具体干了什么呢？

1. 创建一个新对象，如：var obj = {};
2. 新对象的 `__proto__` 属性指向构造函数的原型对象。
3. 将构造函数的作用域赋值给新对象。（也所以this对象指向新对象）
4. 执行构造函数内部的代码，将属性添加给obj中的this对象。
5. 返回新对象obj。

```js
function _new(fn, ...args) {
  const obj = Object.create(fn.prototype)
  const ret = fn.apply(obj, args)
  return ret instanceof Object ? ret : obj
}
```

#### 通过 new 的方式创建对象和通过字面量创建有什么区别？

1. 字面量创建对象，不会调用 Object 构造函数, 简洁且性能更好;
2. new Object() 方式创建对象本质上是方法调用，涉及到在 proto 链中遍历该方法，当找到该方法后，又会生产方法调用必须的 堆栈信息，方法调用结束后，还要释放该堆栈，性能不如字面量的方式

## 事件

### 什么是事件，IE与火狐的事件机制有什么区别？ 如何阻止冒泡？

1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被  JavaScript 侦测到的行为。
2. 事件处理机制：IE9以下只支持事件冒泡、Firefox、Chrome等则同时支持两种事件模型，也就是：捕获型事件和冒泡型事件。
3. ev.stopPropagation();（旧 ie 的方法 ev.cancelBubble = true）

### 事件流

![事件阶段.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/260235/1550160415749-0297296f-0559-485f-bfe4-05859bdc757a.jpeg)

事件流是网页元素接收事件的顺序，"DOM2级事件"规定的事件流包括三个阶段：

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段

首先发生的事件捕获，为截获事件提供机会。

然后是实际的目标接受事件。

最后一个阶段是时间冒泡阶段，可以在这个阶段对事件做出响应。

虽然捕获阶段在规范中规定不允许响应事件，但是实际上还是会执行，所以有两次机会获取到目标对象。

### 什么是DOM事件模型

主要有三种 DOM 事件模型：

1. **事件冒泡**：元素先监听到这个事件，往外传播；
2. **事件捕获**：Document 最先监听到这个事件；
3. **DOM 事件流**：先捕获，再冒泡。允许在某些地方监听事件，假设在某个元素两阶段都绑定一个事件监听器，看你是决定在左边捕获阶段监听，还是右边冒泡阶段监听，哪一边有响应，哪一边事件就被触发。即可以在任一阶段、任意时机监听事件。

### 事件绑定的三种方式

```js
// 【 DOM0 级事件 】
// 第一种：作为属性，写在标签上
// <div onclick="fun();">click</div> ← 绑定在事件冒泡阶段

// 第二种，使用 onclick
document.getElementById("xxx").onclick = function(){}   // ← 绑定在事件冒泡阶段


// 【 DOM2 级事件 】
// 第三种：使用推荐的标准模式
document.getElementById("xxx").addEventListener("click", function(e){}, false)
// 第三种可以改变事件绑定的阶段
// ---> 为 false 时，绑定在事件冒泡阶段（默认下是绑定在冒泡阶段）
// ---> 为 true 时，绑定在捕获阶段

// 如果绑定在捕获阶段，监听函数就只在捕获阶段触发
// 如果绑定在冒泡阶段，监听函数只在冒泡阶段触发。
```

### DOM2 事件传播机制

DOM 作为事件处理的一个基本标准，DOM2 级事件为 DOM 事件的升级；

DOM2 级事件定义用于处理指定和删除事件处理程序的操作：

- addEventListener
- removeEventListener

所有的 DOM 节点都包含这两个方法，并且它们都接受三个参数：

1. 事件类型；
2. 事件处理方法；
3. 布尔参数。如果是 true 表示在捕获阶段调用事件处理程序；如果是 false，则是在事件冒泡阶段处理。

### 事件的执行顺序

- 无论是哪种绑定方式，对于同一个绑定元素，都是遵循先绑定的先执行原则。
- 如果是以 onclick 的方式绑定的，如果对同一个元素重复绑定的话，后面的会覆盖前面的。但是如果是以 addEventListener 方式绑定的话，同一个元素绑定多少次，就会执行多少次。
- 如果在 DOM 中直接使用 onclick ，则 onclick 的绑定是早于 addEventListener 的。

#### 我们给一个 dom 同时绑定两个点击事件，一个用捕获，一个用冒泡。会执行几次事件，会先执行冒泡还是捕获？

会执行两次事件，按代码执行顺序来

**规律**：绑定在被点击元素的事件是按照代码顺序发生，其他元素通过冒泡或者捕获“感知”的事件，按照[W3C](https://www.baidu.com/s?wd=W3C&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)的标准，先发生捕获事件，后发生冒泡事件。所有事件的顺序是：其他元素捕获阶段事件 -> **本元素代码顺序事件** -> 其他元素冒泡阶段事件

#### 小练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件冒泡</title>
</head>
<body>
    <div>
        <p id="parEle">我是父元素    <span id="sonEle">我是子元素</span></p>
    </div>
</body>
</html>
<script type="text/javascript">
var sonEle = document.getElementById('sonEle');
var parEle = document.getElementById('parEle');

parEle.addEventListener('click', function () {
    alert('父级 冒泡');
}, false);
parEle.addEventListener('click', function () {
    alert('父级 捕获');
}, true);

sonEle.addEventListener('click', function () {
    alert('子级冒泡');
}, false);
sonEle.addEventListener('click', function () {
    alert('子级捕获');
}, true);

</script>
```

上述代码父子事件的执行顺序是什么？

解析：

当容器元素及嵌套元素，即在`捕获阶段`又在`冒泡阶段`调用事件处理程序时：**事件按DOM事件流的顺序**执行事件处理程序：

- 父级捕获
- 子级冒泡
- 子级捕获
- 父级冒泡

且当事件处于目标阶段时，事件调用顺序决定于绑定事件的**书写顺序**，按上面的例子为，先调用冒泡阶段的事件处理程序，再调用捕获阶段的事件处理程序。依次alert出“子集冒泡”，“子集捕获”。

### 事件委托

绑定在父级元素，利用事件冒泡去触发父级事件处理函数的一种技巧。

#### 实现一个事件委托

```js
var ul = document.querySelector('ul')

function listen(element, eventType, targetElement, fn) {
    element.addEventListener(eventType, function(e) {
        // 先拿到当前事件的直接触发对象
        var curTarget = e.target
        // 看它是不是使用者监听的目标对象类型
        // 一旦发现不是，就执行循环
        while(!curTarget.matches(targetElement)) {
            // 先看看当前对象是不是和父元素相同
            // 相同则把当前对象置为空，且不执行回调
            if (curTarget === element) {
                curTarget = null
                break
            }
            // 不相同则把当前对象设置成自己的父对象
            curTarget = curTarget.parentNode
        }
        // 是，则先看当前对象有没有值，有值则执行回调函数
        curTarget && fn(e, curTarget, e)
    })
}

listen(ul, 'click', 'li', function(event, el) {
    console.log(event, el)
});

// jquery 使用方式
$("ul").on("click", "li", function(e) {
  console.log($(e.target).html());
});
// 这个 on 事件是绑定在 ul 上面的，li 是目标元素，
// on 事件内部是通过 e.target 来判断点击元素是不是 li 的
```

### 解释以下概念：事件传播机制、阻止传播、取消默认事件、事件代理？

**1. 事件传播机制主要有三种：**

- 事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的元素（从小到大）；
- 事件捕获：不太具体的节点更早接收事件，而最具体的元素最后接收事件，和事件冒泡相反（从大到小）；
- DOM事件流：DOM2 级事件规定事件流包括三个阶段，事件捕获阶段，处于目标阶段，事件冒泡阶段，首先发生的是事件捕获，为截取事件提供机会，然后是实际目标接收事件，最后是冒泡阶段（标准浏览器现在都使用 DOM 事件流，IE 不支持事件流，只支持事件冒泡）。

**2. 阻止传播**

stopPropagation() 取消事件进一步捕获或冒泡。

**3. 取消默认事件**

preventDefault() 取消事件默认行为。

**4. 事件代理**

通过事件冒泡（或者事件捕获）给父元素添加事件监听，e.target 指向引发触发事件的元素。

### 什么是节流和防抖

#### 介绍

- debounce（防抖）的作用是在让在用户动作停止后延迟x ms再执行回调
- throttle（节流）的作用是在用户动作时每隔一定时间（如200ms）执行一次回调

#### 节流防抖作用

- debounce 应用在搜索框的即时搜索（input 事件），避免用户狂按键盘导致的频繁请求
- throttle 应用在监听 resize 改变布局或 onscroll 滚动

**防抖**：

```js
// <input type="text" oninput="change()">
// 防抖（一段时间会等，然后带着一起做了）

function debounce(fn, delay) {
  let timer = null
  return function() {
    const context = this, args = arguments
    if (timer) {
        window.clearTimeout(timer)
    }
    timer = setTimeout(() => {
      fn.apply(context, args)
      timer = null
    }, delay)
  }
}

var change = debounce(function() {
  var input = document.querySelector("input")
  console.log(input.value)
}, 1000)
```

**节流**：

```js
// <div class="sw">23333</div>
// 节流（一段时间执行一次之后，就不执行第二次）

function throttle(fn, delay) {
  let canUse = true
  return function() {
    var context = this, args = arguments
    if (canUse) {
      fn.apply(context, args)
      canUse = false
      setTimeout(()=>canUse = true, delay)
    }
  }
}

var sw = document.querySelector(".sw")

sw.addEventListener("click", throttle(function(e) {
    console.log(e)
}, 1000))
```

参考：

- [在React、Vue和小程序中使用函数节流和函数防抖](https://blog.csdn.net/qq_37860930/article/details/83545473‘)
- [函数防抖与函数节流](https://zhuanlan.zhihu.com/p/38313717)
- [JS事件中防抖debounce和节流throttle概念原理的学习](http://www.webfront-js.com/articaldetail/99.html)

### mouseover 和 mouseenter 的区别

- mouseover：
  不论鼠标指针穿过被选元素或其子元素，都会触发 `mouseover` 事件。
  支持事件冒泡。
  相对应 `mouseout` 事件。
- mouseenter：
  只有在鼠标指针穿过被选元素时，才会触发 `mouseenter` 事件。
  不支持事件冒泡。
  相对应 `mouseleave` 事件。

参考：[JavaScript中的 mouseover 与 mouseenter ，mouseout 和 mouseleave 的区别](https://blog.csdn.net/u010297791/article/details/57412796)

### window.onload 和 document.onDOMContentLoaded 有什么区别？

```js
//当页面所有资源加载完成，则触发（涉及到所有资源，所以触发时机较晚）。
window.onload = function() {
  console.log("window loaded");
};

//DOM 结构解析完成（并不是页面上资源加载完成，而是 dom 结构渲染完成）。
document.addEventListener("DOMContentLoaded", function() {
  console.log("DOMContentLoaded");
});
```

### <script src=’xxx’ ’xxx’/>外部js文件先加载还是onload先执行

onload 是所有加载完成之后执行的

## ES6相关

### ES6 用到过吗，新增了哪些东西，你用到过什么？

- `let` 和 `const`
- 模板字符串
- 箭头函数（自己没有 this ，从自己的作用域链的``上一层继承 this`` ）
- `for-of`（用来遍历数据—例如数组中的值）e.g. Array，String，Set，Map
- `arguments` 对象可被不定参数和默认参数完美代替
- Promise
- 数组的拓展
    - 数组.find((item,index,arr) => {条件}) 返回满足条件的第一个元素的值。否则返回 undefined
    - 数组.findIndex((item,index,arr)=>{...}) 返回满足条件的第一个元素的索引值。否则返回 -1
    - 数组.includes(数据,[searchIndex]) 判断数据是否在数组中,第二个参数(可选参数)为从指定索引处(包含索引处的值)开始搜索 返回布尔值(es7时加入)
    - 扩展运算符 ...

- 引入 `module` 模块的概念

### const 和 let 区别

- let 定义变量可以只声明不赋值
- const 定义常量声明时必须赋值，一旦定义不可轻易改变值

#### 共同点

解决 var 没有块作用域、变量提升、可以重复声明的问题。let 和 const 有自己的块作用域，不存在变量提升问题，同一块作用域中不可重复声明（会报错）

#### let var 区别

- var 有变量提升，let 没有
- let 的作用域是块，而 var 的作用域是函数

    ```js
    var a = 5
    var b = 10

    if (a === 5) {
    let a = 4 // The scope is inside the if-block
    var b = 1 // The scope is inside the function

    console.log(a)  // 4
    console.log(b)  // 1
    }

    console.log(a) // 5
    console.log(b) // 1
    ```

- let 有暂时性死区，只要 let/const 声明的变量，在未声明之前使用或者赋值都会报错（ReferenceError）
- let 不能重复定义

#### 可以改变 const 定义的某个对象的属性吗

可以，因为对象是复杂类型，const 存储的是引用，所以改变对象的成员不会报错，但不建议这样做。

### 箭头函数（ this 指向）

- 箭头函数，本质上，就是一个匿名函数
- 箭头函数无法通过 call、apply、bind 来手动改变内部 this 指向
- 箭头函数：自动 .bind(this) 也就是说箭头函数中的 this 指向与其所在作用域的 this 指向相同

**总结**：

箭头函数不会创建自己的 ``this`` ，它只会从自己的作用域链的 `上一层继承 this`

```js
function Person() {
  // Person() 构造函数定义 `this` 作为它自己的实例.
  this.age = 0

  setInterval(function growUp() {
    // 在非严格模式, growUp() 函数定义 `this`作为全局对象, 
    // 与在 Person() 构造函数中定义的 `this`并不相同.
    this.age++
  }, 1000)
}

var p = new Person()

// 使用箭头函数
function Person() {
  this.age = 0

  setInterval(() => {
    this.age++ // |this| 正确地指向 p 实例
  }, 1000)
}

var p = new Person()
```

#### 箭头函数与普通函数的区别

1. 箭头函数是匿名函数，不能作为构造函数，不能使用 new。

```javascript
var B = () => {
  value:1;
}
var b = new B();  //-->TypeError: B is not a constructor
```

2. 箭头函数不绑定 arguments，取而代之用 rest 参数 ... 解决。

```javascript
function A(a) {
  console.log(arguments);
}

var B = (b) => {
  console.log(arguments);
}

//...c 即为 rest 参数
var C = (...c) => {
  console.log(c);
}
A(1);  //-->[object Arguments] {0: 1}
B(2);  //-->ReferenceError: arguments is not defined
C(3);  //-->[3]
```

3. 箭头函数不绑定 this，会捕获其所在的上下文的 this 值，作为自己的 this 值。

```javascript
var obj = {
  a: 10,
  b: function() {
    console.log(this.a);
  },
  c: function() {
     return () => {
       console.log(this.a);
     }
  }
}
obj.b();  //-->10
obj.c()();  //-->10
```

4. 箭头函数通过 call()  或  apply() 方法调用一个函数时，只传入了一个参数，对 this 并没有影响。

```javascript
var obj = {
  a: 10,
  b: function(n) {
    var f = (v) => v + this.a;
    return f(n);
  },
  c: function(n) {
    var f = (v) => v + this.a;
    var m = {a:20};
    return f.call(m,n);
  }
}

console.log(obj.b(1));  //-->11
console.log(obj.c(1));  //-->11
```

5. 箭头函数没有原型属性。

```javascript
var a = () => {
  return 1;
}
function b() {
  return 2;
}
console.log(a.prototype);  //-->undefined
console.log(b.prototype);  //-->object{...}
```

6. 箭头函数不能当做 Generator 函数,不能使用 yield 关键字。箭头函数不能换行。

```javascript
var a = ()
          => 1;  //-->SyntaxError: Unexpected token =>
```

### Set 和 Map

#### 概念

- Set 是有序列表，类似于数组，但是没有重复值
- Map 是存储许多键值对的有序列表，key 和 value 支持所有数据类型

#### 相同点

- 都是有序列表
- Set 值不重复；Map 键不重复

#### 用法

> SET

- 属性：
    - `Set.prototype.constructor`：构造函数，默认就是 Set 函数
    - `Set.prototype.size`：返回实例的成员总数
- 操作方法：
    -  `add(value)`：添加一个值，返回Set结构本身
    - `delete(value)`：删除某个值，返回布尔值
    - `has(value)`：返回布尔值，表示是否是成员
    - `clear()`：清除所有成员，无返回值

- 遍历方法（ key() 和 values() 行为是一致的。）
    - `keys()`：返回键名的遍历器（什么是遍历器？Iterator）
    - `values()`：返回键值的遍历器
    - `entries()`：返回键值对的遍历器
    - `forEach()`：使用回调函数遍历每个成员

> MAP

- 属性
    - `size` ：返回 Map 结构的成员总数。
- 操作方法
    - `set(key, value)`: `set` 方法设置键名 `key` 对应的键值为 `value` ，然后返回整个 Map 结构。
    - `get(key)` ：`get` 方法读取 `key` 对应的键值，如果找不到 `key` ，返回 `undefined` 。
    - `has(key)`：`has` 方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
    - `delete(key)`：`delete` 方法删除某个键，返回 `true` 。如果删除失败，返回 `false` 。
  - `clear()`：`clear`方法清除所有成员，没有返回值。

- 遍历方法
    - `keys()`：返回键名的遍历器。
    - `values()`：返回键值的遍历器。
    - `entries()`：返回所有成员的遍历器。
    - `forEach()`：遍历 Map 的所有成员。

#### 用途

Set 集合可以用来过滤数组中重复的元素，只能通过 has 方法检测指定的值是否存在，或者是通过 forEach 处理每个值。

Map 集合通过 set() 添加键值对，通过 get() 获取键值，各种方法的使用查看文章教程，你可以把它看成是比 Object 更加强大的对象。

#### Set 与 数组 的区别

set不可重复，array可重复

#### Map 与 对象 的区别

- Object 的键只能是字符串或者 symbol ，Map 的键可以是任意类型的值（包括对象）
- Map 可以通过 size 获取元素个数，对象得遍历。
- Map 是有序的（根据用户插入的顺序进行排序），对象排序有自己规则（比如先排数字开头的 key ，再到字符串）
- `Map` 可直接进行迭代，而 `Object` 的迭代需要先获取它的键数组，然后再进行迭代。

```js
var map = new Map()
map.set("name", "Lance")
map.set("age", 18)

var per = {
    name: 'Jerry',
    age: 19
}

for (const attr of map.values()) {
    console.log(attr)
} // Lance 18

for (const attr of Object.keys(per)) {
    console.log(per[attr])
} // Jerry 19
```

### Promise

#### promise 是什么

Promise 是异步编程的一种解决方案，比传统的异步解决方案【回调函数】和【事件】更合理、更强大。

#### 诞生的原因

异步网络请求的回调地狱，而且我们还得对每次请求的结果进行一些处理，代码会更加臃肿。

#### promise 使用场景有哪些

- ajax请求得到返回值的时间不同,有了 callback 的回调结果之后才能知道接下来应该做什么
- node 中读取文件

#### 三种状态

- pending: 初始状态，既不是成功，也不是失败状态。
- fulfilled: 意味着操作成功完成。
- rejected: 意味着操作失败。

#### 优缺点

**Promise 的优点:**

- 一旦状态改变，就不会再变，任何时候都可以得到这个结果；
- 可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

**Promise 的缺点:**

- 无法取消 Promise，错误需要通过回调函数来捕获；
- 当处于 pending 状态时，无法得知目前进展到哪一个阶段。

#### 基本用法

```js
//defined Promise async function
function asyncFun() {
    return new Promise((resolve, reject) => {
        if(resolve) {
            resolve(/*resolve parameter*/)
        }else{
            reject(new Error(/*Error*/))
        }
    })
}

//use Promise&then
asyncFun().then(/*function*/).then(/*function*/)...
```

#### promise 特性

参考：[八段代码彻底掌握 Promise](https://juejin.im/post/597724c26fb9a06bb75260e8)

#### promise 里面 return 一个 string，和在 resolve 一个 string 的区别

return 一个 string 后续的 then 不会执行; resolve 一个 string 会返回一个 promise 对象，对象的值是这个 string

#### 在 then 里面 throw 一个 error，怎么捕捉

```js
// throw 这个 error 后，在紧挨的下一个 then 中添加两个回调方法（ resolve 的，和 reject ）。
// 然后在第二个 reject 方法中可以捕获
new Promise(function (resolve, reject) {
        return resolve("返回Promise")
    })
    .then(data => {
        console.log("第一个then")
        throw new Error("我是错误")
    })
    .then(resolve => {
        console.log("成功")
        console.log(resolve)
    }, err => {
        console.log("紧挨的失败")
        console.log(err)
    })
    .catch(err => {
        console.log("catch错误")
        console.log(err)
    });
// 123
// 第一个then
// 紧挨的失败
// Error: 我是错误
// at Promise.then.data (<anonymous>:6:15)

// 在 then 的链式调用后添加一个 catch 来捕获
new Promise(function (resolve, reject) {
    return resolve("返回Promise")
}).then(data => {
    console.log("第一个then")
    throw new Error("我是错误")
}).then(err => {
    console.log("then错误")
    console.log(err)
}).catch(err => {
    console.log("catch错误")
    console.log(err)
})

console.log("123")

// 123
// 第一个 then
// catch 错误
// Error: 我是错误
// at Promise.then.data
```

#### 使用 Promise 封装一个 url

```js
var url = ''
var getJSON = url => new Promise((resolve, reject) => {
    var xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.send(null)
    xhr.onreadystatechange = () => {
        if (xhr.readyState === 4) { // <= 请求已完成，且响应已就绪
            if (xhr.status === 200) { // <= 状态OK
                resolve(JSON.parse(xhr.responseText))
            } else {
                reject(new Error("请求失败"))
            }
        }
    }
})

getJSON(url).then(res => console.log(res))
```

#### 在 Promise 链式调用中，怎样才能保证上一个 promise 出现报错不会影响到后续 .then 的正常执行

为了不影响后续 .then 的执行，需要在每一个 then 中指定失败的回调

```js
let asyncFunc = () => new Promise(resolve => {
  resolve("123")                 // 123, 二楼
  // throw new Error("出错了")    // Error: 出错了, 二楼
});

asyncFunc().then(res => {
  console.log(res)
  return Promise.resolve("二楼")
}, err => {                      // <====== 指定失败的回调
  console.log(err)
    return Promise.resolve("二楼")
}).then(res => {
  console.log(res)
})
```

#### Promise 构造函数是同步执行还是异步执行，那么 then 方法呢？

```js
Promise new的时候会立即执行里面的代码 then是微任务 会在本次任务执行完的时候执行 setTimeout是宏任务 会在下次任务执行的时候执行
```

### Async / Await

#### 是什么

- async 用于声明一个异步的 function
- await 用于等待一个异步方法执行完成

Async/Await 是一种用于处理 JS 异步操作的语法糖，可以帮助我们摆脱回调地狱（callback hell），编写更加优雅的代码。

通俗的理解，async 关键字的作用是告诉编译器对于标定的函数要区别对待。当编译器遇到标定的函数中的 await 关键字时，要暂时停止运行，等到 await 标定的函数处理完毕后，再进行相应操作。如果该函数 fulfiled 了，则返回值是 fulfillment value，否则得到的就是 reject value。

#### Async

async 函数会返回一个 Promise 对象，如果在函数中 `return` 一个直接量，async 会把这个直接量通过 `Promise.resolve()` 封装成 Promise 对象。

#### Await

await 是在等待一个 async 函数完成。不过按 [语法说明](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/await) ，await 等待的是一个表达式，这个表达式的计算结果是 Promise 对象或者其它值

```js
function getSomething() {
    return "something"
}

function testAsync() {
    return Promise.resolve("hello async")
}

async function test() {
    const v1 = await getSomething()
    const v2 = await testAsync()
    console.log(v1, v2)
}

test()
```

#### 优势

让代码更易读

假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。

传统 promise ，链式调用 then 一个接一个

改用 async/await 后就像同步代码一样

```js
function doIt() {
    console.time("doIt")
    const time1 = 300
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`)
            console.timeEnd("doIt")
        });
}

doIt()

// =======

async function doIt() {
    console.time("doIt");
    const time1 = 300
    const time2 = await step1(time1)
    const time3 = await step2(time2)
    const result = await step3(time3)
    console.log(`result is ${result}`)
    console.timeEnd("doIt")
}

doIt()
```

参考：[理解 JavaScript 的 async/await](https://segmentfault.com/a/1190000007535316)

最后拿普通的 promise 写法来和 async/await 对比，便于理解：

```js
async function asyncFunc() {
  const result = await otherAsyncFunc(); // otherAsyncFunc()返回一个Promise对象
  console.log(result);
}

// 等同于:
function asyncFunc() {
  return otherAsyncFunc() // otherAsyncFunc()返回一个Promise对象
    .then(*result* => {
      console.log(result);
    });
}
```

按顺序处理多个异步函数的时候优势更为明显：

```js
async function asyncFunc() {
    const result1 = await otherAsyncFunc1();// otherAsyncFunc1()返回一个Promise对象
    console.log(result1);
    const result2 = await otherAsyncFunc2();// otherAsyncFunc2()返回一个Promise对象
    console.log(result2);
}

// 等同于:
function asyncFunc() {
    return otherAsyncFunc1()// otherAsyncFunc1()返回一个Promise对象
    .then(result1 => {
        console.log(result1);
        return otherAsyncFunc2();// otherAsyncFunc2()返回一个Promise对象
    })
    .then(result2 => {
        console.log(result2);
    });
}
```

并行处理多个异步函数：

```js
async function asyncFunc() {
    const [result1, result2] = await Promise.all([
        otherAsyncFunc1(),// otherAsyncFunc1()返回一个Promise对象
        otherAsyncFunc2() // otherAsyncFunc2()返回一个Promise对象
    ]);
    console.log(result1, result2);
}

// 等同于:
function asyncFunc() {
    return Promise.all([
        otherAsyncFunc1(),// otherAsyncFunc1()返回一个Promise对象
        otherAsyncFunc2() // otherAsyncFunc2()返回一个Promise对象
    ])
    .then([result1, result2] => {
        console.log(result1, result2);
    });
}
```

处理错误：

```js
async function asyncFunc() {
    try {
        await otherAsyncFunc();// otherAsyncFunc()返回一个Promise对象
    } catch (err) {
        console.error(err);
    }
}

// 等同于:
function asyncFunc() {
    return otherAsyncFunc()// otherAsyncFunc()返回一个Promise对象
    .catch(err => {
        console.error(err);
    });
}
```



### 解构赋值

#### 数组用法

```js
// 前后的形式必须完全一致 才可以完成结构赋值
let [foo, [[bar], baz]] = [1, [[2], 3]]
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"]
third // "baz"

let [x, , y] = [1, 2, 3]
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4]
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a']
x // "a"
y // undefined
z // []
// 如果解构不成功，变量的值就等于undefined。
```

#### 对象用法

```js
// 对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
{name, age} = {name:'wang',age: 18}
name // wang
age // 18

// 支持别名
{name:nname, age} = {name:'wang',age: 18}
name // '' 取别名时原名就会为空字符串
nname // wang
age // 18
```

#### ...运算符

```js
[a, ...b] = [1, 2, 3, 4, 5]
a // 1
b // [2,3,4,5]

var app = (...sum) => {sum.forEach(item => console.log(item))}
app(1,2,3,4) // 1,2,3,4
// 此运算符或得值为数组形式 主要用于替代函数中的 arguments(伪数组) 属性
// 这样可以非常方便的遍历获取到的未知个数的实参
```

### 函数中的 rest（剩余）参数

> **剩余参数**语法允许我们将一个不定数量的参数表示为一个数组。
>
> ——> MDN - [剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)

```js
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current
  });
}

console.log(sum(1, 2, 3))
// expected output: 6

console.log(sum(1, 2, 3, 4))
// expected output: 10

// 下例中，剩余参数包含了从第二个到最后的所有实参，然后用第一个实参依次乘以它们
function multiply(multiplier, ...theArgs) {
  return theArgs.map(function (element) {
    return multiplier * element
  });
}

var arr = multiply(2, 1, 2, 3)
console.log(arr)  // [2, 4, 6]
```

#### 剩余参数和 arguments 对象的区别

剩余参数和 `arguments` 对象之间的区别主要有三个：

- 剩余参数只包含那些没有对应形参的实参，而 `arguments` 对象包含了传给函数的所有实参。
- `arguments` 对象不是一个真正的数组，而剩余参数是真正的 `Array` 实例，也就是说你能够在它上面直接使用所有的数组方法，比如 `sort`，`map`，`forEach` 或 `pop` 。
- `arguments` 对象还有一些附加的属性 （如 `callee` 属性）。
    - arguments.callee 属性包含当前正在执行的函数。

### 模块化

#### 模块化发展

无模块化 --> CommonJS规范 --> AMD规范 --> CMD规范 --> ES6模块化

#### 无模块劣势

```js
<script src="jquery.js"></script>
<script src="jquery_scroller.js"></script>
<script src="main.js"></script>
...

缺点：
被依赖的放在前面，否则使用就会报错
污染全局作用域
维护成本高
依赖关系不明显
```

#### CommonJS规范（NodeJS）

```js
// 定义模块math.js
var basicNum = 0
function add(a, b) {
  return a + b
}
module.exports = { //在这里写上需要向外暴露的函数、变量
  add,
  basicNum
}

// 引用自定义的模块时，参数包含路径，可省略.js
var math = require('./math')
math.add(2, 5)

// 引用核心模块时，不需要带路径
var http = require('http')
http.createService(...).listen(3000)
```

**exports 是对 module.exports 的引用**。比如我们可以认为在一个模块的顶部有这句代码： `exports = module.exports` 所以，我们不能直接给 `exports` 赋值:

- ✅ exports.foo = 'bar'
- ❌ exports = {foo: 'bar'} //error 这种方式是错误的，相当于重新定义了 exports

**优点**

解决了依赖、全局变量污染的问题

**缺点**

CommonJS 用同步的方式加载模块，这对服务器端不是一个问题，因为所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间。但是，对于浏览器却是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于"假死"状态。所以**不适合浏览器端模块加载**，更合理的方案是使用异步加载。

#### AMD规范（RequireJS）

AMD规范采用**异步方式加载模块**，模块的加载不影响它后面语句的运行。所有**依赖这个模块的语句，都定义在一个回调函数中**，等到加载完成之后，这个回调函数才会运行。

```js
/** 网页中引入 require.js 及 main.js **/
<script src="js/require.js" data-main="js/main"></script>

/** main.js 入口文件/主模块 **/
// 首先用 config() 指定各模块路径和引用名
require.config({
  baseUrl: "js/lib",
  paths: {
    "jquery": "jquery.min",  //实际路径为js/lib/jquery.min.js
    "underscore": "underscore.min",
  }
});
// 执行基本操作
require(["jquery","underscore"], function($,_){
  // some code here
});
```

**优点**

适合在浏览器环境中异步加载模块、并行加载多个模块

**缺点**

必须要**提前加载所有的依赖**，然后才可以使用，而不是需要使用时再加载。（**不能按需加载**）

#### CMD（SeaJS）

与AMD类似，不同点在于：

- AMD 推崇依赖前置、提前执行
- CMD 推崇依赖就近、延迟执行。

**CMD 与 AMD 区别**

```js
/** AMD写法 **/
define(["a", "b", "c", "d", "e", "f"], function(a, b, c, d, e, f) {
     // 等于在最前面声明并初始化了要用到的所有模块
    a.doSomething()
    if (false) {
        // 即便没用到某个模块 b，但 b 还是提前执行了
        b.doSomething()
    }
})

/** CMD写法 **/
define(function(require, exports, module) {
    var a = require('./a') //在需要时申明
    a.doSomething()
    if (false) {
        var b = require('./b');
        b.doSomething()
    }
})
```

#### ES6模块化

ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，旨在成为**浏览器和服务器通用**的模块解决方案。其模块功能主要由两个命令构成： `export` 和 `import` 。 `export` 命令用于规定模块的对外接口，`import` 命令用于输入其他模块提供的功能。

```js
/** 定义模块 math.js **/
var basicNum = 0
var add = function (a, b) {
    return a + b
};
export { basicNum, add }

/** 引用模块 **/
import { basicNum, add } from './math'
function test(ele) {
    ele.textContent = add(99 + basicNum)
}
```

es6 在导出的时候有一个默认导出， `export default` ，使用它导出后，在 import 的时候，不需要加上 {} ，模块名字可以随意起。该名字实际上就是个对象，包含导出模块里面的函数或者变量。

```js
/** export default **/
//定义输出
export default { basicNum, add }
//引入
import math from './math'
function test(ele) {
    ele.textContent = math.add(99 + math.basicNum)
}
```

参考：

- [前端模块化：CommonJS,AMD,CMD,ES6](https://juejin.im/post/5aaa37c8f265da23945f365c)
- [这一次，我要弄懂javascript的模块化](https://juejin.im/post/5b4420e7f265da0f4b7a7b27)

#### 模块化开发怎么做？

[立即执行函数](http://benalman.com/news/2010/11/immediately-invoked-function-expression/),不暴露私有成员

```js
var module1 = (function(){
  var _count = 0
  var m1 = function(){
    //...
  }
  var m2 = function(){
    //...
  }
  return {
    m1,
    m2
  }
})();
```

## 异步

### Ajax

#### Ajax 是什么? 如何创建一个Ajax？

使用 JavaScript 异步获取数据，而且页面不会发生整页刷新的，提高了用户体验。

1. 创建 XMLHttpRequest 对象,也就是创建一个异步调用对象
2. 创建一个新的 HTTP 请求,并指定该 HTTP 请求的方法、URL 及验证信息
3. 设置响应 HTTP 请求状态变化的函数
4. 发送 HTTP 请求
5. 获取异步调用返回的数据
6. 使用 JavaScript 和 DOM 实现局部刷新

```js
// 1. 创建一个 XMLHttpRequest 类型的对象 —— 相当于打开了一个浏览器
var xhr = new XMLHttpRequest()

// 2. 打开与一个网址之间的连接 —— 相当于在地址栏输入访问地址
xhr.open('GET', 'news/list?type=gn')

// 3. 通过连接发送一次请求 —— 相当于回车或者点击访问发送请求
xhr.send(null)

// POST请求：
// xhr.open('POST', 'login', true)
// xhr.send('username=admin&password=admin')

// 4. 指定 xhr 状态变化事件处理函数 —— 相当于处理网页呈现后的操作
xhr.onreadystatechange = function () {
    // 通过 xhr 的 readyState 判断此次请求的响应是否接收完成
    // 4代表done
    if (this.readyState === 4) {
        // 通过 xhr 的 responseText 获取到响应的响应体
        console.log(this)
    }
}
```

#### Ajax 原理

Ajax 的原理简单来说是在用户和服务器之间加了—个中间层（Ajax 引擎），通过 XMLHttpRequest 对象来向服务器发异步请求，从服务器获得数据，然后用 JavaScript 来操作 DOM 而更新页面。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据

Ajax 的过程只涉及 JavaScript、XMLHttpRequest 和 DOM。XMLHttpRequest 是 Ajax 的核心机制。

#### Ajax 解决浏览器缓存问题？

1. 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")
2. 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")
3. 在URL后面加上一个随机数： "fresh=" + Math.random()
4. 在URL后面加上时间戳："nowtime=" + new Date().getTime()

### 什么是同源策略？

同源策略是浏览器的一种安全策略，所谓同源是指 **域名，协议，端口** 完全相同，只有同源的地址才可以相互通过 AJAX 的方式请求。

### 如何解决跨域问题

#### jsonp

借助于 `script` 标签发送跨域请求的技巧

**原理**

css，script 标签允许跨域。客户端借助 `script` 标签请求服务端的一个动态网页（php 文件），服务端的这个动态网页返回一段带有函数调用的 JavaScript 全局函数调用的脚本，将原本需要返回给客户端的数据传递进去。

客户端：

- foo(...arr) {console.log(arr.join(","))} 定义方法，名称随便
- <script src="http://api.zce.me/users.php?callback=foo"></script>
    - 服务端会获取参数名 callback 的值 foo ，然后把数据扔进 foo 中调用
    - 一旦数据返回，就相当于在调用上面的 foo

服务端：

- foo(['我', '是', '你', '原', '本', '需', '要', '的', '数', '据'])

**特色**

1. JSONP 需要服务端配合，服务端按照客户端的要求返回一段 JavaScript 调用客户端的函数
2. 只能发送 GET 请求

#### CORS 跨域资源共享

服务端设置：

```js
// 服务端请求头设置：允许远端访问
header('Access‐Control‐Allow‐Origin: *');

// 这种方案无需客户端作出任何变化（客户端不用改代码），只是在被请求的服务端响应的时候添加一个  
// Access-Control-Allow-Origin  的响应头，表示这个资源是否允许指定域请求。
```

如果跨域 + 发送 cookie：

- 前端：withCredentials = true
- 后端：Access-Control-Allow-Origin 不为 * ，(Access-Control-Allow-Credentials, true)
- 如果还需要发送 post 请求：
  - 前端：
    - post:['Content-Type'] = 'application/x-www-form-urlencoded';
    - qs.stringify(): 对象序列化成URL的形式，以 & 进行拼接

#### 代理

除此之外，还可以通过前端设置代理实现跨域，原理是利用后端不存在跨域问题。比如可以在 `@vue/cli` 项目中新建 `vue.config.js` 文件来配置代理。如果你想了解更多这方面的设置，可以阅读我的这篇博客 [Axios异步请求跨域解决方案](https://evestorm.github.io/posts/15391/)

### JSON 和 JSONP 的区别

JSON 返回的是一串 JSON 格式数据；而 JSONP 返回的是脚本代码（包含一个函数调用）。

JSONP 的全名叫做 JSON with padding，就是把 JSON 对象用符合 JS 语法的形式包裹起来以使其他的网站可以请求到，也就是将 JSON 封装成 JS 文件传过去。

### defer 和 async 的区别

![一图胜千言](https://segmentfault.com/img/bVWhRl?w=801&h=814)

两者都是异步加载，但 defer 是按照加载顺序执行脚本的；async 则是无序加载脚本，例如a.js写在b.js前面，但如果b.js先加载完，则立即执行，不会等a.js的加载。

参考：[https://segmentfault.com/q/1010000000640869](https://segmentfault.com/q/1010000000640869)

#### 技巧

- 如果依赖其他脚本和 DOM 结果，使用 defer
- 如果与 DOM 和其他脚本依赖不强时，使用 async

### preload 和 prefetch 的区别是什么？

对当前页面需要的资源，使用 preload 进行预加载，对其它页面需要的资源进行 prefetch 空闲加载。

```html
<!-- 对sty1e.cs5和 index.js进行pre1oad预加载 -->
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="index.js" as="script">
<!--对资源进行 prefetch预加载-->
<link rel="prefetch" href="next.css">
<link rel="prefetch" href="next.js">
```

### setTimeout、setInterval、requestAnimationFrame 各有什么特点？

`setTimeout` 和 `setInterval` 都不精确。它们的内在运行机制决定了时间间隔参数实际上只是指定了把动画代码添加到浏览器 UI 线程队列中以等待执行的时间。如果队列前面已经加入了其他任务，那动画代码就要等前面的任务完成后再执行。

`requestAnimationFrame` 采用系统时间间隔，保持最佳绘制效率，不会因为间隔时间过短，造成过度绘制，增加开销；也不会因为间隔时间太长，使动画卡顿不流畅，让各种网页动画效果能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果。

### setTimeout 倒计时为什么会出现误差？

`setTimeout()` 只是将事件插入了“任务队列”，必须等当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。要是当前代码消耗时间很长，也有可能要等很久，所以并没办法保证回调函数一定会在 setTimeout() 指定的时间执行。所以， `setTimeout()` 的第二个参数表示的是最少时间，并非是确切时间。

HTML5 标准规定了 `setTimeout()` 的第二个参数的最小值不得小于 4 毫秒，如果低于这个值，则默认是 4 毫秒。在此之前。老版本的浏览器都将最短时间设为 10 毫秒。另外，对于那些 DOM 的变动（尤其是涉及页面重新渲染的部分），通常是间隔 16 毫秒执行。这时使用 `requestAnimationFrame()` 的效果要好于 `setTimeout()`。

### 简单解释单线程、任务队列的概念？

**单线程：**

JavaScript 是浏览器用来与用户进行交互、进行 DOM 操作的，这也使得了它必须是单线程这一特性。

所谓单线程也就是只有一条线，一步一步走。

**任务队列：**

任务（消息）队列是一个先进先出的队列，它里面存放着各种任务（消息）。

在 JavaScript 中任务有两种，一种是同步任务，一种是异步任务。

1. 同步任务：各个任务按照文档定义的顺序一一推入“执行栈”中，当前一个任务执行完毕，才会开始执行下一个任务。

2. 异步任务：各个任务推入“任务队列”中，只有在当前的所有同步任务执行完毕，才会将队列中的任务“出队”执行。

当线程中没有执行任何同步代码的前提下才会执行异步代码。

## 其它

### JSON 的了解

一种轻量级的数据交换格式。 它是基于 JavaScript 的一个子集。

数据格式简单、易于读写、占用带宽小。 e.g. {"age":"12", "name":"back"}

JSON读写的基本封装：

```js
var storage = {
    set: (key, val) => {
        localStorage.setItem(key, JSON.stringify(val))
    },
    get: key => {
        return JSON.parse(localStorage.getItem(key) === null ? '[]' : localStorage.getItem(key))
    },
    remove: key => {
        localStorage.removeItem(key)
    }
}

export default storage
```

#### JSON 方法的缺点

- 不能复制function、正则、Symbol
- 循环引用报错
- 相同的引用会被重复复制

## 概念性问题

### 你理解的面向对象

一种编程开发思想。是过程式代码的一种高度封装，目的在于提高代码的开发效率和可维护性。

### 什么叫优雅降级和渐进增强？

- 优雅降级：Web 站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会针对旧版本的IE进行降级处理了,使之在旧式浏览器上以某种形式降级体验却不至于完全不能用。 如：border-shadow
- 渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加那些只有新版本浏览器才支持的功能,向页面增加不影响基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。 如：默认使用 flash 上传，但如果浏览器支持  HTML5 的文件上传功能，则使用 HTML5 实现更好的体验

### compose 函数 ❌

### 函数柯里化 ❌

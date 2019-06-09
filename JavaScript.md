# JS基础

## JS类型

string，number，boolean，undefined，null，symbol，object

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

## 哪些非 boolean 值被强制转换为一个 boolean 时，它是 false ？

- `""`（空字符串）
- `0`, `-0`, `NaN` （非法的 `number` ）
- `null`, `undefined`

## null，undefined 的区别？

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

## DOM 操作——怎样添加、移除、移动、复制、创建和查找节点？

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
    querySeletor("ul") / querySelectorAll("ul li"); // 查找单个元素 / 多个元素
    getElementsByTagName("div")
    getElementsByClassName()
    getElementById()
```

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
```

### Object.entries()

`**Object.entries()**` 方法返回对象自身可枚举属性的键值对数组，其排列与使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。

```js
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// iterate through key-value gracefully
const obj = { a: 5, b: 7, c: 9 };
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}
```

## 数组的遍历方法

- 标准for循环
- forEach((当前值, 当前索引,当前数组)=>{}）
    - 无法中途退出循环，只能用 `return` 退出本次回调，进行下一次回调。
    - 它总是返回 undefined 值,即使你 return 了一个值。
- for-in（不推荐）会把继承链的对象属性都会遍历一遍，而且数组遍历不一定按次序
    - for-in 循环返回的是所有能通过对象访问的、可枚举的属性。
- for (variable of iterable)（ES6）可迭代 [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array) ，[Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Map)，[Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)，[String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 等（迭代的是值 value ）
    - 在 `for-of` 中如果遍历中途要退出，可以使用 `break` 退出循环。

### ES5

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

### ES6

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

### for in 和 for of 区别

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

## JS 数组有哪些方法

### 改变原数组的方法（9个）

#### splice() 添加 / 删除数组元素

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
let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0, 3); // [1,2,3]
console.log(a); // [4,5,6,7]

// 从最后一个元素开始删除3个元素，因为最后一个元素，所以只删除了7
let item = a.splice(-1, 3); // [7]
```

**删除并添加**

```js
// 从数组下标0开始，删除3个元素，并添加元素'添加'
let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0,3,'添加'); // [1,2,3]
console.log(a); // ['添加',4,5,6,7]

// 从数组最后第二个元素开始，删除3个元素，并添加两个元素'添加1'、'添加2'
let b = [1, 2, 3, 4, 5, 6, 7];
let item = b.splice(-2,3,'添加1','添加2'); // [6,7]
console.log(b); // [1,2,3,4,5,'添加1','添加2']
```

**不删除只添加**

```js
let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
console.log(a); // ['添加1','添加2',1,2,3,4,5,6,7]

let b = [1, 2, 3, 4, 5, 6, 7];
let item = b.splice(-1,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
console.log(b); // [1,2,3,4,5,6,'添加1','添加2',7] 在最后一个元素的前面添加两个元素
```

#### sort() 数组排序

> 定义: sort() 方法对数组元素进行排序，并返回这个数组。
>
> 参数可选: 规定排序顺序的比较函数。
>
> 默认情况下 sort() 方法没有传比较函数的话，默认按字母升序，如果不是元素不是字符串的话，会调用 `toString()` 方法将元素转化为字符串的 Unicode (万国码)位点，然后再比较字符。

**不传参**

```js
// 字符串排列 看起来很正常
var a = ["Banana", "Orange", "Apple", "Mango"];
a.sort(); // ["Apple","Banana","Mango","Orange"]

// 数字排序的时候 因为转换成Unicode字符串之后，有些数字会比较大会排在后面 这显然不是我们想要的
var a = [10, 1, 3, 20,25,8];
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

### 不改变原数组的方法（8个）

#### slice() 浅拷贝数组的元素

> 定义： 方法返回一个从开始到结束（**不包括结束**）选择的数组的一部分浅拷贝到一个新数组对象，且原数组不会被修改。
>
> 语法：array.slice(begin, end);

```js
let a= ['hello','world'];
let b=a.slice(0,1); // ['hello']

// 新数组是浅拷贝的，元素是简单数据类型，改变之后不会互相干扰。
// 如果是复杂数据类型(对象,数组)的话，改变其中一个，另外一个也会改变。
a[0] = '改变原数组';
console.log(a,b); // ['改变原数组','world'] ['hello']

let a = [{name:'OBKoro1'}];
let b = a.slice();
console.log(b,a); // [{"name":"OBKoro1"}]  [{"name":"OBKoro1"}]
// a[0].name='改变原数组';
// console.log(b,a); // [{"name":"改变原数组"}] [{"name":"改变原数组"}]
```

#### join() 数组转字符串

> 定义: join() 方法用于把数组中的所有元素通过指定的分隔符进行分隔放入一个字符串，返回生成的字符串。
>
> 语法: array.join(str)

```js
let a = ['hello','world'];
let str = a.join(); // 'hello,world'
let str2 = a.join('+'); // 'hello+world'

let a = [['OBKoro1','23'],'test'];
let str1 = a.join(); // OBKoro1,23,test
let b = [{name:'OBKoro1',age:'23'},'test'];
let str2 = b.join(); // [object Object],test
// 对象转字符串推荐JSON.stringify(obj);

// 结论：
// join()/toString() 方法在数组元素是数组的时候，会将里面的数组也调用 join()/toString() ,
// 如果是对象的话，对象会被转为 [object Object] 字符串。
```



#### toLocaleString() 数组转字符串

> 定义: 返回一个表示数组元素的字符串。该字符串由数组中的每个元素的  toLocaleString() 返回值经调用  join() 方法连接（由逗号隔开）组成。

```js
let a = [{
    name: 'OBKoro1'
}, 23, 'abcd', new Date()];

console.log(a.join(","));
console.log(a.toString());
console.log(a.toLocaleString('en-us'));
console.log(a.toLocaleString('zh-cn'));

[object Object],23,abcd,Tue Feb 26 2019 11:47:03 GMT+0800 (中国标准时间)
[object Object],23,abcd,Tue Feb 26 2019 11:47:03 GMT+0800 (中国标准时间)
[object Object],23,abcd,2/26/2019, 11:47:03 AM
[object Object],23,abcd,2019/2/26 上午11:47:03
```

#### concat

> 定义： 方法用于合并两个或多个数组，返回一个新数组。
>
> 语法：var newArr =oldArray.concat(arrayX,arrayX,......,arrayX)
>
> 参数：arrayX（必须）：该参数可以是具体的值，也可以是数组对象。可以是任意多个。

```js
let a = [1, 2, 3];
let b = [4, 5, 6];
//连接两个数组
let newVal = a.concat(b); // [1,2,3,4,5,6]
// 连接三个数组
let c = [7, 8, 9]
let newVal2 = a.concat(b, c); // [1,2,3,4,5,6,7,8,9]
// 添加元素
let newVal3 = a.concat('添加元素',b, c,'再加一个');
// [1,2,3,"添加元素",4,5,6,7,8,9,"再加一个"]
// 合并嵌套数组  会浅拷贝嵌套数组
let d = [1,2 ];
let f = [3,[4]];
let newVal4 = d.concat(f); // [1,2,3,[4]]
```

#### ES6扩展运算符 `...` 合并数组

```js
let a = [2, 3, 4, 5]
let b = [ 4,...a, 4, 4]
console.log(a,b); //  [2, 3, 4, 5] [4,2,3,4,5,4,4]
```

#### indexOf() 查找数组是否存在某个元素，返回下标

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
console.log(a.indexOf('啦'));  // -1
console.log(a.indexOf('NaN'));  // -1
console.log(a.indexOf('啦啦')); // 0
```

#### lastIndexOf() 查找指定元素在数组中的最后一个位置

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
 let a = ['OB',4,'Koro1',1,2,'Koro1',3,4,5,'Koro1']; // 数组长度为10
 // let b=a.lastIndexOf('Koro1',4); // 从下标4开始往前找 返回下标2
 // let b=a.lastIndexOf('Koro1',100); //  大于或数组的长度 查找整个数组 返回9
 // let b=a.lastIndexOf('Koro1',-11); // -1 数组不会被查找
 let b = a.lastIndexOf('Koro1',-9); // 从第二个元素4往前查找，没有找到 返回-1
```

#### ES7 includes() 查找数组是否包含某个元素 返回布尔

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
let a = ['OB','Koro1',1,NaN];
// let b=a.includes(NaN); // true 识别NaN
// let b=a.includes('Koro1',100); // false 超过数组长度 不搜索
// let b=a.includes('Koro1',-3);  // true 从倒数第三个元素开始搜索
// let b=a.includes('Koro1',-100);  // true 负值绝对值超过数组长度，搜索整个数组
```

参考：[js 数组详细操作方法及解析合集](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)

## 字符串的方法

### charAt

> #### 从一个字符串中返回指定字符
>
> 如果指定的 index 值超出了该范围，则返回一个空字符串

```js
var anyString = "Brave new world";
console.log(anyString.charAt(0)); // B
```

### substring

> 返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。
>
> - 如果 `indexStart` 等于 `indexEnd`，`substring` 返回一个空字符串。
> - 如果省略 `indexEnd`，`substring` 提取字符一直到字符串末尾。
> - 如果任一参数小于 0 或为 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)，则被当作 0。
> - 如果任一参数大于 `stringName.length`，则被当作 `stringName.length`。
> - 如果 `indexStart` 大于 `indexEnd`，则 `substring` 的执行效果就像两个参数调换了一样。见下面的例子。

```js
var anyString = "Mozilla";
// 输出 "Moz"
console.log(anyString.substring(0,3));
console.log(anyString.substring(3,0));
console.log(anyString.substring(3,-3));
console.log(anyString.substring(3,NaN));
console.log(anyString.substring(-2,3));
console.log(anyString.substring(NaN,3));
```

### replace

> 返回一个由替换值（replacement）替换一些或所有匹配的模式（pattern）后的新字符串。模式可以是一个字符串或者一个 [正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) ，替换值可以是一个字符串或者一个每次匹配都要调用的回调函数。

### toLowerCase / toUpperCase

字母转为全小写或全大写

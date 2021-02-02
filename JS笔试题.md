# JS笔试题

## JS类型相关

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

### valueOf 和 toString

- toString(): 返回一个反映这个对象的字符串。
- valueOf(): 返回它相应的原始值。

## 字符串

### 判断一个单词是否是回文

```js
var str = "mamam"
function checkPalindrom(str) {  
    return str === str.split('').reverse().join('')
}
```

### 验证回文字符串

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

- **输入:** "A man, a plan, a canal: Panama"        **输出:** true
- **输入:** "race a car"                                      **输出:** false

```js
var str = "A man, a plan, a canal: Panama"
var isPalindrome = function (s) {
    var reg = /[^A-Za-z0-9]/g
    var tempStr = s.replace(reg, "")
    var reverseStr = tempStr.split("").reverse().join("")
    return reverseStr.toLowerCase() === tempStr.toLowerCase()
};

console.log(isPalindrome(str))
```

### 统计字符串出现最多的字母

```js
var str = "afjghdfraaaasdenas"

function findMaxDuplicateChar(str) {
    if (str.length == 1) return str
    let charObj = {}
    for (let i = 0; i < str.length; i++) {
        if (!charObj[str.charAt(i)]) {
            charObj[str.charAt(i)] = 1
        } else {
            charObj[str.charAt(i)] += 1
        }
    }
    let maxChar = '',
        maxValue = 1
    for (var key in charObj) {
        if (charObj[key] >= maxValue) {
            maxChar = key
            maxValue = charObj[key]
        }
    }
    return maxChar
}
```

### 字符串中的第一个唯一字符

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

- s = "leetcode"          返回 0.
- s = "loveleetcode"    返回 2.

```js
var str = "leetcode"
function firstUniqChar(str) {
  for (var i = 0; i < str.length; i++) {
    var curChar = str[i]
    if (str.lastIndexOf(curChar) === str.indexOf(curChar)) {
      return i
    }
  }
  return -1
}
console.log(firstUniqChar(str))
```

### 有效的字母异位词

即判断字符串中是否只有字符的位置不同，也就是判断两个字符串中包含的字符以及这些字符出现的次数是否相同

- **输入:** *s* = "anagram", *t* = "nagaram"        **输出:** true
- **输入:** *s* = "rat", *t* = "car"                        **输出:** false

```js
var s = "anagram", t = "nagaram"
function isAnagram(s, t) {
  if (s.length !== t.length) return false;
  var ss = s.split("").sort().join("")
  var tt = t.split("").sort().join("")
  return ss === tt ? true : false
}
console.log(isAnagram(s, t))
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

### 报数

报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

```shell
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` 被读作  `"one 1"`  (`"一个一"`) , 即 `11`。

`11` 被读作 `"two 1s"` (`"两个一"`）, 即 `21`。

`21` 被读作 `"one 2"`,  "`one 1"` （`"一个二"` ,  `"一个一"`) , 即 `1211`。

```js
var countAndSay = function (n) {
    // 从1开始报数
    var result = '1'
    // 循环第 N 次
    for (let i = 1; i < n; i++) {
        // 默认当前连续的数字的次数为1
        var repeatCount = 1
        var str = ''
        // 循环当前数字
        for (let j = 0; j < result.length; j++) {
            console.log(result[j], result[j + 1])
            // 当前数字和后面一个是否相同，相同则重复数计数+1
            if (result[j] === result[j+1]) {
                repeatCount++
            } else {
                // 否则就把到目前为止的报数“读出来”
                str += repeatCount + result[j]
                repeatCount = 1
            }
        }
        // 当前第N次报数的结果，下次报数以此为准
        result = str
    }
    return result
}
```

### 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 `""`。

- **输入:** ["flower","flow","flight"]    **输出:** "fl"
- **输入:** ["dog","racecar","car"]      **输出:** ""

```js
var arr = ["flower", "flow", "flight"]
function longestCommonPrefix(arr) {
    if (!arr.length) return ""
    var firstStr = arr[0]
    var result = ""

    for (var i = 0; i < firstStr.length; i++) {
        for (var j = 1; j < arr.length; j++) {
            if (firstStr[i] !== arr[j][i]) {
                return result
            }
        }
        result += firstStr[i]
    }
    return result
}
console.log(longestCommonPrefix(arr))
```

### 生成指定长度的随机字母数字字符串

```js
function getRandomStr(len) {
    var str = ""
    for (var i = 0; i < len; i++ ) {
        str += Math.random().toString(36).substring(2)
    }
    return str.substring(0, len)
}
```

### 如何把一个字符串的大小写取反（大写变小写小写变大写），例如 ’AbC' 变成 'aBc'

```js
// 方法一：常规
function transformStr(str) {
    let tempArr = str.split('')
    let result = tempArr.map(char => {
        return char === char.toUpperCase() ? char.toLowerCase() : char.toUpperCase()
    })
    return result.join('')
}
console.log(transformStr('aBc'))

// 方法二：正则
'aBc'.replace(/[A-Za-z]/g, char => char === char.toUpperCase() ? char.toLowerCase() : char.toUpperCase())
```

题目来源：[Daily-Interview-Question 第69题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/116)

### 实现一个字符串匹配算法，从长度为 n 的字符串 S 中，查找是否存在字符串 T，T 的长度是 m，若存在返回所在位置。

```js
const find = (S, T) => {
  if (S.length < T.length) return -1
  for (let i = 0; i < S.length; i++) {
    if (S.slice(i, i + T.length) === T) return i
  }
  return -1
}
```

### 用 JavaScript 写一个函数，输入 int 型，返回整数逆序后的字符串。如：输入整型 1234，返回字符串“4321”。要求必须使用递归函数调用，不能用全局变量，输入函数必须只有一个参数传入，必须返回字符串。

```js
function test(num) {
  var str = num + "";
  if (str.length > 1) {
    var newStr = str.substring(str.length - 1);
    var oldStr = str.substring(0, str.length - 1);
    return newStr + test(oldStr);
  } else {
    return str;
  }
}

console.log(test(123));
```

### 找出字符串中连续出现最多的字符和个数

```js
'abcaakjbb' => {'a':2,'b':2}
'abbkejsbcccwqaa' => {'c':3}
```

解答：

```js
const str = "abcaakjbb";

function findLongest (str) {
  if (!str || typeof str !== 'string') return {}
  var obj = {},
    maxCount = 0, // 最大连续次数
    curCount = 1  // 当前连续次数（默认最小连续次数为1）
  for (let i = 0; i < str.length; i++) {
    var curChar = str[i] // 当前字符
    if (curChar === str[i + 1]) { // 如果连续
      ++curCount // 当前连续次数+1
      if (maxCount < curCount) {
        maxCount = curCount
        obj = { [curChar]: maxCount } // 只要当前连续次数比之前的大，就直接覆盖obj
      }
      if (maxCount === curCount) { // 有可能若干字符连续次数相同，那么追加到obj
        obj[curChar] = maxCount
      }
    } else { // 如果不连续
      curCount = 1 // 重置连续次数
    }
  }
  return obj
}

console.log(findLongest(str));
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

// 方法5：ES6 Set去重（能应对NaN重复，对对象无效）
function myDistinct(arr) {
  return [...new Set(arr)]
}
```

### 两个数组比较，判断是否有相同元素（交集）

```js
var nums1 = [1, 2, 3, NaN, {}]
var nums2 = [2, 2, 8, 10, NaN, {}]

// 方法1：filter + indexOf（不支持 NaN 和 {}）
function intersect(nums1, nums2) {
  return nums1.filter(v => {
    const index = nums2.indexOf(v)
    if (index > -1) {
      nums2.splice(index, 1)
      return v
    }
  })
}
console.log(intersect(nums1, nums2))

// 方法2：filter + indexOf（支持 NaN，不支持 {}）
function intersect(nums1, nums2) {
  const aHasNaN = nums1.some(v => isNaN(v))
  const bHasNaN = nums2.some(v => isNaN(v))
  return nums1.filter(v => {
    const index = nums2.indexOf(v)
    if (index > -1) {
      nums2.splice(index, 1)
      return v
    }
  }).concat(aHasNaN && bHasNaN ? [NaN] : [])
}
console.log(intersect(nums1, nums2))

// 方法3：哈希表（支持 NaN，{}）
function intersect(nums1, nums2) {
  const map = {}
  const res = []
  for (let n of nums1) {
    if (map[n]) {
      map[n]++
    } else {
      map[n] = 1
    }
  }
  for (let n of nums2) {
    if (map[n] > 0) {
      res.push(n)
      map[n]--
    }
  }
  return res
}
console.log(intersect(nums1, nums2))
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

### 找出正数组中的最大差值

```js
var arr = [10,5,11,7,8,9]; // 11-5=6

function getMaxProfit(arr) {
  var min = max = arr[0];
  for(var i=0; i<arr.length; i++) {
    min = min <= arr[i] ? min : arr[i];
    max = max >= arr[i] ? max : arr[i];
  }
  return Math.abs(max - min);
}
```

### 从排序数组中删除重复项

```js
// 若 nums = [1,1,2], 则函数应返回长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。
// 若 nums = [0,0,1,1,1,2,2,3,3,4], 则返回 5, 并且原数组被修改为 0, 1, 2, 3, 4。
var nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]
function removeDuplicates(nums) {
  for (var i = 1; i < nums.length; i++) {
    if (nums[i - 1] === nums[i]) {
      nums.splice(i - 1, 1)
      i--
    }
  }
  return nums.length
}
```

### 找出数组中出现次数最多的元素，并给出其出现过的位置

```js
var arr = [1, 1, 2, 1, 10, 10, 11, 1, 7]

function fn(arr) {
    var bestItem, indexs = [], obj = {}
    for (var i = 0, len = arr.length; i < len; i++) {
        var item = arr[i].toString()
        obj[item] ? obj[item].push(i) : obj[item] = [].concat([i])
    }
  
    var tempArr = Object.entries(obj)
    bestItem = parseInt(tempArr[0][0])
    indexs = tempArr[0][1]
  
    for (const [key, value] of tempArr) {
        if (indexs.length < value.length) {
            bestItem = parseInt(key)
            indexs = value
        }
    }

    return { bestItem, indexs }
}
console.log(fn(arr))
```

### 数组扁平化

```js
function flatten(arr) {
  let result = [];

  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result = result.concat(arr[i]);
    }
  }

  return result;
}

// or
Array.prototype.flat = function() {
  return [].concat(...this.map(item => Array.isArray(item) ? item.flat() : [item]))
}

const a = [1, [2, [3, 4]]];
console.log(flatten(a));
console.log(a.flat())
```



### 将数组扁平化并去除其中重复数据，最终得到一个升序且不重复的数组

> 已知如下数组：
>
> var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];
>
> 编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组

```js
// ES6
// 扁平化数组
Array.prototype.flat = function() {
  return [].concat(...this.map(item => Array.isArray(item) ? item.flat() : [item]))
}
// 数组去重
Array.prototype.unique = function() {
  return [...new Set(this)]
}
// 数组排序
const sort = (a, b) => a - b
console.log(arr.flat().unique().sort(sort))

// ===========================================

// ES5
Array.prototype.flat = function() {
  return this.toString().split(',').map(v => +v)
}
Array.prototype.unique = function() {
  var obj = {}
  return this.filter((item, index) => {
    var tempItem = typeof item + JSON.stringify(item)
    return obj.hasOwnProperty(tempItem) ?
      false :
    	obj[tempItem] = true
  })
}
const sort = (a, b) => a - b
console.log(arr.flat().unique().sort(sort))
```

### 使用迭代的方式实现 flatten 函数

> 迭代实现

```js
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]

const flatten = function(arr) {
  while (arr.some(v => Array.isArray(v))) {
     arr = [].concat(...arr)
  }
  return arr
}
```

> 递归实现

```js
const flatten = function(arr) {
  return [].concat(...arr.map(v => Array.isArray(v) ? flatten(v) : [v]))
}
```

> 字符串转换

```js
function flatten(arr) {
  return arr.join(',').split(',').map(v => Number(v))
}
```

### 买卖股票的最佳时机

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意**：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

输入: [7,1,5,3,6,4]         输出: 7

解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

```js
var arr = [7, 1, 5, 3, 6, 4]
function maxProfit(arr) {
    var income = 0;
    for (var i = 1, len = arr.length; i < len; i++) {
        var gap = arr[i] - arr[i - 1] // 后一个与前一个比较，大于零则赚
        if (gap > 0) {
            income += gap
        }
    }
    return income
}
console.log(maxProfit(arr))
```

### 旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

输入: [1,2,3,4,5,6,7] 和 k = 3          输出: [5,6,7,1,2,3,4]

解释:

向右旋转 1 步: [7,1,2,3,4,5,6]

向右旋转 2 步: [6,7,1,2,3,4,5]

向右旋转 3 步: [5,6,7,1,2,3,4]

```js
var arr = [1, 2, 3, 4, 5, 6, 7]

function rotate(arr, k) {
    for (var i = 0; i < k; i++) {
        var temp = arr.pop()
        arr.unshift(temp)
    }
    return arr
}

console.log(rotate(arr, 3))
```

### 判断数组是否存在重复

给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

输入: [1,2,3,1]        输出: true

输入: [1,2,3,4]        输出: false

```js
var arr = [1, 2, 3, 1]
function containsDuplicate(arr) {
    var obj = {}
    for (var i = 0, len = arr.length; i < len; i++) {
        if (obj[arr[i]]) {
            return true
        } else {
            obj[arr[i]] = true
        }
    }
    return false
}

// or

function containsDuplicate(arr) {
  return [...new Set(arr)].length !== arr.length
}

console.log(containsDuplicate(arr))
```

### 找出只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

```js
var arr = [4, 1, 2, 1, 2]

function singleNumber(arr) {
    var tempArr = arr
    tempArr.sort()
    var onceNumber = null
    for (var i = 0, len = tempArr.length - 1; i < len; i++) {
        if (tempArr[i] !== tempArr[i + 1]) {
            onceNumber = tempArr[i]
        } else {
            ++i
        }
    }
    // 有可能排序后最后一个才是单着的数字，所以直接赋值为数组最后一个值
    return onceNumber !== null ? onceNumber : tempArr[tempArr.length - 1]
}

// or

function singleNumber(arr) {
    const a = [...new Set(arr)].reduce((total, cur) => total + cur, 0)
    const b = arr.reduce((total, cur) => total + cur, 0)
    return 2 * a - b
}

console.log(singleNumber(arr))
```

### 加一

给定一个由**整数**组成的**非空**数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1:**

```shell
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

**示例 2:**

```shell
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```

```js
var arr = [9, 9, 9] // 预期：[1, 0, 0, 0]
function plusOne(arr) {
    var count = arr.length - 1
    // 从末尾往前倒
    while (count > -1) {
        // 只要当前位+1大于9，就把当前位置为0，count--
        if (arr[count] + 1 > 9) {
            arr[count] = 0
            count--
        } else {
            // 一旦当前位+1不大于9，就放心+1，且直接退出，不用再算更高位的了
            arr[count]++
            break;
        }
    }
    // 如果while后，第一位还是0，证明这个数组所有数字都为9.这个时候往数组最前面加个1就好
    if (arr[0] === 0) {
        arr.unshift(1)
    }
    return arr;
};
console.log(plusOne(arr))
```

### 移动零

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```shell
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

```js
var arr = [0, 1, 0, 3, 12] // 预期：[ 1, 3, 12, 0, 0 ]
var moveZeroes = function (nums) {
    // 长度提取出来
    var len = nums.length
    for (var i = 0; i < len; i++) {
        // 从头往后遍历，遇到0则删掉追放到尾部，
        // 同时让i--，因为头部删了个0；同时len--，因为不再判断追加后的
        if (nums[i] === 0) {
            nums.splice(i, 1)
            nums.push(0)
            i--
            len--
        }
    }
    return nums
}
console.log(moveZeroes(arr))
```

### 某公司 1 到 12 月份的销售额存在一个对象里面

如下：{1:222, 2:123, 5:888}，请把数据处理为如下结构：[222, 123, null, null, 888, null, null, null, null, null, null, null]。

```js
// for循环
let obj = {1:222, 2:123, 5:888};
function fn(obj) {
  let arr = []
  for (let i = 1; i <= 12; i++) {
    obj[i] ? arr.push(obj[i]) : arr.push(null)
  }
  return arr
}
console.log(fn(obj))


// Array.from
let obj = {1:222, 2:123, 5:888};
const result = Array.from({ length: 12 }).map((_, index) => obj[index + 1] || null);
console.log(result);
```

### 两数之和

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```shell
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

```js
var nums = [2, 7, 11, 15], target = 9
function twoSum(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    const num = nums[i]
    // 第二个数索引
    const targetIndex = nums.indexOf(target - num)
    // 确保存在第二个数，且不为当前遍历的数
    if (targetIndex > -1 && targetIndex !== i) {
      return [i, targetIndex]
    }
  }
}
console.log(twoSum(nums, target))
```

### 旋转图像

给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**原地**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

**示例 1:**

```shell
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```shell
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
],

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

```js
var matrix = [
    [5, 1, 9, 11],
    [2, 4, 8, 10],
    [13, 3, 6, 7],
    [15, 14, 12, 16]
]

function rotate(arr) {
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            var tempArr = arr[j][i]
            arr[j][i] = arr[i][j]
            arr[i][j] = tempArr
        }
        arr[i].reverse()
    }
    return arr
}

console.log(rotate(matrix))
```

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

### 两个数组合并成一个数组

请把两个数组 `['A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'D1', 'D2']` 和 `['A', 'B', 'C', 'D']`，合并为 `['A1', 'A2', 'A', 'B1', 'B2', 'B', 'C1', 'C2', 'C', 'D1', 'D2', 'D']`。

> 考察点：假设有一种情况，让你在一个列表中插入一个广告，不光是数组，对象依然有这种需求，这道题其实就是平常经常需要用到的一个小功能。

```js
let arr1 = ["A1", "A2", "B1", "B2", "C1", "C2", "D1", "D2"]
let arr2 = ["A", "B", "C", "D"]

console.log(
	[...arr1, ...arr2].sort((v2, v1) => (
  	v2.codePointAt(0) - v1.codePointAt(0) ||
    v1.length - v2.length ||
    v2.codePointAt(1) - v1.codePointAt(1)
  ))
)
```

- 第一个条件v2.codePointAt(0) - v1.codePointAt(0) 保证了所有已A开头的字符串会放在最前边，然后依次是B和C。
- 第二个条件v1.length - v2.length保证A会被放在A1和A2之后。
- 第三个条件v2.codePointAt(1) - v1.codePointAt(1)保证了A1会被放在A2前边。

这是我挑的一个比较好的答案，更多解法可[在此](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/39)查看

### 随机生成一个长度为 10 的整数类型的数组，例如 [2, 10, 3, 4, 5, 11, 10, 11, 20]，将其排列成一个新数组，要求新数组形式如下，例如 [[2, 3, 4, 5], [10, 11], [20]]。

解析：由于题意没有说清楚，到底是按「数字区间」排还是按照「连续数字」排，所以两种都写出来供参考

答案：

```js
// 随机整数
function randomArr(min, max) {
  return Math.round(Math.random() * (max - min) + min)
}

// 初始化数组
let initArr = Array.from({length: 10}, () => randomArr(20, 100))

// 数组去重
initArr = [...new Set(initArr)]

// 数组排序
initArr.sort((a, b) => a - b)

// 1. 区间分类解法：
function createTargetArr(initArr) {
  let obj = {}
  initArr.map((num) => {
    const initNum = Math.floor(num / 10)
    if (!obj[initNum]) obj[initNum] = []
    obj[initNum].push(num)
  })

  let result = []
  for (const key in obj) {
    result.push(obj[key])
  }
  return result
}
console.log(createTargetArr(initArr))

// 2. 连续数字分类解法：
function createTargetArr(initArr) {
  let continueArr = [],
    tempArr = []
  console.log(initArr)
  initArr.map((e, index) => {
    tempArr.push(e)
    if (initArr[index + 1] !== ++e) {
      continueArr.push(tempArr)
      tempArr = []
    }
  });
  return continueArr
}
console.log(createTargetArr(initArr))
```

题目来源：[Daily-Interview-Question 第67题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/113)

### 打印出 1 - 10000 之间的所有对称数 例如：121、1331 等

```js
// 遍历 10000 次
function symmetry(begin, end) {
  let result = []
  for (let i = begin > 10 ? begin : 11; i < end; i++) {
    let str = i + ""
    if (str === str.split("").reverse().join("")) {
      result.push(str)
    }
  }
  return result
}
console.log(symmetry(11, 100))

// 利用对称数
function symmetry() {
  var arr = []
  for (let i = 1; i < 10; i++) {
    arr.push(i * 11); // 两位数的对称数
    for (let j = 0; j < 10; j++) {
      arr.push(i * 101 + j * 10) //  三位数的对称数
      arr.push(i * 1001 + j * 110) // 四位数的对称数，当i和j均为9是值为9999
    }
  }
  return arr
}

console.log(symmetry());
```

题目来源：[Daily-Interview-Question 第81题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/131)

### 实现 convert 方法，把原始 list 转换成树形结构，要求尽可能降低时间复杂度

以下数据结构中，id 代表部门编号，name 是部门名称，parentId 是父部门编号，为 0 代表一级部门，现在要求实现一个 convert 方法，把原始 list 转换成树形结构，parentId 为多少就挂载在该 id 的属性 children 数组下，结构如下：

```javascript
// 原始 list 如下
let list = [
    {id:1,name:'部门A',parentId:0},
    {id:2,name:'部门B',parentId:0},
    {id:3,name:'部门C',parentId:1},
    {id:4,name:'部门D',parentId:1},
    {id:5,name:'部门E',parentId:2},
    {id:6,name:'部门F',parentId:3},
    {id:7,name:'部门G',parentId:2},
    {id:8,name:'部门H',parentId:4}
];
const result = convert(list, ...);

// 转换后的结果如下
let result = [
    {
      id: 1,
      name: '部门A',
      parentId: 0,
      children: [
        {
          id: 3,
          name: '部门C',
          parentId: 1,
          children: [
            {
              id: 6,
              name: '部门F',
              parentId: 3
            }, {
              id: 16,
              name: '部门L',
              parentId: 3
            }
          ]
        },
        {
          id: 4,
          name: '部门D',
          parentId: 1,
          children: [
            {
              id: 8,
              name: '部门H',
              parentId: 4
            }
          ]
        }
      ]
    },
  ···
];
```

解答：

```js
// 常规
function convert(list) {
  function fn(pid) {
    return list.filter(dep => {
      if (dep.parentId === pid) {
        // 以自己为根节点，找儿子
        let children = fn(dep.id)
        dep.children = children.length ? children : null
        return true
      }
    })
  }
  // 找最外层根节点0
  return fn(0)
}

// map
function convert (arr) {
  // 数组转对象
  var obj = {}
  for (const item of arr) {
    obj[item.id] = item
  }
  var result = []
  // 循环数组，如果是父部门则装进结果数组然后跳过本次循环，再根据剩下的子部门找相应父部门，把自己挂载到父部们下
  for (const item of arr) {
    if (item.parentId === 0) {
      result.push(item)
      continue
    }
    if (item.parentId in obj) {
      let parent = obj[item.parentId]
      parent.children = parent.children || []
      parent.children.push(item)
    }
  }
  return result
}
```

题目来源：[Daily-Interview-Question 第88题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/139)

### 编程题，请写一个函数，完成以下功能

```js
输入：'1,2,3,5,7,8,10'
输出：'1~3,5,7~8,10'
```

解答：

```js
let str = "1,2,3,5,7,8,10,11"
function fn(str) {
  let arr = str.split(",").map(v => +v)
  let continueNum = arr[0] // tempNum负责存储连续数字的起始
  let result = []
  for (let i = 0; i < arr.length; i++) {
    let curNum = arr[i],
      nextNum = arr[i + 1]
    // 当前+1等于下一个数则跳过
    // 不相等则处理
    if (curNum + 1 !== nextNum) {
      // 分两种情况，
      // 一开始就连续数字：[1,2,5,7] 当i为2时满足上方if条件，tempNum = 1，curNum = 2
      // 一开始非连续数字：[3,5,7] 当i为1时就满足上方if条件，tempNum = 3，curNum = 3
      if (curNum !== continueNum) {
        // 连续数字打头的tempNum + ~ + 目前数字为止
        result.push(continueNum + "~" + curNum)
      } else {
        // 否则直接把当前数字推入数组
        result.push(curNum)
      }
      // 无论连续还是不连续，都把下一项当做连续的起始
      continueNum = nextNum
    }
  }
  return result.join(',')
}

console.log(fn(str))
```

### 补全代码，实现数组按姓名、年纪、任意字段排序。

```js
var users = [
  { name: "John", age: 20, company: "Baidu" },
  { name: "Pete", age: 18, company: "Alibaba" },
  { name: "Ann", age: 19, company: "Tecent" }
]
users.sort(byField('age'))
users.sort(byField('company'))
```

解答：

```js
var users = [
  { name: "John", age: 20, company: "Baidu" },
  { name: "Pete", age: 18, company: "Alibaba" },
  { name: "Ann", age: 19, company: "Tecent" }
]
var byField = function (key) {
  return (a, b) => a[key] > b[key]
}
users.sort(byField('age'))
console.log(users)
users.sort(byField('company'))
console.log(users)
```

**题目来源：**[2019 前端面试 | “HTML + CSS + JS”专题](https://juejin.im/post/5ce4171ff265da1bd04eb4f3#heading-6) 「JavaScript 基础」部分编号 js_12

### 取数组的最大值

```js
// ES5写法
function max(arr) {
  if (arr.length < 1) return undefined
  return Math.max.apply(null, arr)
}
// ES6写法
function max(arr) {
  if (arr.length < 1) return undefined
  return Math.max(...arr)
}
// or
function max(arr) {
  if (arr.length < 1) return undefined
  return arr.reduce((previous, current) => {
    return previous > current ? previous : current
  })
}

console.log(max([1, 22, 3435, 321, 9990, 100001, 2133]))
```

### 实现一个 reduce 函数

解析：reduce是归并方法，它并不对每一项执行目标函数，简单来说概括为如下步骤：

- 不断地对数组的前两项“取出”，对其执行目标函数，计算得到的返回值
- 把上述返回值“填回”数组首部，作为新的 array[0]
- 持续循环执行这个过程，直到数组中每一项都访问了一次
- 返回最终结果

举个例子🌰

对数组 [1，2，3] 归并执行 `(prev, cur) => prev + cur`，流程如图：

```js
[1, 2, 3] // 取出 1 + 2 ，填回 3
[3, 3] // 取出 3 + 3 ，填回 6
[6] // 最终结果为 6
```

答案：

```js
function reduce(arr, fn, initValue) {
  const tempArr = (initValue === undefined ? [] : [initValue]).concat(arr)
  while (tempArr.length > 1) {
    tempArr.splice(0, 2, fn(tempArr[0], tempArr[1]))
  }
  return tempArr[0]
}

var sum = reduce([1, 2, 3], function (previous, current) {
  return previous + current
}, 0)
console.log(sum)
```

写法2：

```js
function reduce(arr, reduceCallback, initialValue) {
  // 首先，检查传递的参数是否正确。
  if (!Array.isArray(arr) || !arr.length || typeof reduceCallback !== 'function') 
  {
    return [];
  } else {
    // 如果没有将initialValue传递给该函数，我们将使用第一个数组项作为initialValue
    let hasInitialValue = initialValue !== undefined;
    let value = hasInitialValue ? initialValue : arr[0];
   、

    // 如果有传递 initialValue，则索引从 1 开始，否则从 0 开始
    for (let i = hasInitialValue ? 0 : 1, len = arr.length; i < len; i++) {
      value = reduceCallback(value, arr[i], i, arr); 
    }
    return value;
  }
}
```



### 手动实现 Array.prototype.map 方法

map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

```js
function map(arr, mapCallback) {
  // 首先，检查传递的参数是否正确。
  if (!Array.isArray(arr) || !arr.length || typeof mapCallback !== 'function') { 
    return [];
  } else {
    let result = [];
    // 每次调用此函数时，我们都会创建一个 result 数组
    // 因为我们不想改变原始数组。
    for (let i = 0, len = arr.length; i < len; i++) {
      result.push(mapCallback(arr[i], i, arr)); 
      // 将 mapCallback 返回的结果 push 到 result 数组中
    }
    return result;
  }
}
```

### 手动实现Array.prototype.filter方法

filter()方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

```js
function filter(arr, filterCallback) {
  // 首先，检查传递的参数是否正确。
  if (!Array.isArray(arr) || !arr.length || typeof filterCallback !== 'function') 
  {
    return [];
  } else {
    let result = [];
     // 每次调用此函数时，我们都会创建一个 result 数组
     // 因为我们不想改变原始数组。
    for (let i = 0, len = arr.length; i < len; i++) {
      // 检查 filterCallback 的返回值是否是真值
      if (filterCallback(arr[i], i, arr)) { 
      // 如果条件为真，则将数组元素 push 到 result 中
        result.push(arr[i]);
      }
    }
    return result; // return the result array
  }
}
```



## 随机数 / 数字

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

### 随机获取数组中的元素

```js
var arr = ["前端", "后端", "全栈"]
function fn(arr) {
    return arr[parseInt(Math.random() * arr.length)]
}
var i = 0
while (i<10) {
    console.log(fn(arr))
    i++
}
```

### 打乱数组顺序

```js
var arr = [1,2,3,4,5,6,7,'a','dsfs',8,9,'v']
function fn(arr) {
  for(var i = 0; i < arr.length; i++) {
    var randomIndex = parseInt(Math.random() * arr.length)
    var temp = arr[i]
    arr[i] = arr[randomIndex]
    arr[randomIndex] = temp
  }
  return arr
}
```

### 保留指定小数位

```js
var num =4.345678
num = num.toFixed(4) // 4.3457 第四位小数位以四舍五入计算
```

### 前端价格展示，保留2位小数。位数不够补零

```js
function priceFormat(price) {
  return parseFloat(Math.round(price * 100) / 100).toFixed(2)
}
```

### 如何将字符串转化为数字，例如'12.3b'？

```js
parseFloat('12.3b')
// 12.3
```

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

### 写一个函数，生成一个随机颜色字符串，合法的颜色为 #000000 ~ #ffffff

```js
function getRandColor() {
  //补全
  const dict = '0123456789abcdef'
  let result = '#'
  for (let i = 0; i < 6; i++) {
    result += dict[Math.floor(Math.random() * dict.length)]
  }
  return result
}
var color = getRandColor()
console.log(color)
```

### 写一个函数，生成一个随机 IP 地址，一个合法的 IP 地址为 0.0.0.0 ~ 255.255.255.255。

```js
function getRandIP() {
  return Array.from({length: 4})
    .reduce((ip, cur, idx) => {
      const randomIdx = Math.floor(Math.random() * 256)
      return idx === 3 ? ip + randomIdx : ip + randomIdx + '.'
    }, '')
}
var ip = getRandIP()
console.log(ip)

// or
function getRandIP() {
  let ip = ''
  for (let i = 0; i < 3; i++) {
    ip = ip + Math.floor(Math.random() * 256) + "."
  }
  return ip + Math.floor(Math.random() * 256)
}
var ip = getRandIP()
console.log(ip)
```

题目来源：[2019 前端面试 | “HTML + CSS + JS”专题](https://juejin.im/post/5ce4171ff265da1bd04eb4f3#heading-6)「《JS 提供的对象：④ Math》[编号：js_19]」

## 函数

### 实现柯里化

预先设置一些参数

柯里化是什么：是指这样一个函数，它接收函数 A，并且能返回一个新的函数，这个新的函数能够处理函数 A 的剩余参数

```js
function createCurry(func, args) {
  var argity = func.length;
  var args = args || [];
  
  return function () {
    var _args = [].slice.apply(arguments);
    args.push(..._args);
    
    if (args.length < argity) {
      return createCurry.call(this, func, args);
    }
    
    return func.apply(this, args);
  }
}
```



## this

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



## 闭包

### 如下代码输出多少？如果想输出 3，那如何改造代码？

```js
var fnArr = [];
for (var i = 0; i < 10; i ++) {
  fnArr[i] =  function(){
    return i
  };
}
console.log(fnArr[3]())
```

答案：

```js
//方法一
var fnArr = []
for(var i=0; i<10; i++) {
  fnArr[i] =  (function(j) {
    return function() {
      return j
    } 
  })(i)
}
console.log(fnArr[3]())  //-->3

//方法二
var fnArr = []
for(var i=0; i<10; i++) {
  (function(i) {
    fnArr[i] =  function() {
      return i
    } 
  })(i)
}
console.log(fnArr[3]())  //-->3

//方法三
var fnArr = []
for(let i=0; i<10; i++) {
  fnArr[i] =  function() {
    return i
  } 
}
console.log(fnArr[3]())  //-->3
```

**题目来源：** [2019 前端面试 | “HTML + CSS + JS”专题](https://juejin.im/post/5ce4171ff265da1bd04eb4f3#heading-5) 的 「JavaScript 基础」部分，编号[js_11]中第5题。

## 对象 & 原型 & 原型链

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
            // 避免相互引用对象导致死循环，如 obj.a = obj（其实下面三行不写也没事儿，系统会自动把 obj.a 设为 undefined）
            if (obj[key] === obj) {
               continue
            }
          	// 如果是数组或者对象: 递归；否则直接返回
          	newObj[key] = typeof obj[key] === 'object' ? copy(obj[key]) : obj[key];
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

#### 拓展：编程题，写个程序把 entry 转换成如下对象

```js
var entry = {
  a: {
  b: {
    c: {
      dd: 'abcdd'
    }
  },
  d: {
    xx: 'adxx'
  },
  e: 'ae'
  }
}

// 要求转换成如下对象
var output = {
'a.b.c.dd': 'abcdd',
'a.d.xx': 'adxx',
'a.e': 'ae'
}
```

解答：

```js
function fn(obj, parentKey = "", result = {}) {
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      const ele = obj[key];
      const keyName = `${parentKey}${key}`
      if (typeof ele === 'object' && !Array.isArray(ele)) {
        fn(ele, keyName+'.', result)
      } else {
        result[keyName] = ele
      }
    }
  }
  return result
}

console.log(fn(entry))
```

### JavaScript 中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

**hasOwnProperty**

hasOwnProperty 返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法不会检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。

使用方法： object.hasOwnProperty(proName) 其中参数 object 是必选项，一个对象的实例；proName 是必选项。一个属性名称的字符串值。 如果 object 具有指定名称的属性，那么 JavaScript 中 hasOwnProperty 函数方法返回 true，反之则返回 false。

### instanceof 的实现原理

> 思路：只要右边变量的 prototype 在左边变量的原型链上即可。

**思路**

步骤1：先取得当前类的原型，当前实例对象的原型链

步骤2：一直循环（执行原型链的查找机制）

-  取得当前实例对象原型链的原型链（`proto = proto.____proto____`，沿着原型链一直向上查找）
-  如果 当前实例的原型链`____proto__`上找到了当前类的原型`prototype`，则返回 `true`
-  如果 一直找到`Object.prototype.____proto____ == null`，Object的基类(null)上面都没找到，则返回 false

```js
function _instanceof (instanceObject, classFunc) {
  let classFunc = classFunc.prototype; // 取得当前类的原型
  let proto = instanceObject.__proto__; // 取得当前实例对象的原型链

  while (true) {
    if (proto === null) { // 找到了 Object的基类 Object.prototype.__proto__
      return false;
    };
    if (proto === classFunc) { // 在当前实例对象的原型链上，找到了当前类
      return true;
    }
    proto = proto.__proto__; // 沿着原型链__ptoto__一层一层向上查找
  }
}
```

**优化版 (处理兼容问题)**

>  **Object.getPrototypeOf ( )：**用来获取某个实例对象的原型（内部**[[prototype]]**属性的值，包含**proto**属性）

```js
function _instanceof (instanceObject, classFunc) {
  let classFunc = classFunc.prototype; // 取得当前类的原型
  let proto = Object.getPrototypeOf(instanceObject); // 取得当前实例对象的原型链上的属性
  
  while (true) {
    if (proto === null) { // 找到了 Object的基类 Object.prototype.__proto__
      return false;
    };
    if (proto === classFunc) { // 在当前实例对象的原型链上，找到了当前类
      return true;
    }
    proto = Object.getPrototypeOf(proto); // 沿着原型链__ptoto__一层一层向上查找
  }
}
```

参考：[浅谈 instanceof 和 typeof 的实现原理](https://juejin.im/post/5b0b9b9051882515773ae714)

### 要求设计 LazyMan 类，实现以下功能

```js
LazyMan('Tony');
// Hi I am Tony

LazyMan('Tony').sleep(10).eat('lunch');
// Hi I am Tony
// 等待了10秒...
// I am eating lunch

LazyMan('Tony').eat('lunch').sleep(10).eat('dinner');
// Hi I am Tony
// I am eating lunch
// 等待了10秒...
// I am eating diner

LazyMan('Tony').eat('lunch').eat('dinner').sleepFirst(5).sleep(10).eat('junk food');
// Hi I am Tony
// 等待了5秒...
// I am eating lunch
// I am eating dinner
// 等待了10秒...
// I am eating junk food
```

答案：

```js
class LazyManClass {
    constructor (name) {
        this.name = name
        this.task = []
        console.log('Hi I am', name)
        setTimeout(() => {
            this.next()
        }, 0)
    }
    eat (str) {
        this.task.push(() => {
            console.log('I am eating', str)
            this.next()
        })
        return this
    }
    sleep (n) {
        this.task.push(() => {
            setTimeout(() => {
                console.log('等待了' + n + 's')
                this.next()
            }, n * 1000)
        })
        return this
    }
    sleepFirst (n) {
        this.task.unshift(() => {
            setTimeout(() => {
                console.log('等待了' + n + 's')
                this.next()
            }, n * 1000)
        })
        return this
    }
    next () {
        let fn = this.task.shift()
        fn && fn()
    }
};

let LazyMan = function (name) {
    return new LazyManClass(name)
};
```

解析：

这道题的关键点有如下几个：

- 链式调用，通过返回 this 实现
- 内部需要维护一个 taskList ，根据不同逻辑，向 taskList 中push 、shift、unshift 执行函数
- 每执行一个task，需要继续执行后续 task，这通过 next 函数实现。

题目来源：[Daily-Interview-Question 第56题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/98)

### 实现一个 new

```js
function _new(fn, ...args) {
  const obj = Object.create(fn.prototype)
  const ret = fn.apply(obj, args)
  return ret instanceof Object ? ret : obj
}

// 测试
function Person(name, age) {
  this.name = name
  this.age = age
  this.sayHi = function() {}
}
Person.prototype.run = function() {}

console.log(_new(Person, 'Lance', 19))
console.log(new Person('Jerry', 20))
```

### 实现一个单例

```js
// ES5
function Person(name, age) {
  this.name = name;
  this.age = age;
}

let Singleton = function(Fn) {
  let _instance = null;
  Singleton = function(...args) {
    if (_instance) return _instance;
    return (_instance = new Fn(...args));
  };
  return Singleton;
};

var peopleSingleton = Singleton(Person);

var ming = new peopleSingleton("ming", 18);
var fang = new peopleSingleton("fang", 28);

console.log(ming, fang);


// ES6
class People {
  constructor(name, age) {
    if (typeof People.instance === 'object') {
      return People.instance
    }
    People.instance = this
    this.name = name
    this.age = age
    return this
  }
}
var ming = new People('ming', 20)
var fang = new People('fang', 19)
console.log(ming, fang)
```

## ES6

### 实现一个 sleep 函数

比如 sleep(1000) 意味着等待1000毫秒，可从 Promise、Generator、Async/Await 等角度实现。

```js
// Promise
const sleep = delay => new Promise(resolve => setTimeout(resolve, delay))
sleep(1000).then(() => console.log('Done'))

// Generator
function* sleepGenerator(delay) {
  yield new Promise(resolve => setTimeout(resolve, delay))
}
sleepGenerator(1000).next().value.then(() => console.log('Done'))

// async / await
const sleep = delay => new Promise(resolve => setTimeout(resolve, delay))
async function output() {
  await sleep(1000)
  console.log('Done')
}
output()

// ES5
function sleep(cb, delay) {
  if (typeof cb === 'function')
    setTimeout(cb, delay)
}
function output() {
  console.log('Done')
}
sleep(output, 1000)
```

### Promise 和 setTimeout 的区别？

Promise 是微任务，setTimeout 是宏任务，同一个事件循环中，promise.then 总是先于 setTimeout 执行。

### Promise 构造函数是同步还是异步执行？then 呢？Promise 如何实现 then 处理？

Promise 的构造函数是同步执行的；then 中的方法是异步执行的。

Promise 实现 then 处理：

```javascript
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";
function Promise(executor) {
  let self = this;
  self.status = PENDING;

  //成功的回调
  self.onFulfilled = [];  

  //失败的回调
  self.onRejected = [];  

  //PromiseA+ 2.1
  function resolve(value) {
    if(self.status === PENDING) {
      self.status = FULFILLED;
      self.value = value;

      //PromiseA+ 2.2.6.1
      self.onFulfilled.forEach(fn => fn());  
    }
   }

  function reject(reason) {
    if(self.status === PENDING) {
      self.status = REJECTED;
      self.reason = reason;

      //PromiseA+ 2.2.6.2
      self.onRejected.forEach(fn => fn());  
    }
  }

  try {
    executor(resolve, reject);
  } catch (e) {
    reject(e);
  }
}

Promise.prototype.then = function (onFulfilled, onRejected) {

  //PromiseA+ 2.2.1 / PromiseA+ 2.2.5 / PromiseA+ 2.2.7.3 / PromiseA+ 2.2.7.4
  onFulfilled = typeof onFulfilled === "function" ? onFulfilled : value => value;
  onRejected = typeof onRejected === "function" ? onRejected : reason => {throw reason};
  let self = this;

  //PromiseA+ 2.2.7
  let promise2 = new Promise((resolve, reject) => {
    if(self.status === FULFILLED) {

      //PromiseA+ 2.2.2
      //PromiseA+ 2.2.4 --- setTimeout
      setTimeout(() => {
        try {

          //PromiseA+ 2.2.7.1
          let x = onFulfilled(self.value);
          resolvePromise(promise2, x, resolve, reject);
        } catch (e) {

          //PromiseA+ 2.2.7.2
          reject(e);
        }
      });
    }else if (self.status === REJECTED) {

      //PromiseA+ 2.2.3
      setTimeout(() => {
        try {
          let x = onRejected(self.reason);
          resolvePromise(promise2, x, resolve, reject);
        } catch (e) {
          reject(e);
        }
      });
    }else if (self.status === PENDING) {
      self.onFulfilled.push(() => {
        setTimeout(() => {
          try {
            let x = onFulfilled(self.value);
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        });
      });
      self.onRejected.push(() => {
        setTimeout(() => {
          try {
            let x = onRejected(self.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        });
      });
    }
  });
  return promise2;
}
```

### 实现一个基本的 Promise

```js
class MyPromise {
  constructor(fn) {
    this.resolvedCallbacks = [];
    this.rejectedCallbacks = [];
    
    this.state = 'PENDING';
    this.value = '';
    
    fn(this.resolve.bind(this), this.reject.bind(this));
    
  }
  
  resolve(value) {
    if (this.state === 'PENDING') {
      this.state = 'RESOLVED';
      this.value = value;
      
      this.resolvedCallbacks.map(cb => cb(value));   
    }
  }
  
  reject(value) {
    if (this.state === 'PENDING') {
      this.state = 'REJECTED';
      this.value = value;
      
      this.rejectedCallbacks.map(cb => cb(value));
    }
  }
  
  then(onFulfilled, onRejected) {
    if (this.state === 'PENDING') {
      this.resolvedCallbacks.push(onFulfilled);
      this.rejectedCallbacks.push(onRejected);
      
    }
    
    if (this.state === 'RESOLVED') {
      onFulfilled(this.value);
    }
    
    if (this.state === 'REJECTED') {
      onRejected(this.value);
    }
  }
}
```

### 如何实现 Promise.all()

首先，需要知道 Promise.all() 的功能：

- 如果传入的参数是一个空的可迭代对象，那么此promise 对象回调完成（resolve），只有此情况，是同步执行的，其它都是异步返回的。
- 如果传入的参数不包含任何 promise，则返回一个异步完成。promises 中所有的 promise 都“完成”时或参数中不包含 promise 时回调完成。
- 如果参数中有一个 promise 失败，那么 Promise.all 返回的 promise 对象失败。
- 在任何情况下，Promise.all 返回的 promise 的完成状态的结果都是一个数组。

```javascript
Promise.all = function (promises) {
  return new Promise((resolve, reject) => {
    let index = 0;
    let result = [];
    if(promises.length === 0) {
      resolve(result);
    }else {
      function processValue(i, data) {
        result[i] = data;
        if(++index === promises.length) {
          esolve(result);
        }
      }
      for(let i = 0; i < promises.length; i++) {
        //promises[i] 可能是普通值
        Promise.resolve(promises[i]).then((data) => {
          processValue(i, data);
        }, (err) => {
          reject(err);
          return;
        });
      }
    }
  });
}
```

### 如何实现 Promise.prototype.finally() ？

不管成功还是失败，都会走到 finally 中,并且 finally 之后，还可以继续 then。并且会将值原封不动的传递给后面的 then。

```javascript
Promise.prototype.finally = function (callback) {
  return this.then((value) => {
    return Promise.resolve(callback()).then(() => {
      return value;
    });
  }, (err) => {
    return Promise.resolve(callback()).then(() => {
      throw err;
    });
  });
}
```

题目来源：[2019 前端面试 | “HTML + CSS + JS”专题](https://mp.weixin.qq.com/s/9T6ebHhof4DIQw3pXHytdQ)「《ES6 速学：⑤ Promise 对象》[编号：es6_05]」

## Event Loop & setTimeout

### setTimeout 的机制

等到当前脚本的同步任务和 "任务队列" 中已有的事件，全部处理完以后，才会执行 setTimeout 指定的任务。

参考：[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

### 使用setTimeout模拟setInterval

```js
// 可避免setInterval因执行时间导致的间隔执行时间不一致
setTimeout (function () {
  // do something
  setTimeout (arguments.callee, 500)
}, 500)
```

### Event Loop

有关 Event Loop 相关的概念和面试题可参考我的博客：[Event Loop 学习笔记](https://evestorm.github.io/posts/10505/)

### 自我测验

上面两篇文章阅读完毕后可以自我测验下：

```js
//请写出输出内容
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2() {
	console.log('async2');
}

console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0)

async1();

new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');
```

题目出处和答案参考：[从一道题浅说 JavaScript 的事件循环](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/7)

## DOM

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

### 原生 JS 添加类

- element.setAttribute("class", 'Lance')
- element.className = "lance awesome"
- 追加类：element.setAttribute("class", element.getAttribute("class") + " " + "lance")

### 编写一个可拖拽的 div

> HTML

```html
<div id="sw"></div>
```

> CSS

```css
#sw { position: absolute; width: 100px; height: 100px; background-color: yellow; }
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

## 预测执行结果

### 下面代码打印结果是什么？为什么?

```js
var b = 10;
(function b() {
  b = 20;
  console.log(b)
})()
```

答案：

- 非严格模式：[Function b]
- 严格模式：`Uncaught TypeError: Assignment to constant variable`

解析：

```js
var b = 10;
(function b() {
   // 内部作用域，会先去查找是有已有变量b的声明，有就直接赋值20，确实有了呀。发现了具名函数 function b(){}，拿此b做赋值；
   // IIFE的函数无法进行赋值（内部机制，类似const定义的常量），所以无效。
  // （这里说的“内部机制”，想搞清楚，需要去查阅一些资料，弄明白IIFE在JS引擎的工作方式，堆栈存储IIFE的方式等）
    b = 20;
    console.log(b); // [Function b]
    console.log(window.b); // 10，不是20
})();
// 严格模式下能看到错误：Uncaught TypeError: Assignment to constant variable
```

其他情况例子：

有`window`：

```js
var b = 10;
(function b() {
    window.b = 20; 
    console.log(b); // [Function b]
    console.log(window.b); // 20是必然的
})();
```

有`var`:

```js
var b = 10;
(function b() {
    var b = 20; // IIFE内部变量
    console.log(b); // 20
   console.log(window.b); // 10 
})();
```

[解析来源 ←](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/48#issuecomment-472695263)

### 输出以下代码执行的结果并解释为什么

```js
var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)
```

结果：

![screenshots](https://user-images.githubusercontent.com/26674103/55368589-3605a000-5525-11e9-8c20-c5aeea6b1880.png)

解析参考：

> 结果最后变为一个稀疏数组[,,1,2]。
> 因为在对象中加入splice属性方法，和length属性后。这个对象变成一个类数组。属性名称可转换为数字时，会映射成为索引下标。所以对象实际可这样描述：[,,3,4],然后继续执行length 属性复制，length 原来值为4，length：2 语句将数组截断成为长度为2的数组。此时数组为[,,]。接着push（1）、push (2)。数组追加两个值，[,,1,2]。小菜看法，有错望指正！！！

参考来源：

- [Daily-Interview-Question 第 46 题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/76)
- [小菜解析](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/75)

### 输出以下代码的执行结果并解释为什么

```js
var a = {n: 1};
var b = a;
a.x = a = {n: 2};

console.log(a.x)
console.log(b.x)
```

答案：

```js
a.x   // --> undefined
b.x   // --> {n: 2}
```

解析：

- 1、优先级`.`的优先级高于`=`，所以先执行`a.x`，堆内存中的`{n: 1}`就会变成`{n: 1, x: undefined}`，改变之后相应的`b.x`也变化了，因为指向的是同一个对象。
- 2、赋值操作是`从右到左`，所以先执行`a = {n: 2}`，`a`的引用就被改变了，然后这个返回值又赋值给了`a.x`，**需要注意**的是这时候`a.x`是第一步中的`{n: 1, x: undefined}`那个对象，其实就是`b.x`，相当于`b.x = {n: 2}`

题目来源：[Daily-Interview-Question 第53题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/93)

### 输出以下代码运行结果

```js
// example 1
var a={}, b='123', c=123;  
a[b]='b';
a[c]='c';  
console.log(a[b]);

---------------------
// example 2
var a={}, b=Symbol('123'), c=Symbol('123');  
a[b]='b';
a[c]='c';  
console.log(a[b]);

---------------------
// example 3
var a={}, b={key:'123'}, c={key:'456'};  
a[b]='b';
a[c]='c';  
console.log(a[b]);
```

分析：这题考察的是对象的键名的转换。

- 对象的键名只能是字符串和 Symbol 类型。
- 其他类型的键名会被转换成字符串类型。
- 对象转字符串默认会调用 toString 方法。

解答：

```js
// example 1
var a={}, b='123', c=123;
a[b]='b';

// c 的键名会被转换成字符串'123'，这里会把 b 覆盖掉。
a[c]='c';  

// 输出 c
console.log(a[b]);
```

```js
// example 2
var a={}, b=Symbol('123'), c=Symbol('123');  

// b 是 Symbol 类型，不需要转换。
a[b]='b';

// c 是 Symbol 类型，不需要转换。任何一个 Symbol 类型的值都是不相等的，所以不会覆盖掉 b。
a[c]='c';

// 输出 b
console.log(a[b]);
```

```js
// example 3
var a={}, b={key:'123'}, c={key:'456'};  

// b 不是字符串也不是 Symbol 类型，需要转换成字符串。
// 对象类型会调用 toString 方法转换成字符串 [object Object]。
a[b]='b';

// c 不是字符串也不是 Symbol 类型，需要转换成字符串。
// 对象类型会调用 toString 方法转换成字符串 [object Object]。这里会把 b 覆盖掉。
a[c]='c';  

// 输出 c
console.log(a[b]);
```

题目来源：[Daily-Interview-Question 第76题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/125)

### 请写出如下代码的打印结果

```js
function Foo() {
    Foo.a = function() {
        console.log(1)
    }
    this.a = function() {
        console.log(2)
    }
}
Foo.prototype.a = function() {
    console.log(3)
}
Foo.a = function() {
    console.log(4)
}
Foo.a();
let obj = new Foo();
obj.a();
Foo.a();
```

解答：

```js
function Foo() {
    Foo.a = function() {
        console.log(1)
    }
    this.a = function() {
        console.log(2)
    }
}
// 以上只是 Foo 的构建方法，没有产生实例，此刻也没有执行

Foo.prototype.a = function() {
    console.log(3)
}
// 现在在 Foo 上挂载了原型方法 a ，方法输出值为 3

Foo.a = function() {
    console.log(4)
}
// 现在在 Foo 上挂载了直接方法 a ，输出值为 4

Foo.a();
// 立刻执行了 Foo 上的 a 方法，也就是刚刚定义的，所以
// # 输出 4

let obj = new Foo();
/* 这里调用了 Foo 的构建方法。Foo 的构建方法主要做了两件事：
1. 将全局的 Foo 上的直接方法 a 替换为一个输出 1 的方法。
2. 在新对象上挂载直接方法 a ，输出值为 2。
*/

obj.a();
// 因为有直接方法 a ，不需要去访问原型链，所以使用的是构建方法里所定义的 this.a，
// # 输出 2

Foo.a();
// 构建方法里已经替换了全局 Foo 上的 a 方法，所以
// # 输出 1
```

题目来源：[Daily-Interview-Question 第100题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/155)

### 分别写出如下代码的返回值

```js
String('11') == new String('11');
String('11') === new String('11');
```

解析：

返回 true 和 false
`new String()` 返回的是对象
`==` 的时候，实际运行的是：
`String('11') == new String('11').toString();`

### 输出以下代码运行结果

```js
1 + "1"

2 * "2"

[1, 2] + [2, 1]

"a" + + "b"
```

解析：

- 1 + "1"

加性操作符：如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来

所以值为：“11”

- 2 * "2"

乘性操作符：如果有一个操作数不是数值，则在后台调用 Number()将其转换为数值

- [1, 2] + [2, 1]

Javascript中所有对象基本都是先调用`valueOf`方法，如果不是数值，再调用`toString`方法。

所以两个数组对象的toString方法相加，值为："1,22,1"

- "a" + + "b"

后边的“+”将作为一元操作符，如果操作数是字符串，将调用Number方法将该操作数转为数值，如果操作数无法转为数值，则为NaN。

所以值为："aNaN"

以上均参考：《Javascript高级程序设计》

题目来源：[Daily-Interview-Question 第116题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/229)

### 下方js执行后的打印值为？

```js
function demo() {
  this.length = 10;
  var fn = function() {
    console.log(this.length); // 输出多少？
  }
  arr = [fn, 'hello layui'];
  fn.length = 100;
  arr[0]();
}
demo()
```

答案：2

解析：this指向问题一般看谁在调用就OK了，在上述函数中，fn的拥有者是arr并且也是它调用的，所以this指向arr，arr中有两个成员，length为2。

#### 延伸

```js
function fn() {}
fn.length = 2
console.log(fn.length) // length 为多少？
```

答案：0

解析：length 是js函数对象的一个属性值，该值是指 “该函数有多少个必须要传入的参数”，即形参的个数。形参的数量不包括剩余参数个数，仅包括 “第一个具有默认值之前的参数个数”，然而fn不需要传递参数，length为0。

拓展阅读：[JS 中函数的 length 属性](https://www.cnblogs.com/go4it/p/9678028.html)

### {} 和 [] 的 valueOf 和 toString 的结果是什么？

```html
{} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"
[] 的 valueOf 结果为 [] ，toString 的结果为 ""
```

- toString是把对象转换为字符串；
- valueOf是把对象转换du成一个基zhi本数据的值
- valueOf偏向dao于运算，toString偏向于显示。

1. 在进行对象转换时（例如:alert(a)）,将优先调用toString方法，如若没有重写toString将调用valueOf方法，如果两方法都没有重写，按Object的toString输出。
2. 在进行强转字符串类型时将优先调用toString方法，强转为数字时优先调用valueOf。
3. 在有运算操作符的情况下，valueOf的优先级高于toString。

### 预测下面的输出是什么

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

解析：

答案是A

1. getFullName函数是赋值给Person的，没有赋值给Person的原型，所以member并没有getFullName这个函数，这样运行会报错 
2. 如果这里运行的是Person.getFullName()  那么会得到两个undefined
3. 如果写了Person.prototype.getFullName = .... 此时调用member.getFullName函数 会得到答案C

### 下面的输出是什么？

```js
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor
    return this.newColor
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor
  }
}

const freddie = new Chameleon({ newColor: 'purple' })
freddie.colorChange('orange')
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

解析：

colorChange是静态方法不是实例方法，需要Chameleon.colorChange才能调用

### 输出是什么？

```js
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2
  },
  perimeter: () => 2 * Math.PI * this.radius
}

shape.diameter()
shape.perimeter()

```

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

解析：

1. shape定义所在环境，是window对象。
2. diameter方法中的this指向方法调用者，即shape。
3. perimeter方法中的this指向方法调用者**所在的环境**，即window对象。

### NaN 是什么，用 typeof 会输出什么？

Not a Number，表示非数字，typeof NaN === 'number'

## 算法题

我面的都不是什么大公司，所以很少被问到算法，不过对于前端来说，了解一些基本的算法还是很有必要的，起码最常见的排序算法得掌握，例如冒泡和快排。这部分内容可参考我的博客：

- [常见排序算法](https://evestorm.github.io/posts/59937/)

### 使用递归完成 1 到 100 的累加？

```js
function fn(num) {
  if(num === 1) return 1
  return num + fn(num - 1)
}

console.log(fn(100))
```

## 正则

对正则不太熟练的同学，在这儿我分享掘金的一篇教程。这篇教程对我来说真的是受益匪浅，希望也能帮助到你：

- [JS正则表达式完整教程（略长）](https://juejin.im/post/5965943ff265da6c30653879)

### 写一个函数 isValidUsername(str)，判断用户输入的是不是合法的用户名（长度 6-20 个字符，只能包括字母、数字、下划线）？

```js
function isValidUsername(str) {
  return /^\w{6,20}$/.test(str)
}

console.log(isValidUsername('swlance'))
```

### 写一个函数 isPhoneNum(str)，判断用户输入的是不是手机号？

```js
function isPhoneNum(str) {
  return /^1[3578]\d{9}$/g.test(str)
}

console.log(isPhoneNum('13871455497'))
```

### 写一个函数 isEmail(str)， 判断用户输入的是不是邮箱？（e.g. xxx@gmail.com 或者 xxx@swlance.com.cn）

```js
function isEmail(str) {
  return /^\w+@\w+(\.\w+){1,2}$/.test(str)
}

console.log(isEmail('xxx@swlance.com.cn'))
```

### 写一个函数 trim(str)，去除字符串两边的空白字符？

```js
function trim(str) {
  return str.replace(/^\s+|\s+$/g, '')
}

console.log(trim('     xxxswlance.cn        '))
```

上述正则题目来源：[2019 前端面试 | “HTML + CSS + JS”专题](https://juejin.im/post/5ce4171ff265da1bd04eb4f3#heading-6) 「《JS 提供的对象：② 正则表达式》[编号：js_17]」

### 转化为驼峰命名

```js
var s1 = "get-element-by-id"
// 转化为 getElementById
```

```js
function fn(str) {
  return str.replace(/-\w/g, x => x.slice(1).toUpperCase())
}
console.log(fn(s1))
```

## 场景题

### 数学逻辑

某果蔬平台商品设置与订单应付款的计算规则说明如下：

A.商品可被设置为不参与其他优惠活动，即不参与会员折扣、满减活动
B.会员价商品不参与其他优惠活动，即不参与会员折扣、满减活动
C.用户积分可抵扣，积分抵扣规则为100积分可抵扣0.5元
D.订单计算规则为：
  Step 1：购物车中的商品分别计算参与优惠部分的商品总价与不参与优惠的商品总价；
  Step 2：参与优惠的商品总价可参与满减活动
  Step 3：上一步处理之后，参与优惠的商品总价可参与会员折扣
  Step 4：所有商品总价，扣减掉积分可抵扣的部分得到订单应支付金额

该果蔬平台有一个商户A，在后台创建了一个商品fruit_a，定价为10元/KG。用户购买3KG商品，问下列各独立场景下，应付款几何？[需要写出结果与计算式，如下所示]

Eg. 商品设置了阶梯价，>=3KG，价格为8元/KG；应付款24=8*3元

2.1 用户有500积分，店铺积分可抵扣，抵扣规则为100积分可抵扣0.5元；店铺设置有满减活动，满20元减3元；商品设置阶梯价，》=3KG，价格为8元/KG，则用户应付款为____元。（5.0分）

2.2 用户有500积分，店铺积分可抵扣，抵扣规则为100积分可抵0.5元；商品设置会员价8元/KG，使用会员价必须使用余额支付；店铺设置有满减活动，满20元减3元；用户若实用余额支付应付款为____元。若使用在线支付，则应付款为____元；（8.0元）

2.3 用户有500积分，店铺积分不可抵扣，抵扣规则为100积分可抵0.5元；店铺有开启会员折扣，会员折扣为8.5折，使用会员折扣必须使用余额支付；店铺设置有满减活动，满20元减3元；用户若使用余额支付应付款为____元，若使用在线支付，则应付款为____元;(8.0分)

参考答案：

2.1
8×3-3-500/100×0.5=18.5元

2.2
8×3-500/100×0.5=21.5元
10×3-3-500/100×0.5=24.5元

2.3
（10×3-3）×0.85=22.95元
10×3-3=27元

## 非常规题

### ['1', '2', '3'].map(parseInt)的结果

正确答案：[1, NaN, NaN]

答案解析：[['1', '2', '3'].map(parseInt) what & why ?](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/4)

## 其它

### 修改以下 print 函数，使之输出 0 到 99，或者 99 到 0

```js
function print(n){
  setTimeout(() => {
    console.log(n);
  }, Math.floor(Math.random() * 1000));
}
for(var i = 0; i < 100; i++){
  print(i);
}
```

解答：

```js
function print0(n){
  setTimeout(() => {
    console.log(n);
  }, 0, Math.floor(Math.random() * 1000));//关闭延迟
}

function print1(n) {
  setTimeout(
    (() => {
      console.log(n);
    })(),
    Math.floor(Math.random() * 1000) //自执行
  );
}

function print2(n) {
  setTimeout(() => {
    console.log(--i);
  }, Math.floor(Math.random() * 1000)); //全局i
}

function print3(n) {
  // setTimeout(() => {
  console.log(n);
  //}, Math.floor(Math.random() * 1000)); //注释
}
```

题目来源：[Daily-Interview-Question 第101题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/158)

### 写一个通用的事件侦听器函数

```js
const EventUtils = {
  // 视能力分别使用dom0||dom2||IE方式 来绑定事件
  // 添加事件
  addEvent: function(element, type, handler) {
    if (element.addEventListener) {
      element.addEventListener(type, handler, false);
    } else if (element.attachEvent) {
      element.attachEvent("on" + type, handler);
    } else {
      element["on" + type] = handler;
    }
  },

  // 移除事件
  removeEvent: function(element, type, handler) {
    if (element.removeEventListener) {
      element.removeEventListener(type, handler, false);
    } else if (element.detachEvent) {
      element.detachEvent("on" + type, handler);
    } else {
      element["on" + type] = null;
    }
  },

  // 获取事件目标
  getTarget: function(event) {
    return event.target || event.srcElement;
  },

  // 获取 event 对象的引用，取到事件的所有信息，确保随时能使用 event
  getEvent: function(event) {
    return event || window.event;
  },

  // 阻止事件（主要是事件冒泡，因为 IE 不支持事件捕获）
  stopPropagation: function(event) {
    if (event.stopPropagation) {
      event.stopPropagation();
    } else {
      event.cancelBubble = true;
    }
  },

  // 取消事件的默认行为
  preventDefault: function(event) {
    if (event.preventDefault) {
      event.preventDefault();
    } else {
      event.returnValue = false;
    }
  }
};
```


# Vue面试题

## watch 和 computed 区别

- watch 是监听动作，computed 是计算属性
- watch 没缓存，只要数据变化就执行。computed 有缓存，只在属性变化的时候才去计算。
- watch 可以执行异步操作，而 computed 不能
- watch 常用于一个数据影响多个数据，computed 则常用于多个数据影响一个数据

## 讲一下 Vue 的生命周期？

创建期间的生命周期函数：

- beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
- created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
- beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中。
    - 换句话说，此时页面中的类似 {{msg}} 这样的语法还没有被替换成真正的数据。
- mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示【可以获取 DOM 节点 | 发起异步请求】
    - 用户已经可以看到渲染好的页面了

运行期间的生命周期函数：

- beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
- updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！

销毁期间的生命周期函数：

- beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
- destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

### Vue 的父组件和子组件生命周期钩子执行顺序是什么

1. 加载渲染过程
   `父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted`
2. 子组件更新过程
   `父beforeUpdate->子beforeUpdate->子updated->父updated`
3. 父组件更新过程
   `父beforeUpdate->父updated`
4. 销毁过程
   `父beforeDestroy->子beforeDestroy->子destroyed->父destroyed`

总结：从外到内，再从内到外

### 更多阅读

- [包你理解---vue 的生命周期](https://segmentfault.com/a/1190000014640577)

## 组件

### 现在有个父子组件，我希望在父级中给子组件绑定一个原生click事件，这个事件会被触发吗？

```html
<div id='app'>
  <my-button @click='change'></my-button>
</div>
<script>
 export default {
  methods: {
    change() {
      alert(1)
    }
  }
 }
</script>
```

答：不能，绑定的该click事件会被当做组件上的一个普通属性看待，如果想要使click事件生效，可以使用 `@click.native='change'` 的方式来实现。

### 组件间如何通讯？

- `props/$emit+v-on`: 通过props将数据自上而下传递，而通过$emit和v-on来向上传递信息。
- EventBus: 通过EventBus进行信息的发布与订阅
- vuex: 是全局数据管理库，可以通过vuex管理全局的数据流
- `$attrs/$listeners`: Vue2.4中加入的`$attrs/$listeners`可以进行跨级的组件通信
- provide/inject：以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效，这成为了跨组件通信的基础

### this.$emit 的返回值是什么？如果需要返回值该怎么办？

this.$emit 的返回值就是 this ，即当前子组件 VueComponent 。

如果想要有返回值可以如下操作：

> 子组件

```html
<template>
  <input :value="name" @change="handleChange" />
</template>
<script>
export default {
  props: ['name'],
  methods: {
    handleChange(e) {
      const res = this.$emit("Echange", e.target.value, val => {
    console.log(val);
      });
      console.log(res, res === this);
    },
  }
}
</script>
```

> 父组件

```html
<template>
  <Child :name='name' @Echange="handleEventChange" />
</template>
<script>
export default {
  data() {
    return {
      name: '',
    }
  }
  methods: {
    handleEventChange(val, callback) {
      this.name = val
      callback("hello")
      return 'hello'
    }
  }
}
</script>
```

## vue 指令

### 能讲下 v-if 和 v-show 的区别吗？

- v-if: 是否加载这个元素（一次性的）
- v-show：控制显示方式block or none（需要切换的，侧边栏）

因此：如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好

### `v-for` 你使用过程中，有遇到什么问题或者关注点吗？

1. 避免将 `v-if` 和 `v-for` 放在同一个元素上，因为 `v-for` 优先级比 `v-if` 更高。例如要渲染 todo 列表中未完成的任务，给 li 标签同时写上 v-for 和 v-if 后会导致每次重新渲染都得遍历整个列表。优化方案是把需要遍历的 todoList 更换为在计算属性上遍历过滤。（Vue文档有详细说明，具体见下方「阅读更多」）
2. 给 `v-for` 设置键绑定键值 `key`。理由见下。

### 在列表组件中添加 key 属性的作用？

key的主要作用就是在更新组件时判断两个节点是否相同。相同就复用，不相同就删除旧的创建新的。这样可以更高效的更新虚拟 DOM。

另外如果给列表组件设置了过渡效果，不添加key属性会导致过渡效果无法触发。因为不添加key会导致vue无法区分它们，导致只会替换节点内部属性而不会触发过渡效果。

### 为什么不建议用 index 作为 key 呢？

**更新DOM时会出现性能问题**

例如我们在使用index作为key值时想要下面列表进行倒序排列：

```html
<li key='0'>React</li>
<li key='1'>Vue</li>
<li key='2'>Angular</li>
<!-- 倒序后↓↓↓↓↓↓↓↓↓↓ -->
<li key='0'>Angular</li>
<li key='1'>Vue</li>
<li key='2'>React</li>
```

vue这个时候仅会调换第一和第三项的文本，li元素则不会调换顺序，因为vue发现key值也就是index没变化。而这会导致li里面的文本重新渲染，影响性能。

而如果采用 id 作为 key，则仅仅需要移动下DOM就OK了，并不需要重新渲染DOM：

```html
<li key='react'>React</li>
<li key='vue'>Vue</li>
<li key='angular'>Angular</li>
<!-- 倒序后↓↓↓↓↓↓↓↓↓↓ -->
<li key='angular'>Angular</li>
<li key='vue'>Vue</li>
<li key='react'>React</li>
```

**会发生一些状态bug**

还是以上面例子为例，给每个li元素中加一个checkbox：

```html
<li key='react'><input type='checkbox'>React</li>
<li key='vue'><input type='checkbox'>Vue</li>
<li key='angular'><input type='checkbox'>Angular</li>
```

如果采用 index 作为 key，选中状态会出现bug（左边列表采用index，右边采用id形式）：

![index-vs-id](https://gitee.com/evestorm/various_resources/raw/master/vue/checkbox.gif)

原因也是 index 没有变化被复用了，导致选中状态永远都是 index=0 的第一项。

阅读更多：

- [避免 v-if 和 v-for 用在一起](https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81)
- [写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1)
- [Vue2.0 v-for 中 :key 到底有什么用？](https://www.zhihu.com/question/61064119/answer/183717717)
- [Vue：为什么使用v-for时必须添加唯一的key？为什么不宜用index作为key？](https://juejin.im/post/5d3c7f25e51d45599e019ea6)

## 数据响应式（双向绑定）怎么做到的？

原理：Vue 采用 **数据劫持** 结合 **发布者-订阅者** 模式的方式，通过 `Object.defineProperty()` 来劫持各个属性的 setter 以及 getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

1. 第一步：需要 Observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
2. 第二步：Compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新数据。
3. 第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情有：
   1. 在自身实例化时往属性订阅器（dep）里面添加自己。
   2. 自身必须有一个 update() 方法
   3. 待属性变动 `dep.notice()` 通知时，能调用自身的 `update()` 方法，并触发 Compile 中绑定的回调，则功成身退。
4. 第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的桥梁，达到数据变化 -> 视图更新；视图交互变化（input） -> 数据 model 变更的双向绑定效果。

参考：

[Vue.js双向绑定的实现原理](https://www.cnblogs.com/kidney/p/6052935.html)

[vue双向数据绑定简单实现(一)](https://www.bilibili.com/video/av16175295)

### 追问1：那如果我要监听一个对象属性的删除或添加呢？

受 defineProperty 限制，Vue 无法检测对象属性的删除和添加。所以我们可以利用 Vue 提供的 `Vue.set` 来解决此问题。

例子：

```shell
有一个obj:{a:1}，想要this.obj.b=233，不会触发视图更新

Vue.set(this.obj, 'b', 233) or this.$set(this.obj, 'b', 233)
```

### 追问2：为什么对象属性的删除或添加无法触发页面更新

因为 vue 在实例化过程中，深度遍历了 data 下所有属性, 把属性全转为 getter/setter 。这样才能监听属性变化。所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的。

当你在对象上新加了一个属性 newProperty ，当前新加的这个属性并没有加入 vue 检测数据更新的机制（因为是在初始化之后添加的）, vue.$set 是能让 vue 知道你添加了属性, 它会给你做处理。

### js 实现简单的双向绑定

```html
<body>
  <div id="app">
    <input type="text" id="txt">
    <p id="show"></p>
  </div>

  <script>
    window.onload = function () {
      let obj = {};
      window.obj = obj
      Object.defineProperty(obj, "txt", {
        get: function () {
          return obj;
        },
        set: function (newValue) {
          document.getElementById("txt").value = newValue;
          document.getElementById("show").innerHTML = newValue;
        }
      })
      document.addEventListener("keyup", function (e) {
        obj.txt = e.target.value;
      })
    }
  </script>
</body>
```

### Vue 的响应式原理中 Object.defineProperty 有什么缺陷？为什么在 Vue3.0 采用了 Proxy，抛弃了 Object.defineProperty？

1. Vue 中使用 Object.defineProperty 进行双向数据绑定时，告知使用者是可以监听数组的，但是只是监听了数组的 push()、pop()、shift()、unshift()、splice()、sort()、reverse() 这八种方法，其他数组的属性检测不到。
2. Object.defineProperty 无法监控到数组**下标**的变化，导致通过数组下标添加元素，不能实时响应；
3. Object.defineProperty 只能劫持对象的属性，因此对每个对象的属性进行遍历时，如果属性值也是对象需要深度遍历，那么就比较麻烦了，所以在比较 Proxy 能完整劫持对象的对比下，选择 Proxy。
4. Proxy 不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。

答案参考：[Daily-Interview-Question - 第51题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/90)
更多阅读：[实现双向绑定Proxy比defineproperty优劣如何](https://www.jianshu.com/p/2df6dcddb0d7)

## Vuex 用过吗？

Vuex 是专为 Vue 应用程序开发的状态管理工具，相当于共享仓库，方便任何组件直接获取和修改。

- state - 数据【存项目共享状态，是响应式的，store的数据改变，所有依赖此状态的组件会更新】
    - $store.state.count
- mutations - 方法【同步函数】
    - inc(state, 参数唯一) {}
    - $store.commit('inc', 2)
- getters - 包装数据 【store的计算属性，可缓存】
    - show: function(state) {}
    - this.$store.getters.show
    - 传参，返回函数：show(state) {return function(参数) {return ...}}【不会缓存数据】
- actions -【异步操作】【提交的是mutations，不直接修改状态】
    - increment(context, num) {context.commit()}
    - this.$store.dispatch('',arg)

### 使用 Vuex 管理数据，与直接在全局window下定义变量相比，有什么区别或者说优势？

全局作用域下定义的数据是静态的，只能通过手动修改，修改后数据变了，但使用这些数据的组件并不会重新渲染，也必须得手动渲染。而且全局作用域下定义太多变量还容易造成变量污染。

Vuex 只要 store 中的数据更新，就会立即渲染所有使用 store 数据的组件。Vuex 使用单向数据流，要想修改 store 数据需要经过 action 层，mutation 层，层次划分明确，便于管理。

### Vuex 是通过什么方式提供响应式数据的？

在 Store 构造函数中通过 new Vue({}}) 实现的。利用 Vue 来监听 state 下的数据变化，给状态添加 getter、setter。

### Vuex 如何区分 state 是外部直接修改，还是通过 mutation 方法修改的？

Vuex 中修改 state 的唯一渠道就是执行 commit('xx', payload) 方法，其底层通过执行 this._withCommit(fn) 设置_committing 标志变量为 true，然后才能修改 state，修改完毕还需要还原_committing 变量。外部修改虽然能够直接修改 state，但是并没有修改_committing 标志位，所以只要 watch 一下 state，state change 时判断是否_committing 值为 true，即可判断修改的合法性。

### Vuex 原理

vuex 仅仅是作为 vue 的一个插件而存在，不像 Redux,MobX 等库可以应用于所有框架，vuex 只能使用在 vue 上，很大的程度是因为其高度依赖于 vue 的 computed 依赖检测系统以及其插件系统，

vuex 整体思想诞生于 flux,可其的实现方式完完全全的使用了 vue 自身的响应式设计，依赖监听、依赖收集都属于 vue 对对象 Property set get 方法的代理劫持。最后一句话结束 vuex 工作原理，vuex 中的 store 本质就是没有 template 的隐藏着的 vue 组件。

## VueRouter 是什么？你平常是怎么用的？

- 是什么：Vue-Router 是 Vue 官方的路由管理器

- 作用：为了页面跳转

- 原理：监听锚点值改变，渲染指定页面

    ```js
    <div class="h">我是头部</div>
    <div id="content" class="b"></div>
    <div class="f">我是底部</div>
    <script type="text/javascript">
    //监视锚点值的改变
    window.addEventListener('hashchange', function() {
        var text = '';
        switch (location.hash) {
            case '#/music':
                text = '各种音乐的数据';
                break;
            case '#/movie':
                text = '各种电影的数据';
                break;
        }
        document.getElementById('content').innerHTML = text;
    })
    </script>
    ```

### 动态路由

查询字符串：

1. 去哪（列表页传参） <router-link :to="{name:'detail',query:{id:1}}">xxx</router-link>
2. 导航（router中） { name:'detail' , path:'/detail', component: Detail }
3. 去了干嘛（详情页接收参数）this.$route.query.id

path方式：

1. 去哪里 <router-link :to="{name:'detail',params:{name:1}}">xxx</router-link>
2. 导航 { name:'detail' , path:'/detail/:name', component: Detail}
3. 去了干嘛（获取路由参数）this.$route.params.name

### 编程式导航

```js
// name配params：
this.$router.push({name: 'Goods', params: {goodsId:id}})

// path配query：
this.$router.push({path: '/goods', query: {goodsId:id}})

// 参数接收匹配：
this.goodsId = this.$route.query.goodsId
this.goodsId = this.$route.params.goodsId
```

### 路由守卫

- 全局守卫：beforeEach, beforeResolve, afterEach【没有next】
- 路由独享守卫：beforeEnter
- 组件内守卫：beforeRouteEnter【唯一next有回调】, Update, Leave

#### 应用

- 全局守卫：beforeEach（用户登录以及权限判定）

    ```js
    router.beforeEach((to, from, next) => {
      const isLogin = localStroage.token
      // 个人中心需要登录
      if (to.name === 'Member') {
        isLogin ? next() : next('/login')
      } else {
        next()
      }
    })
    ```

- 组件内守卫：beforeRouteEnter（根据用户从何而来，修改当前组件标题）
    ```js
    // 详情页组件
    beforeRouteEnter(to, from, next) {
        let title = ''
        title = from.name === 'news.list' ? '新闻详情' : '商品详情'
        // 一定要调用 next ，否则无法从列表页跳转到详情页
        next(vm => vm.title = title) // 通过 vm 访问组件实例
    }
    ```

#### 讲一下完整的Vue路由生命周期

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

### 实现一个简单路由

```js
// hash路由
class Route{
  constructor(){
    // 路由存储对象
    this.routes = {}
    // 当前hash
    this.currentHash = ''
    // 绑定this，避免监听时this指向改变
    this.freshRoute = this.freshRoute.bind(this)
    // 监听
    window.addEventListener('load', this.freshRoute, false)
    window.addEventListener('hashchange', this.freshRoute, false)
  }
  // 存储
  storeRoute (path, cb) {
    this.routes[path] = cb || function () {}
  }
  // 更新
  freshRoute () {
    this.currentHash = location.hash.slice(1) || '/'
    this.routes[this.currentHash]()
  }
}
```

### vue脚手架生成的 router.js 中，有一段 `base: process.env.BASE_URL` 配置，你知道它引用了谁的路径么？

```js
const router = new Router({
  mode: 'history',
  base: process.env.BASE_URL, // <-
  ...
```

它会和 vue.config.js 中的 publicPath 选项相符，即你的应用会部署到的线上/开发环境的基础路径：

```js
const path = require('path')
module.exports = {
  // 旧版叫做baseURL
  // 线上/开发环境的路径配置
  publicPath: process.env.NODE_ENV === 'production' ? 'http://你的线上环境' : '/',
```

## 你是如何使用插槽的？

- 默认插槽：父<child>html模板</child> | 子<slot></slot>
- 具名插槽：父<child><template slot="footer"></template> | 子 <slot name="footer"></slot>
- 作用域插槽：父<child><template slot-scope="user"></template></child> | 子提供数据 <slot :data=data>

阅读更多：[深入理解vue中的slot与slot-scope](https://segmentfault.com/a/1190000012996217)

### 相同名称的插槽是合并还是替换？

- Vue 2.5版本，匿名和具名插槽都是合并，作用域插槽是替换
- Vue 2.6版本，都是替换（因为新版底层原理一样）

## nextTick的更新原理

Vue 在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。

nextTick的回调函数会等到同步任务执行完毕，DOM更新后才触发。

阅读更多：[Vue.nextTick 的原理和用途](https://segmentfault.com/a/1190000012861862)

## 在 Vue 中，子组件为何不可以修改父组件传递的 Prop

因为 prop 的传递是单向数据流，这样易于监测数据的流动，出现了错误可以更加迅速的定位到错误发生的位置。另外 props 传入的值如果对象的话，是可以直接在子组件里更改的，因为是同一个引用。

### 如果修改了，Vue 是如何监控到属性的修改并给出警告的

```js
if (process.env.NODE_ENV !== 'production') {
  var hyphenatedKey = hyphenate(key);
  if (isReservedAttribute(hyphenatedKey) ||
      config.isReservedAttr(hyphenatedKey)) {
    warn(
      ("\"" + hyphenatedKey + "\" is a reserved attribute and cannot be used as component prop."),
      vm
    );
  }
  defineReactive$$1(props, key, value, function () {
    if (!isRoot && !isUpdatingChildComponent) {
      warn(
        "Avoid mutating a prop directly since the value will be " +
        "overwritten whenever the parent component re-renders. " +
        "Instead, use a data or computed property based on the prop's " +
        "value. Prop being mutated: \"" + key + "\"",
        vm
      );
    }
  });
}
```

在initProps的时候，在defineReactive时通过判断是否在开发环境，如果是开发环境，会在触发set的时候判断是否此key是否处于updatingChildren中被修改，如果不是，说明此修改来自子组件，触发warning提示。

需要特别注意的是，当你从子组件修改的prop属于基础类型时会触发提示。 这种情况下，你是无法修改父组件的数据源的， 因为基础类型赋值时是值拷贝。你直接将另一个非基础类型（Object, array）赋值到此key时也会触发提示(但实际上不会影响父组件的数据源)， 当你修改object的属性时不会触发提示，并且会修改父组件数据源的数据。

## 虚拟DOM

### 虚拟DOM实现原理?

- 虚拟DOM本质上是JavaScript对象,是对真实DOM的抽象
- 状态变更时，记录新树和旧树的差异
- 最后把差异更新到真正的dom中

详细实现见[虚拟DOM原理?](https://user-gold-cdn.xitu.io/2019/8/1/16c49afec13e0416)

### 虚拟DOM的优劣如何?

优点:

- 保证性能下限: 虚拟DOM可以经过diff找出最小差异,然后批量进行patch,这种操作虽然比不上手动优化,但是比起粗暴的DOM操作性能要好很多,因此虚拟DOM可以保证性能下限
- 无需手动操作DOM: 虚拟DOM的diff和patch都是在一次更新中自动进行的,我们无需手动操作DOM,极大提高开发效率
- 跨平台: 虚拟DOM本质上是JavaScript对象,而DOM与平台强相关,相比之下虚拟DOM可以进行更方便地跨平台操作,例如服务器渲染、移动端开发等等

缺点:

- 无法进行极致优化: 在一些性能要求极高的应用中虚拟DOM无法进行针对性的极致优化,比如VScode采用直接手动操作DOM的方式进行极端的性能优化

## 其它

### vue 的优点是什么？

- 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。
- 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用 Expression Blend 可以很容易设计界面并生成 xml 代码。
- 可测试。界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

平时开发中，经常会用到这样一个语句：

```javascript
import Vue from 'vue';
```

由于浏览器兼容性问题，通常这个语法是在 webpack 的构建流搭建的项目中执行的，那么这个语句到底做了什么呢？

其实在 nodejs 中，执行 `import` 就相当于执行了 `require`，而 `require` 被调用，其实会用到 `require.resolve` 这个函数来查找包的路径，而这个函数在 nodejs 中会有一个关于优先级的算法。先看一下 `import Vue from 'vue'` 这一句做了什么：

1. `import Vue from 'vue'` 解析为 `const Vue = require('vue')`。
2. `require` 判断 vue 是否未 nodejs 核心包，如我们常用的：path，fs 等，是则直接导入，否则继续往下走。
3. vue 非 nodejs 核心包，判断 vue 是否未 '/' 根目录开头，显然不是，继续往下走。
4. vue 是否为 './'、'/' 或者 '../' 开头，显然不是，继续往下走。
5. 以上条件都不符合，读取项目目录下 node_modules 包里的包。

到了第五步，`import Vue from 'vue'` 就找到了 vue 所在的实际位置了，那么问题来了，node_modules 下的 vue 是一个文件夹，而引入的 Vue 是一个 javascript 对象，那它是怎么取到这个对象呢？

其实对于一个 npm 包，内部还有一个文件输出的规则，先看下 node_modules/vue 下的文件结构是怎么样的：

```bash
├── LICENSE
├── README.md
├── dist
├── package.json
├── src
└── types
```

是不是看起来很笼统，其实对于 npm 包，`require` 的规则是这样的：

1. 查找 package.json 下是否定义了 main 字段，是则读取 main 字段下定义的入口。
2. 如果没有 package.json 文件，则读取文件夹下的 index.js 或者 index.node。
3. 如果都 package.json、index.js、index.node 都找不到，抛出错误 `Error: Cannot find module 'some-library'`。

那么看一下 vue 的 package.json 文件有这么一句：

```json
{
    ...
    "main": "dist/vue.runtime.common.js",
    ...
}
```

到这里就很清晰了：

```javascript
import vue from 'vue';

// 最后转换为
const vue = require('./node_modules/vue/dist/vue.runtime.common.js');
```

而 vue.runtime.common.js 文件的最后一行是：`module.exports = Vue;`，就正好跟我们平时使用时的 `new Vue({})` 是一致的，这就是 `import vue from 'vue'` 的过程了。

当然，这个是我们平时使用得最多的导入方式，还有其他一些导入规则，都可以在 [nodejs 的文档](https://nodejs.org/api/modules.html#modules_all_together) 中找到。

**文章来源**：https://www.jianshu.com/p/fad3688cbd81

### Vue 和 jQuery 有什么区别？

jQuery是使用选择器（$）选取DOM对象，对其进行赋值、取值、事件绑定等操作，其实和原生的HTML的区别只在于可以更方便的选取和操作DOM对象，而数据和界面是在一起的。比如需要获取label标签的内容：$("label").val();,它还是依赖DOM元素的值。

Vue则是通过Vue对象将数据和View完全分离开来了。对数据进行操作不再需要引用相应的DOM对象，可以说数据和View是分离的，他们通过Vue对象这个vm实现相互的绑定，也就是MVVM。

### 谈谈你对 MVVM 开发模式的理解

- Model 数据模型层，保存页面中的数据
- View 视图层，页面数据的展示；
- ViewModel M，V调度者。双向数据绑定。通过监听 Model 数据的改变来控制V的更新，处理用户交互操作；

M 数据改变 =>VM => 触发 View 更新

V 用户操作 => VM => M 数据同步更新

开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom 。

### 请你讲一下Vue项目中使用token登录的具体流程

对 token 完全不了解的同学可以查看我博客中转载的 [这篇文章](https://evestorm.github.io/posts/29493/) 。

不想看长篇大论的我这里简单描述下基于 token 身份验证的整个流程：

1. 用户通过账户名和密码发送登录请求
2. 服务端对账户的有效性进行验证
3. 验证成功后再利用「密钥」和「加密算法」（如：HMAC-SHA256）对「用户数据」（如账号信息）做一个签名的 token 返回给客户端
4. 客户端本地存储 token ，并在每次请求的 header 中带上 token
5. 服务端验证 token 并返回数据

对于 Vue 项目来说，具体流程如下：

1. 客户端：登录页带上用户名和密码请求登录接口
2. 服务端：接收请求并在数据库中查询账户的有效性
3. 服务端：查询通过后利用「密钥」和「加密算法」对「用户数据」做签名 token 并返回给客户端此 token （此 token 有时效性）
4. 客户端：本地存储 token（如 localStorage + Vuex）
5. 客户端：每次路由跳转前都要判断 localStorage 是否存在 token ，有则正常跳转，无则跳转回登录页
6. 客户端：每次发送请求时，在 Axios 请求头里携带上 token
7. 服务端：接收请求并判断请求头有无 token ，有且 token 没有过期，正常返回数据；无或 token 失效返回 401 状态码
8. 客户端：一旦发现 401 则重定向到登录页

一般回答完毕后面试官还会追问一些细节，这里列举两个常问的：

1. 你是如何利用 Axios 实现携带 token 以及 401 状态码判定的？

    利用 Axios 的请求/响应拦截。使用 `axios.interceptors.request.use` 进行请求拦截，判断 localStorage 是否有 token ，有则在请求头里携带 token 。使用 `axios.interceptors.response.use` 进行响应拦截，判断 response.status 是否为 401 ，是则代表 token 失效，清空本地 token ，跳转登录页

2. 你是如何监控路由跳转，并在没有 token 时跳转回登录页的？

    使用 Vue Router 的全局路由守卫 router.beforeEach ，该方法接收三个参数：to、from 和 next ，如果用户访问的是不需要登录就能访问的页面（如 to.path === '/login'），则直接跳转。否则判断本地是否有 token ，有则调用 next() ，无则 next('/login') 跳转回登录页

### SPA 的缺点有哪些，如何解决？

- 不利于SEO
- 首屏渲染时间过长

### vue 如何优化首页的加载速度？vue 首页白屏是什么问题引起的？如何解决呢？

首页白屏的原因：
单页面应用的 html 是靠 js 生成，因为首屏需要加载很大的js文件(`app.js` `vendor.js`)，所以当网速差的时候会产生一定程度的白屏

解决办法：

- 1.将公用的JS库通过script标签外部引入，减小app.bundle的大小，让浏览器并行下载资源文件，提高下载速度；
- 2.在配置路由时，页面和组件使用懒加载的方式引入，进一步缩小 app.bundle 的体积，在调用某个组件时再加载对应的js文件；
- 3.上**骨架屏**或loading动画，提升用户体验；
- 4.合理使用web worker优化一些计算
- 5.缓存一定要使用，但是请注意合理使用
- 6.最后可以借助一些工具进行性能评测，重点调优，例如chrome开发者工具的 performance 或 Google PageSpeed Insights 插件协助测试。

# Vue面试题

## watch 和 computed 区别

- watch 是监听动作，computed 是计算属性
- computed 有缓存，只在属性变化的时候才去计算。watch 没缓存，只要数据变化就执行
- watch 可以执行异步操作，而computed不能
- watch 一个数据影响多个数据，computed是多个数据影响一个数据

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

### 更多阅读

- [包你理解---vue 的生命周期](https://segmentfault.com/a/1190000014640577)

## 组件间通信

- 父子组件
    - 父—>子：prop传递值、v-on绑定方法(@子属性名="父方法名") | 子主动获取 this.$parent.*
    - 子—>父：子组件 this.$emit

- 兄弟组件（非父子）【用eventBus】

    - 封装 eventVue.js export 一个新 vue 实例
    - 监听者：VueEvent.$on("事件名", function(data){...})
    - 广播者：VueEvent.$emit("事件名", data)
    - 缺点：消息容易重名，同一个状态分散在不同地方，不容易管理

## v-if vs v-show

- v-if: 是否加载这个元素（一次性的）
- v-show：控制显示方式block or none（需要切换的，侧边栏）

因此：如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好

## 数据响应式（双向绑定）怎么做到的？

原理：采用 数据劫持 + 发布/订阅者模式的方式

- 创建 vm 实例时深度遍历 data 下所有属性，利用 Object.defineProperty() 把属性转为 setter，getter
- 在数据变动时利用消息订阅器（Dep）发消息给订阅者（Watcher），触发相应的监听回调。
- 创建 vm 实例时有一个指令解析的过程，会对每个元素节点的指令进行扫描和解析（e.g. v-if, v-model），根据指令模板替换数据，以及绑定相应的更新函数，并且在遇到 v-model 指令时，监听用户的输入，以便更新 data 下相应的属性。

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

### Vue 的响应式原理中 Object.defineProperty 有什么缺陷？为什么在 Vue3.0 采用了 Proxy，抛弃了 Object.defineProperty？

1. Object.defineProperty无法监控到数组下标的变化，导致通过数组下标添加元素，不能实时响应；
2. Object.defineProperty只能劫持对象的属性，从而需要对每个对象，每个属性进行遍历，如果，属性值是对象，还需要深度遍历。Proxy可以劫持整个对象，并返回一个新的对象。
3. Proxy不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。

答案参考：[Daily-Interview-Question - 第51题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/90)

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

## 你是如何使用插槽的？

- 默认插槽：父<child>html模板</child> | 子<slot></slot>
- 具名插槽：父<child><template slot="footer"></template> | 子 <slot name="footer"></slot>
- 作用域插槽：父<child><template slot-scope="user"></template></child> | 子提供数据 <slot :data=data>

阅读更多：[深入理解vue中的slot与slot-scope](https://segmentfault.com/a/1190000012996217)

## nextTick的更新原理

Vue 在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。

nextTick的回调函数会等到同步任务执行完毕，DOM更新后才触发。

阅读更多：[Vue.nextTick 的原理和用途](https://segmentfault.com/a/1190000012861862)

## 在列表组件中添加 key 属性的作用？

key的主要作用是为了高效的更新虚拟 DOM。另外如果给列表组件设置了过渡效果，不添加key属性会导致过渡效果无法触发。

阅读更多：

- [写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1)
- [Vue2.0 v-for 中 :key 到底有什么用？](https://www.zhihu.com/question/61064119/answer/183717717)

## 在 Vue 中，子组件为何不可以修改父组件传递的 Prop

因为 prop 的传递是单向数据流，这样易于监测数据的流动，出现了错误可以更加迅速的定位到错误发生的位置。另外 props 传入的值如果对象的话，是可以直接在子组件里更改的，因为是同一个引用。

## 其它

### 谈谈你对 MVVM 开发模式的理解

- Model 数据模型层，保存页面中的数据
- View 视图层，页面数据的展示；
- ViewModel M，V调度者。双向数据绑定。通过监听 Model 数据的改变来控制V的更新，处理用户交互操作；

M 数据改变 =>VM => 触发 View 更新

V 用户操作 => VM => M 数据同步更新

开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom 。

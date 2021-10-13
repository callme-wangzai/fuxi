一面
1、微信小程序，内嵌H5怎么和原生小程序通信【描述】

小程序 web-view 组件支持事件：bindload binderror bindmessage
web-view 网页中可使用JSSDK 1.3.2 提供的接口返回小程序页面：wx.miniProgram.reLaunch，H5向小程序发消息：wx.miniProgram.postMessage

2、小程序分包开发怎么做【描述】【举例】

通过在 app.json 的 subpackages 字段声明项目分包结构：
{
  "subpackages": [
    {
      "root": "packageA",
      "pages": [
        "pages/cat",
        "pages/dog"
      ]
    },
    {
      "root": "packageB",
      "name": "pack2",
      "pages": [
        "pages/apple",
        "pages/banana"
      ]
    }
  ]
}
声明 subpackages 后，将按 subpackages 配置路径进行打包，subpackages 配置路径外的目录将被打包到 app（主包） 中
app（主包）也可以有自己的 pages（即最外层的 pages 字段）
subpackage 的根目录不能是另外一个 subpackage 内的子目录
tabBar 页面必须在 app（主包）内

3、做过哪些性能优化【描述】

4、h5首页加速项目，怎么衡量优化结果是否达到预期【描述】

5、h5页面，页面缓存(html)、资源缓存(js,css)，微信长缓存怎么解决【描述】
针对html的缓存
（1）利用服务端设置response Headers, 强制让HTML文件，每次都向服务器端强制校验文件有效性~
cache-control: max-age=0
Last-Modified: Fri,05 Jun 2020 09:52:12 GMT
ETag: W/"5e5asdfs-98dc"
（2）当前链接路径加井号时间戳
对于JS和CSS静态文件【取以下两种方案其一即可】
（1）在引用时加上动态版本号，例如 <script src="index.js?v=2.5.0" />
（2）动态命名问题，例如利用webpack等打包工具生成HASH文件名 <script src="index.12321312.js" />

6、写个函数，判断一个字符串是否为手机靓号，手机靓号条件：有3个连续相同的数字如 '111' 或者 有4个依次递增1的数字 '1234'【编程】
解法1：枚举
将所有可能的靓号规则存入数组，逐一判断，扩展性较差，如果不是4位就难搞
解法2：正则
function isGoodPhone (phone) {
    let phoneReg = /^1\d{10}$/;
    let reg1 = /(.)\1{2,}/g;
    let reg2 = /(?:0(?=1)|1(?=2)|2(?=3)|3(?=4)|4(?=5)|5(?=6)|6(?=7)|7(?=8)|8(?=9)){3}/g;
    return phoneReg.test(phone) && (reg1.test(phone) || reg2.test(phone));
}

7、算法，写一个排序算法【编程】
// 冒泡排序
// 该算法的操作次数是一个等差数列 n + (n - 1) + (n - 2) + 1 ，去掉常数项以后得出时间复杂度是 O(n * n)
function bubble(arr) {
    if (!arr instanceof Array) return arr;
    arr = arr.slice();
    for (let i = arr.length - 1; i > 0; i--) {
        for (let j = 0; j < i; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    return arr;
}

8、vue-router vuex 等vue插件是怎么注入vue实例的？【描述】

9、vue-router 的 beforeEach 和 vue组件生命周期钩子之间的执行顺序【描述】

10、说一下vue响应式原理【描述】
Vue 响应式原理其实是在 vm._init() 中完成的，调用顺序 initState() --> initData() --> observe() 。observe() 就是响应式的入口函数。
（1）observe(value)：这个方法接收一个参数 value ，就是需要处理成响应式的对象；判断 value 是否为对象，如果不是直接返回；判断 value 对象是否有 __ob__ 属性，如果有直接返回；如果没有，创建 observer 对象；返回 observer 对象；
（2）Observer：给 value 对象定义不可枚举的 __ob__ 属性，记录当前的 observer 对象；数组的响应式处理，覆盖原生的 push/splice/unshift 等方法，它们会改变原数组，当这些方法被调用时会发送通知；对象的响应式处理，调用 walk 方法，遍历对象的每个属性，调用 defineReactive ；
（3）defineReactive：为每一个属性创建 dep 对象，如果当前属性的值是对象，再调用 observe ；定义 getter ，收集依赖，返回属性的值；定义 setter ，保存新值，如果新值是对象，调用 observe，派发更新(发送通知)，调用 dep.notify() ;
（4）依赖收集：在 Watcher 对象的 get 方法中调用 pushTarget 记录 Dep.target 属性；访问 data 中的成员时收集依赖， defineReactive 的 getter 中收集依赖；把属性对应的 watcher 对象添加到 dep 的 subs 数组中；给 childOb 收集依赖，目的是子对象添加和删除成员时发送通知；
（5）Watcher：dep.notify 在调用 watcher 对象的 update() 方法时，调用 queueWatcher() ，判断 watcher 是否被处理，如果没有的话添加到 queue 队列中，并调用 flushSchedulerQueue() : 触发 beforeUpdate 钩子，调用 watcher.run() , run() --> get() --> getter() --> updateComponent ，清空上一次的依赖，触发 actived 钩子，触发 updated 钩子。

11、vue的组件化是如何实现的【描述】
一个 Vue 组件就是一个拥有预定义选项的一个 Vue 实例
一个组件可以组成页面上一个功能完备的区域，组件可以包含脚本、样式、模板
先创建父组件再创建子组件，先挂载子组件再挂载父组件
组件注册方式：全局注册和局部注册
全局注册，通过 Vue.component ，然后通过 Vue.extend 将组件配置转换为组件的构造函数，记录到 this.options.components.comp 里
（1）创建一个唯一值的cid ，目的是创建一个包裹着子构造函数通过原型继承并且能够缓存它们
（2）保存Vue构造函数，从缓存中加载组件的构造函数
（3）给 Sub 初始化一个 VueComponent 构造函数，内部调用 _init() 初始化
（4）Sub 原型继承自 Vue ，然后cid自增并记录到 Sub.cid ，合并 options
（5）初始化子组件的 props computed , 将 extend mixin use 等静态方法继承到 Sub
（6）把组件构造函数保存到 Ctor.options.components.comp = Ctor
（7）最后把组件的构造函数缓存到 options._Ctor ，返回 Sub

12、vuex 怎么使用，实现原理【描述】【举例】
使用：
访问 state : this.$store.state.count
访问 getters : this.$store.getters.getCount
触发 mutations : this.$store.commit('increate', 2)
触发 actions : this.$store.dispatch('increateAsync', 5)
还有为了方便使用的 mapState mapGetters mapMutations mapActions
{
  computed: {
    ...mapState('cart', 'cartProducts')
  },
  methods: {
    ...mapMutations('cart', 'deleteFromCart')
  }
}
原理：
可以通过 Vue.use(Vuex) 注册，所以要实现一个对象 Vuex，这个对象包含一个 install 方法
install 方法用于保存 Vue 构造函数，并往 Vue 原型上注册 $store 属性，_Vue.prototype.$store = this.$options.store
然后实现一个 Vuex.Store 类，包含 state getters mutations actions 等属性和方法
实现 state 属性的关键是设置它为响应式数据，this.state = _Vue.observable(state)
实现 getters 的关键是 遍历用户的 getters，为每一个 key 设置 Object.defineProperty 设置 get 方法
然后分别定义 commit 和 dispatch 方法

13、Object.defineProperty 在实际业务中有什么应用？【举例】
可以针对它的特性来说：
value: 设置属性的值
writable: 值是否可以重写。true | false
enumerable: 目标属性是否可以被枚举。true | false
configurable: 目标属性是否可以被删除或是否可以再次修改特性 true | false
getter 是一种获得属性值的方法
setter 是一种设置属性值的方法
在特性中使用 get/set 属性来定义对应的方法
注意：当使用了getter 或 setter 方法，不允许使用 writable 和 value 这两个属性

14、webpack的plugin是怎么实现的【描述】【举例】
开发 Plugin 的思路：
plugin 是通过钩子机制实现的，我们可以在不同的事件节点上挂载不同的任务，就可以扩展一个插件
插件必须是一个函数或者是一个包含 apply 方法的对象
一般可以把插件定义为一个类型，在类型中定义一个 apply 方法
apply 方法接收一个 compiler 参数，包含了这次构建的所有配置信息，通过这个对象注册钩子函数
通过 compiler.hooks.emit.tap 注册钩子函数（emit也可以为其他事件），钩子函数第一个参数为插件名称，第二个参数 compilation 为此次打包的上下文，根据 compilation.assets 就可以拿到此次打包的资源，做一些相应的逻辑处理

15、做的 nuxt 项目，是实时渲染还是后台异步渲染【描述】
使用 asyncData 钩子就是实时渲染16、nuxt应用性能问题瓶颈是什么，怎么优化【描述】【举例】
主要性能问题：
Nuxt 服务端渲染应用最大的性能问题在于 Node 服务端渲染性能，模板转换是 cpu 密集型的操作，node 又是单线程的，并发一高，cpu 就会飙到 100%。
客户端的每次 request 都会到 node 服务器中，触发后端渲染。渲染服务器引入 renderer 和相应的 vue 应用，根据 route 找到相应的组件和数据，拉组件再拉数据（可能是异步的），加载组件产生 DOM，然后再使用 renderToString 吐给 response 。
优化方法：
缓存（页面缓存、组件缓存、API数据缓存）
服务器集群分布式（提高机器的处理数量）
控制好首屏模块化个数，对返回的结果进行精简，最小化，保证吐出到浏览器的内容足够小。就是说并不要对所有模块都做 SSR ，需要首屏呈现的/需要爬虫爬的，就做直出，其他部分做 CSR 就行了
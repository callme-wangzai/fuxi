一面
1、实际开发中用过什么设计模式【描述】
发布-订阅模式，实现 EventEmit 跨组件通信
观察者模式，实现 history 路由变化监听 单例模式 

2、ES6 都有哪些新增的特性【举例】

3、ES6 对象新增了哪些特性【举例】
简洁表示法
属性表达式
Object.is()
Object.assign()
Object.entries()

4、Set 和 Map 在实际业务中有哪些应用【举例】 Set 常用于数组去重，当某个对象的key为非字符串类型时会使用 Map

5、箭头函数和普通函数的区别？【描述】
箭头函数没有 prototype(原型)，所以箭头函数本身没有 this
箭头函数的 this 在定义的时候继承自外层第一个普通函数的 this
如果箭头函数外层没有普通函数，严格模式和非严格模式下它的 this 都会指向 window(全局对象)
箭头函数本身的 this 指向不能改变，但可以修改它要继承的对象的 this
箭头函数的 this 指向全局，使用 arguments 会报未声明的错误
箭头函数的 this 指向普通函数时，它的 argumens 继承于该普通函数
使用 new 调用箭头函数会报错，因为箭头函数没有 constructor
箭头函数不支持 new.target
箭头函数不支持重命名函数参数，普通函数的函数参数支持重命名
箭头函数相对于普通函数语法更简洁优雅

6、为什么构造函数一般用普通函数而不用箭头函数来定义【描述】 使用 new 调用箭头函数会报错，因为箭头函数没有 constructor 箭头函数也没有 prototype

7、call apply 可以改变箭头函数的指向吗【描述】 不能。箭头函数不会改变 this 的指向。this始终指向沿着作用域往上找的第一个 function ，看这个 function 最终是怎样调用的

8、ES6 中 class 的原理【描述】
（1）Class 在语法上更加贴合面向对象的写法
（2）Class 实现继承更加易读、易理解
（3）更易于写 Java 等后端语言的使用
（4）本质还是语法糖，使用 prototype 来实现

9、讲一下原型【描述】

10、哪些是原始类型数据，哪些是引用类型数据，两者不同点【描述】

11、判断数据类型有哪些方法？【举例】

12、为什么 typeof 判断数据类型不精确【描述】

13、说下 instanceof 原理【描述】

14、for in、Object.keys、Object.getOwnPropertyNames 不同点【描述】

for in 主要用于遍历对象的可枚举属性，包括自有属性、继承自原型的属性

Object.keys 返回一个数组，元素均为对象自有的可枚举属性

Object.getOwnPropertyNames 用于返回对象的自有属性，包括可枚举和不可枚举的 15、css position 都有哪些属性值【描述】 static, relative, absolute, fixed, sticky 注意：sticky 粘性定位要理解到位

16、什么是跨域？什么是同源策略【描述】

17、有什么办法解决跨域【描述】

18、讲一下 https 的请求过程【描述】

19、讲讲3次握手和4次挥手的具体过程，syn ack seq 是什么含义【描述】

20、为什么需要3次握手和4次挥手，3次挥手不行吗【描述】
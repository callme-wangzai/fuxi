一面
1、编程题，改造 Person 使其在非new调用时抛出错误【编程】
function Person(name) {
    this.name = name
}
Person("Hello")
new Person("Hello")
参考答案：
function Person(name) {
    if (!(this instanceof Person)) {
        throw Error('error msg')
    }
    this.name = name
}

2、实现 instanceof 方法【编程】
参考答案：
function myIntanceOf (left, right) {
    left = left.__proto__
    var prototype = right.prototype
    while (true) {
        if (left ==null ) return false
        if (left === prototype) return true
        left = left.__proto__
    }
}
function Foo () {}
var f = new Foo()
console.log(myInstanceof(f, Foo)); // true
console.log(myInstanceof(f, Object)); // true
console.log(myInstanceof([1,2], Array)); // true
console.log(myInstanceof({ a: 1 }, Array)); // false

3、结合第1题，Person 是什么类型的对象？是否有 __proto__ 属性？【描述】
Person 是函数对象，有 __proto__ 属性，Person 的隐式原型等于它的构造函数的显式原型
Person.__proto__ === Function.prototype

4、说下 prototype 和 __proto__ 的区别【描述】
5条原型规则：
所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（除了"null"）以外
所有的引用类型（数组、对象、函数），都有一个__proto__ （隐式原型）属性，属性值是一个普通的对象
所有的函数，都有一个 prototype （显式原型）属性，属性值也是一个普通的对象
所有的引用类型（数组、对象、函数），__proto__ 属性值指向它的构造函数的 prototype 属性值
当试图得到一个对象（引用类型）的某个属性时，如果这个对象本身没有这个属性，那么会去它的 __proto__（即它的构造函数的 prototype）中寻找

5、实现 Student 方法，Student 继承 Person ，也有自己的属性和方法【编程】
参考答案：
function Student(grade, name) {
    Person.call(this)
    this.grade = grade
    this.name = name
}
Student.prototype = Object.create(Person.prototype)
Student.prototype.Constructor = Student
Student.prototype.getGrade = function () {
    return this.grade
}

6、完成一个 sum 函数，实现如下功能【编程】
/*
* 编写函数sum
* sum(1)(2).count() // 3
* sum(1)(2)(3).count() // 6
*/
参考答案：
function sum () {
    let args = [...arguments]
    let add = function () {
        args.push(...arguments)
        return add
    }
    add.count = function () {
        return args.reduce((acc, cur) => acc + cur)
    }
    return add
}
console.log(sum(1)(2).count()); // 3
console.log(sum(1)(2)(3).count()); // 6
其他版本，调用方式形如：sum(1)(2)() // 3
function curry (fn) {
    let args = []
    return function f (...newArgs) {
        if (newArgs.length) {
            args = [...args, ...newArgs]
            return f
        } else {
            let res = fn(...args)
            args = []
            return res
        }
    }
}
function count (...args) {
    return args.reduce((a,c) => a+c)
}
let sum = curry(count)
sum(1)(2)() // 3
sum(1)(2)(3)() // 6

7、完成如下 findPath 函数，输入一个对象和对象上的一个节点或子节点的值（值唯一，值类型为字符串），输出该值对应在该对象的key的路径【编程】
const obj = {
    a: {
        a_1: {
            a_1_1: 'abc',
            a_1_2: 'efg'
        }
    },
    b: {
        b_1: 'xyz',
        b_2: '111'
    },
    c: '000'
}
const result = findPath(obj, 'xyz') // ['b', 'b_1']
参考答案：
function flatObj (obj, key = '', res = {}) {
    for (let [k, v] of Object.entries(obj)) {
        if (Object.prototype.toString.call(v) === '[object Object]') {
            let temp = key + k + '.'
            flatObj(v, temp, res)
        } else {
            let temp = key + k
            res[temp] = v
        }
    }
    return res
}

function findPath(obj, value) {
    let o = flatObj(obj)
    // k == 'a.a_1.a_1_1'
    for (let [k, v] in Object.entries(o)) {
        if (v === value) {
            return k.split('.')
        }
    }
    return []
}
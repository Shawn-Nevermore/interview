# JavaScript

- ECMAScript 3/5/6/7/8/9
- DOM（事件对象，拖拽插件封装，发布订阅模式，事件传播/事件代理）
- BOM 
- 设计原理
- 底层原理
  - 堆栈内存
  - 闭包作用域 AO/VO/GO/EC/ECSTACK
  - 惰性函数/柯里化函数/高阶函数
  - 面向对象OOP（new，call，apply，bind，数据类型检测方案，继承方案）
  - this
  - JS运行机制（微任务&宏任务）
  - EventLoop（事件循环，事件队列）
  - 浏览器渲染原理
  - 回流重绘

数据类型检测：typeof instanceof constructor Object.prototype.toString.call()
继承方案：原型继承，call继承，混合继承，寄生组合式继承（最重要），class继承

## Q：对象（数组）的深克隆和浅克隆
```javascript
let obj = { a: 100, b: [10, 20, 30], c: {x:10}, d: /^\d+$/ }
let arr = [10, [100, 200], {x:10, y:20}]

// 深克隆
function deepClone(obj) {
    if(obj === null) return null
    if(typeof obj !== "object") return obj
    if(obj instanceof RegExp) return new RegExp(obj)
    if(obj instanceof Date) return new Date(obj)
    let cloneObj = new obj.constructor  // 这种写法直接是obj原有类型的，更好一点
    for(let key in obj){
        if(obj.hasOwnProperty(key)){
            cloneObj = deepClone(obj[key])
        }
    }
    return cloneObj
}

let obj2=deepClone(obj)
console.log(obj, obj2)
console.log(obj===obj2)
console.log(obj.c===obj2.c)
```

浅克隆：只克隆第一层，且两个对象不===。例子：`let obj1 = {...obj}`就属于浅克隆。
深克隆：`JSON.parse(JSON.stringify(obj))` 可行，但是如果是函数、日期、正则、Symbol，都会出现问题。undefined会被忽略。


## Q：
```javascript
let a = {},
    b = '0',
    c = 0
a[b] = '你好'
a[c] = '中国'
console.log(a[b])

// 引申：对象和数组的区别
-----------------------
let a = {},
    b = Symbol('1'),
    c = Symbol('1')
a[b] = '你好'
a[c] = '中国'
console.log(a[b])

// 引申：自己实现一个Symbol
-----------------------
let a = {},
    b = {n: '1'},
    c = {m: '2'}
a[b] = '你好'
a[c] = '中国'
console.log(a[b])

// Object.prototype.toString 用法、场景，valueOf区别（执行顺序：先执行valueOf,再toString

// 答案：中国，你好，中国
```
结论：
- 数字属性 = 字符串属性，发生覆盖。
- 对象的属性名不一定是字符串
- 对象作为属性名的时候都会转化为字符串'[object Object]'

```javascript
var test = (function(){
    return function () {
        alert(i *= 2)
    }
})(2)
test(5)

// 答案：'4'
```
要点：立即执行函数，闭包，垃圾回收机制

```javascript
var a = 0, b = 0
function A(a) {
    A = function (b) {
        alert(a + b++)
    }
    alert(a++)
}
A(1)
A(2)

// 答案：'1' '4'
```
![](https://i.loli.net/2020/06/30/DKlQeA9pPIxb3f8.png)

```javascript
// 问如何能输出1？
if(a==1 && a==2 && a==3){
    console.log(1)
}
```

涉及到 == 的隐式转换规律：
- null == undefined，但和其他任何都不想等
- NaN != NaN，和其他的都不想等
- String类型 == 对象，对象会调用toString方法
- 其他情况都会转换为Number

答案：
-   `var a = true`
-   `var a = { i:0, toString(){ return ++this.i } }`重写toString方法，这里重写valueOf也可以，因为所有数据类型转换的时候都是先调用valueOf
-   数组shift，`var a=[1,2,3]; a.toString = a.shift`，本质上也是重写toString
-   ES5 数据劫持：
```javascript
var i = 0
Object.defineProperty(window, 'a', {
    get(){
        // GETTER拦截器中不能再次获取当前属性，所以需要i
        return ++i
    }
})
```
-   ES6 Proxy

练习题：
```javascript
var x = 2
var y = {
    x: 3,
    z: (function (x) {
        this.x *= x
        x += 2
        return function (n) {
            this.x *= n
            x += 3
            console.log(x)
        }
    })(x)
}
var m = y.z
m(4)
y.z(5)
console.log(x, y.x)

```

## Q：面向对象面试题
```javascript
function Foo() {
    getName = function(){
        console.log(1)
    }
    return this
}
Foo.getName = function(){
    console.log(2)
}
Foo.prototype.getName = function(){
    console.log(3)
}
var getName = function () {
    console.log(4)
}
function getName() {
    console.log(5)
}
Foo.getName()
getName()
Foo().getName()
getName()
new Foo.getName()
new Foo().getName()
new new Foo().getName()

// 答案：2 4 1 1 2 3 3
```

![](https://i.loli.net/2020/06/30/ozN5OPbkHiltwQC.png)

解析：
- 先函数提升，加载了function Foo, function getName，此时getName输出5，然后加载var getName，覆盖了前面的，此时getName输出4。
- 接着执行代码`Foo.getName()`，直接输出2。
- 执行`getName()`，输出4。
- 执行`Foo().getName()`，先执行Foo()，使得getName输出1，返回的this是window。所以this.getName()就是全局的getName，输出1。
- 执行`getName()`，输出1。
- 执行`new Foo.getName()`和`new Foo().getName()`，根据[运算符优先级](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)，函数调用`…(…)`的优先级是19，成员访问`.`是19，带参数列表的`new`是19，无参列表的`new`是18。所以`new Foo.getName()`先执行`Foo.getName()`，输出2。`new Foo().getName()`从左到右依次执行，`new Foo()`相当于创建实例xxx，实例的getName()方法要从原型上去找，因此`xxx.getName()`输出3。
- 执行`new new Foo().getName()`，从最后一个new开始，`new Foo()`得到实例xxx，然后相当于`new xxx.getName()`，属于无参列表的new，先执行`xxx.getName()`，输出3。

```javascript
function A() {
    alert(1)
}
function Fn(){
    A = function () {
        alert(2)
    }
    return this
}
Fn.A = A
Fn.prototype = {
    A: ()=>{ alert(3) }
}

A()
Fn.A()
Fn().A()
new Fn.A()
new Fn().A()
new new Fn().A()

// 答案：1 1 2 1 3 报错
```
考点：箭头函数不能被new，因为没有原型链，没有构造器函数。所以最后一个会报错

```javascript
var x = 0, y = 1
function fn() {
    x += 2
    fn = function (y) {
        console.log(y + --x)
    }
    console.log(x, y)
}
fn(3)
fn(4)
console.log(x, y)
```

## Q：EventLoop
```javascript
async function async1(){
    console.log('async1 start')
    await async2()
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}
console.log('script start')
setTimeout(() => {
    console.log('settimeout');
}, 0);
async1()
new Promise((resolve) => {
    console.log('promise1')
    resolve()
}).then(() => console.log('promise2'))
console.log('script end')
```
![](https://i.loli.net/2020/06/30/ofG9Ig6qwsZ7HeW.png)

宏任务：定时器，事件绑定，AJAX，IO
微任务：Promise，async/await

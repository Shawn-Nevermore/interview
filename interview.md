# 前端面试要点

## HTML5

怎么理解HTML5？（不光是语义化标签）
- 语义化标签类
- 音视频的处理（video，audio）
- canvas/webGL
- history API（路由——hash模式，history模式）
- requestAnimationFrame（定时器动画弊端：丢帧卡顿）
- 地理位置
- web socket（socket IO——直播，长轮询、长连接的不合理）

实战中，HTML5基础
- 响应式布局：@media/rem/flex
- viewport和dpi适配
- H5的音视频处理方案
- 新表单元素和表单校验

Canvas和Echarts实战
- canvas基础用法
- canvas动画及小游戏开发
- canvas图像处理
- Echarts基础API
- 多元化/个性化数据图标联系
- 异步数据的处理和更新
- Echarts中的事件和行为

## CSS3
- 常规
- 动画
- 盒子模型
- 响应式布局
- sass/less/stylus/postcss

![](https://i.loli.net/2020/06/29/WtyiSuZ2q1CIUBs.png)

### Q：盒子水平垂直居中的方案？
话术：这种需求在我之前的项目中是非常常见的，刚开始我只用了这几种，后来随着CSS3的兴起，Flex非常方便，尤其是在移动端开发，实现起来特别强大，然后我有段时间看了有个叫CSS-tricks的网站，发现了（display:table）这种方式，觉得挺好玩的，就记下来了。

答案：
1. 基于定位的3种
   - top,left 50%; margin再负移动
   - top,left,right,bottom 0; margin: auto;
   - top,left 50%; transform: transition(-50%, -50%);
2. .parent { display: flex; justify-content: center; align-items: center; }
3. JS实现：
4. display: table-cell;

### Q: 谈谈盒模型
话术：其实我们最常用的是标准和模型，它是由content+padding+border组成的，设定的宽高就是content的宽高，并不是最终的宽高，在真实的项目中可能会遇到一系列的问题，比如：……，我觉得比较麻烦。
后来CSS3给我们提供了border-box，这个就很方便，给定宽高就是盒子的宽高，不管我怎么调整内容宽高，盒子都会自己缩放到给定宽高，这样我写样式就很方便，不用来回算值。所以我后来的项目基本都是用border-box。
包括了我后来自己做了简易的UI库，参考了element UI等等，它们的公共样式也基本都默认是border-box，所以我觉得这应该是我们开发中的规范和样式。

答案：
- 标准盒模型（box-sizing: content-box;）
- 怪异盒模型（border-box，包括Flex，Columns多列布局）

### Q: 几大经典布局方案
- 圣杯布局、双飞翼布局：左右固定，中间自适应
- calc法：.center{ width:calc(100%-400px); } （假设左右都是固定的200px，这种方案性能不好）
- Flex布局：父亲：justify-content: space-between; 左右flex: 0 0 200px; 中间flex: 1; 
- 定位法：

### Q：移动端响应式布局
- @media（用于PC端移动端是一套项目）
- rem（用于PC端移动端是两套项目）
- flex
- vh/vw（一般是小项目）

![](https://i.loli.net/2020/06/29/gjPHbuV8x2GNm3W.png)

课后作业的话术：
1. display:none 和 visibility:none 区别，opacity：0 透明度能延伸哪些问题？负margin能做哪些事？z-index 脱离了文档流，还有哪些脱离文档流的方式？（浮动，定位，transform，animation帧动画虚拟平面——只引发了一次回流，性能好）

阿里思考题：不考虑其他因素，下面哪种渲染性能更高？
```css
.box a{
    ...
}

a{
    ...
}
```
答：CSS的渲染机制是，选择器从右向左查询。第一个进行了二次筛选，所以渲染更慢。


## JavaScript
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

### Q：对象（数组）的深克隆和浅克隆
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


### Q：
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

### Q：面向对象面试题
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

### Q：EventLoop
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

## 网络通信层（前后端分离）
- AJAX/Fetch/axios
- HTTP 1.0/2.0（双向通道的好处，什么时候用2.0，HTTPS）
- TCP（三次握手四次挥手）
- 缓存（DNS，强缓存 & 协商缓存）
- 跨域处理方案
- 性能优化

实战中，
- 登录注册的前后端处理机制
- 加密策略：encodeURI-Component，MD5等
- 存储方案：cookie，webStorage，session等
- 用户权限和登陆态的校验处理
- Token的校验处理

### Q：跨域的实现方案
跨域产生的原因：前后端分离
常见问题：你服务器是怎么部署的？（Linux + Nginx + Docker）

#### 1）JSONP
#### 2）iframe
window.name
document.domain
location.hash
post.message
#### 3）CORS跨域资源共享
客户端
登陆验证：第一次向服务器发请求，服务器告诉你成功了，会通过JWT算法生成一个TOKEN，把TOKEN直接给你，当客户端拿到TOKEN之后，可以存储到vuex/localstorage中。再一次发请求，我们需要在请求拦截器上把token都带上，`config.headers.Authorization = token`，即在请求头里带上Authorization字段。服务器再次接收到请求，再通过JWT算法进行校验，如果是之前给的token，说明是合法。这就保证了接口调用信息的安全性。

服务器端
Access-Control-Allow-Origin
#### 4）基于http proxy实现跨域请求

#### 5）Nginx反向代理


## Hybrid APP 小程序
- Hybrid
- Uni-app
- RN 
- Flutter
- 小程序 MPVUE
- Weex
- PWA

小程序开发实战：
- 获取用户信息和地理位置
- 小程序中的支付处理
- mpvue/uni-app框架实战
- 小程序注册和IDE
- rpx适配、模板使用、图片处理
- 小程序中核心组件/API的应用
- 导航及列表渲染（包含无限渲染）
- HTTP-Promise及和服务器的交互处理


## 工程化
- webpack
- git
- linux/nginx（平时项目怎么部署？）

webpack核心配置及优化
- webpack核心基础知识
- 单（多）入口项目打包
- webpack devServer和Proxy代理
- loaders plugins的处理
- CSS样式的抽离、压缩、兼容性处理
- resolve/sourceMap
- less/sass等预编译语言的处理
- 基于babel转换ES6/ES7语法及Polyfill兼容处理
- webpack性能优化（Tree-shaking、CDN加载、热更新、图片压缩等）
- vue-cli3.0脚手架的应用/配置和性能优化

## 全栈
- node
- express
- koa2
- mongodb
- nuxt.js/next.js

node.js/express的全栈开发
- node的安装和npm包管理器
- fs内置模块和PromiseFS封装
- AMD/CMD/CommonJS/ES6Module模块规范深度解析
- HTTP内置模块和搭建web服务器
- 搭建数据服务器和实现Restful API规范的接口
- Express框架基础知识
- express中路由和中间件的实战应用
- 数据Mock和MongoDB数据库应用


## 框架
- Vue
  - 基础知识
  - 核心原理（虚拟DOM，
  - vue-router
  - vue-cli
  - Vuex
  - element UI（PC）
  - vant UI（移动端）
  - cube UI（移动端）
  - SSR
  - 优化

1. Vue基础知识
   - 事件处理及修饰符
   - 表单元素的细节处理
   - 条件和循环渲染
   - vue transition动画和路由切换动效
   - 生命周期钩子函数
   - MVVM双向绑定的原理
   - 受控组件（data）和非受控组件（ref）
   - 常用指令和Vue.directive自定义指令
   - methods/watch/computed/filters
2. Vue组件化开发和Vuex状态管理
   - 属性及属性处理
   - $on/$emit自定义插件
   - slot插槽（具名插槽和作用域插槽）
   - mixin混入和Vue.use插件开发
   - 复合组件信息通信的方案
   - Vue.component全局注册和components局部注册
   - template模板语法和JSX语法
   - Vuex基础：state/getter/mutation/action
   - Vuex的模块化管理和mapXxx遍历
3. Vue Router和UI组件库
   - SPA和MPA对比
   - hash路由和browser路由对比
   - vue-router的基础核心语法
   - 编程式导航和动态理由
   - keep-alive缓存
   - 路由守卫及权限校验
   - Vue-Devtools
   - 单元测试和性能优化

考点：Vue-CLI（2.0向3.0过渡，配置，优化）

### Q：Vue2.0/3.0双向数据绑定的实现原理
ES5：Object.defineProperty
```HTML
姓名：<span id="spanName"></span>
<br />
<input type="text" id="inputName" />
```
```javascript
let obj = {
    name: '',
}
// 深克隆，选一种方式即可
let newObj = JSON.parse(JSON.stringify(obj))
Object.defineProperty(obj, 'name', {
    get() {
        return newObj.name
    },
    set(value) {
        if (value === newObj.name) return
        newObj.name = value
        observe()
    },
})

// Model改变更新View
function observe() {
    spanName.innerHTML = obj.name
    inputName.value = obj.name
}
observe()
setTimeout(() => {
    obj.name = '更新了'
}, 1000)

// View输入改变更新Model
inputName.oninput = function () {
    obj.name = this.value
}

```
弊端：
1. 需要对原始数据克隆
2. 需要分别给对象中每一个属性设置监听
3. 不能监听的可以用$set强制监听，数组的方法重写了



ES6：Proxy
```HTML
 姓名：<span id="spanName"></span>
<br />
<input type="text" id="inputName" />
```
```javascript
let obj = {}
obj = new Proxy(obj, {
    get(target, prop) {
        return target[prop]
    },
    set(target, prop, value) {
        target[prop] = value
        observe()
    },
})

// Model改变更新View
function observe() {
    spanName.innerHTML = obj.name
    inputName.value = obj.name
}
observe()
setTimeout(() => {
    obj.name = '更新了'
}, 1000)

// View输入改变更新Model
inputName.oninput = function () {
    obj.name = this.value
}
```

- React
  - 基础知识
  - 核心原理
  - react-router-DOM
  - redux
  - react-redux
  - dva
  - umi
  - mobix
  - antd
  - antd pro
  - SSR
  - 优化

1. react核心基础知识和Hooks
   - create-react-app脚手架的应用和优化
   - JSX的基础知识和应用
   - 虚拟DOM到真实DOM的渲染原理
   - 属性和状态的管理（set-state）
   - 受控和非受控组件
   - react合成事件和实现双向数据绑定
   - 函数式组件及应用
   - 类组件及应用
   - react.component和react.purecomponent
   - react hooks组件及应用
2. react组件化开发和redux状态管理
   - redux操作流程和应用
   - react-redux的实际应用和各种中间件
   - react中的高阶组件
   - dva 和 UmlJs
   - 组件划分和组件封装思想
   - 复合组件和组件嵌套
   - 基于属性实现数据的的单向（双向）信息传递
   - 构建发布订阅体系完成组件信息交互
   - 基于上下文（react.createContext）实现信息交互 
3. react router和大型项目实战
   - withRouter高阶函数
   - 编程式导航和动态理由
   - Ant Design UI库的实战应用
   - HashRouter && BrowserRouter
   - React Router的基础常规操作

电商实战项目：
- axios/fetch的二次封装
- API接口模块化管理
- 第三方登陆和支付功能
- 项目打包和服务器部署流程


## 游戏方向

白鹭引擎 + TS

## 可视化 or AI方向
webGL，D3.js，tree.js
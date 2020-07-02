# Vue

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

## Q：Vue2.0/3.0双向数据绑定的实现原理
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

## Q：watch 和 computed 和 methods 区别是什么？
思路：先翻译单词，再阐述作用，最后强行找不同。
要点：
- computed 和 methods 相比，最大区别是 computed 有缓存：如果 computed 属性依赖的属性没有变化，那么 computed 属性就不会重新计算。methods 则是看到一次计算一次。
- watch 和 computed 相比，computed 是计算出一个属性（废话），而 watch 则可能是做别的事情（如上报数据）

## Q：Vue组件间通信

- 父子组件：`$emit('xxx', data)` `$on('xxx', function(){})`
- 爷孙组件、兄弟组件 eventBus
```javascript
var eventBus = new Vue()
eventBus.$emit() 
eventBus.$on()
```
-   Vuex


## Q：你是怎么用Vuex的？

背下文档第一句：Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。
说出核心概念的名字和作用：State/Getter/Mutation/Action/Module
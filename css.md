
# CSS3

- 常规
- 动画
- 盒子模型
- 响应式布局
- sass/less/stylus/postcss

![](https://i.loli.net/2020/06/29/WtyiSuZ2q1CIUBs.png)

## Q：盒子水平垂直居中的方案？
话术：这种需求在我之前的项目中是非常常见的，刚开始我只用了这几种，后来随着CSS3的兴起，Flex非常方便，尤其是在移动端开发，实现起来特别强大，然后我有段时间看了有个叫CSS-tricks的网站，发现了（display:table）这种方式，觉得挺好玩的，就记下来了。

答案：
1. 基于定位的3种
   - top,left 50%; margin再负移动
   - top,left,right,bottom 0; margin: auto;
   - top,left 50%; transform: transition(-50%, -50%);
2. .parent { display: flex; justify-content: center; align-items: center; }
3. JS实现：
4. display: table-cell;

## Q: 谈谈盒模型
话术：其实我们最常用的是标准和模型，它是由content+padding+border组成的，设定的宽高就是content的宽高，并不是最终的宽高，在真实的项目中可能会遇到一系列的问题，比如：……，我觉得比较麻烦。
后来CSS3给我们提供了border-box，这个就很方便，给定宽高就是盒子的宽高，不管我怎么调整内容宽高，盒子都会自己缩放到给定宽高，这样我写样式就很方便，不用来回算值。所以我后来的项目基本都是用border-box。
包括了我后来自己做了简易的UI库，参考了element UI等等，它们的公共样式也基本都默认是border-box，所以我觉得这应该是我们开发中的规范和样式。

答案：
- 标准盒模型（box-sizing: content-box;）
- 怪异盒模型（border-box，包括Flex，Columns多列布局）

## Q: 几大经典布局方案
- 圣杯布局、双飞翼布局：左右固定，中间自适应
- calc法：.center{ width:calc(100%-400px); } （假设左右都是固定的200px，这种方案性能不好）
- Flex布局：父亲：justify-content: space-between; 左右flex: 0 0 200px; 中间flex: 1; 
- 定位法：

## Q：移动端响应式布局
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

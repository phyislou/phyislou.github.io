<div style='display:flex;justify-content:space-between;'>
<div>上一节：<a href='./元素渲染.md'>元素渲染</a></div>
<div><a href='../React概述.md'>章节列表</a></div>
<div>下一节：<a href='./State&生命周期.md'>State&生命周期</a></div>
</div>

***

# 组件&Props

> 组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于JS函数。它接受任意的入参（也就是“props”），并返回用于描述页面展示内容的React元素。

### 函数组件与class组件

函数组件是较为简单的实现方式：
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
它接收一个唯一的输入参数 “props”对象与并返回一个React元素。

而另外的class组件，则是使用了[ES6的class](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)：
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
就像代码中显示的，虽然这里只是简单的类实现，但为了之后能够使用React自身的特性，请务必添加一个继承类`React.Component`。  
另外地，本段的两个代码是等效的。

* 注意：之后将会以class组件为基础讲解，但由于函数组件是之后React Hook的编写基础，所以也是需要比较熟悉的哦。

### 渲染组件


***

<div style='display:flex;justify-content:space-between;'>
<div>上一节：<a href='./元素渲染.md'>元素渲染</a></div>
<div><a href='../React概述.md'>章节列表</a></div>
<div>下一节：<a href='./State&生命周期.md'>State&生命周期</a></div>
</div>


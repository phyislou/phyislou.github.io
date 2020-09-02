「[元素渲染](./元素渲染.md)」<--「[章节列表](../React概述.md)」-->「[State&生命周期](./State&生命周期.md)」

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

之前，我们遇到的React元素都只是由DOM标签（`<div></div>`、`<img />`等等）组成的元素，但事实上React元素也是可以由用户自定义组件的：
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
当React元素为用户自定义组件时，它会将JSX所接收的属性（在这里就是`name="Sara"`）以及子组件转换为单个对象传递给组件，这个对象被称之为 “props”。

让我们来按照步骤看一看这段代码中发生了什么：
1. 我们调用`ReactDOM.render()`函数，并传入`<Welcome name="Sara" />`作为参数。
2. React 调用 Welcome 组件，并将`{name: 'Sara'}`作为 props 传入。
3. Welcome 组件将`<h1>Hello, Sara</h1>`元素作为返回值。
4. React DOM 将 DOM 高效地更新为`<h1>Hello, Sara</h1>`。

* 注意:**组件名称必须以大写字母开头**。
  
  React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 Welcome。想要了解其原因可以查看[深入JSX](../AdvanacedGuides/深入JSX.md)。

### 组合组件

***

「[元素渲染](./元素渲染.md)」<--「[章节列表](../React概述.md)」-->「[State&生命周期](./State&生命周期.md)」
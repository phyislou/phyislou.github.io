「[元素渲染](./02-元素渲染.md)」<--「[章节列表](../React概述.md)」-->「[State&生命周期](./04-State&生命周期.md)」

***

# 组件&Props

> 组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于JS函数。它接受任意的入参（也就是“props”），并返回用于描述页面展示内容的React元素。

### 函数组件与class组件

函数组件是较为简单的实现方式：
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
它接收一个唯一的输入参数 “props”对象与并返回一个React元素。

而另外的class组件，则是使用了[ES6的class](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)：
```jsx
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
```jsx
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

组件可以在输出中引用其他组件，形成组件的套用，这可以使我们能够将不同层级的逻辑分别实现到各自的组件中，比如按钮，表单，对话框等功能的实现，均可以封装到不同的组件当中。  
例如，我们可以创建一个可以多次渲染Welcome组件的App组件：
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```
一般情况下，每个新的React应用程序的顶层组件都是App组件。

### 提取组件

我们也可以将一个复杂逻辑组件拆分为更小的组件。  
例如，参考如下Comment组件：
```jsx
//原始的Comment组件
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
该组件用于描述一个社交媒体网站上的评论功能，它接收:
* author（对象）
* text （字符串）
* date（日期）

三者作为props。

由于组件嵌套了许多层的关系，难以组件代码难以阅读，难以维护，且难以复用它的各个部分。因此，我们可以从中提取一些组件出来。

首先，我们将提取 Avatar 组件：
```jsx
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
Avatar并不需知道它在Comment组件内部是如何渲染的。因此，我们给它的props起个更通用的名字：user————而不是author。
* 注意：建议从组件自身的角度命名props，而不是依赖于调用组件的上下文命名。

针对Comment组件，我们再做些微小的调整：
```jsx
//第一次提取后的Comment组件
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

接下来，再让我们来提取UserInfo组件，该组件渲染用户名以及刚才的Avatar组件：
```jsx
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

将Avatar组件用UserInfo组件包裹进去后，我们就可以进一步地简化Comment组件了：
```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

这样，一个有着三层逻辑（Comment->UserInfo->Avatar）的组件就完成了，像是UserInfo、Avatar这样的组件也可以在其他地方进行多次复用。根据经验来看，如果UI中有一部分被多次使用（Button，Panel，Avatar），或者组件本身就足够复杂（App，FeedStory，Comment），那么它就是一个可提取出独立组件的候选项。

### Props的只读性

组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。这是实现react的核心特征“单向数据流动”所必须的基本要求。

来看下两个函数：
```jsx
function sum(a, b) {
  return a + b;
}

function withdraw(account, amount) {
  account.total -= amount;
}
```
第一个sum函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。这样的函数就被称为“纯函数”。  
而第二个withdraw函数更改了自己的入参，所以它就不是纯函数。

依据以上的说明，React有一个严格的规则：

> **所有React组件都必须像纯函数一样保护它们的props不被更改。**

当然，应用程序的UI是动态的，并会伴随着时间的推移而变化。在下一节中，我们将学习react的基本特征之一“state”。在不违反上述规则的情况下，state允许React组件随着我们操作、网络响应或者其他变化而动态更改输出内容。

***

「[元素渲染](./02-元素渲染.md)」<--「[章节列表](../React概述.md)」-->「[State&生命周期](./04-State&生命周期.md)」
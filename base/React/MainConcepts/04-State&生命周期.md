「[组件&Props](./03-组件&Props.md)」<--「[章节列表](../React概述.md)」-->「[事件处理](./05-事件处理.md)」

***

# State&生命周期

在「[元素渲染](./02-元素渲染.md)」一节中，我们只了解了一种更新UI界面的方法。也就是通过调用`ReactDOM.render()`来修改我们想要渲染的元素。  
我们当时说：“一般仅用`ReactDOM.render()`来做初次渲染”，那么在本节中，我们将学习react中常用的更新UI的方法。正如在「元素渲染」一节中实现的计时器的例子一样，在本节中我们将实现一个真正可复用的Clock组件。它将每秒更新一次时间。

首先，我们原始的组件代码应该如下例一样，采用`ReactDOM.render()`来做渲染：
```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

理想情况下，我们希望抛弃定时器，只编写一次代码，便可以让Clock组件自我更新：
```jsx
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
我们需要在Clock组件中添加 “state” 来实现这个功能。

state结构上与props类似，但区别在于state是私有的，并且完全受控于当前组件。

### 将函数组件转换成class组件

通过以下五步将Clock的函数组件转成class组件：

1. 创建一个同名的 ES6 class，并且继承于`React.Component`。
2. 添加一个空的`render()`方法。
3. 将函数体移动到`render()`方法之中。
4. 在`render()`方法中使用`this.props`替换`props`。
5. 删除剩余的空函数声明。

于是Clock组件变成了：
```jsx
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

每次组件更新时其render方法都会被调用，但只要在相同的DOM节点中渲染`<Clock />`，就仅有一个Clock组件的class实例被创建使用（也就是说，render方法里return的内容一旦发生变化，render方法就会被再次调用，且对于同一个DOM节点再次调用render，并不会return一个新的节点，而是替换了之前的旧节点）。  
这就使得我们可以使用如state或生命周期方法等很多其他特性。

### 向class组件中添加局部的state

***

「[组件&Props](./03-组件&Props.md)」<--「[章节列表](../React概述.md)」-->「[事件处理](./05-事件处理.md)」
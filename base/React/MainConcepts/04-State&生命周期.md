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

我们通过以下三步将date从props移动到state中：

1. 把`render()`方法中的`this.props.date`替换成`this.state.date`，于是将从父组件获取的数据（props）改为从本组件内（state）获取：
    ```jsx
    class Clock extends React.Component {
      render() {
        return (
          <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
          </div>
        );
      }
    }
    ```
2. 添加一个class构造函数，然后在该函数中为`this.state`赋初值：
    ```jsx
    class Clock extends React.Component {
      constructor(props) {
        super(props);//注意这里
        this.state = {date: new Date()};
      }

      render() {
        return (
          <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
          </div>
        );
      }
    }
    ```
    在这个构造器中，在使用`this.state`之前，先定义了一句`super(props)`，请务必要写上这一句，不然this就失效了，详细的原因请看：[super(props)](https://segmentfault.com/a/1190000018084870)。
    `super(props)`将props传递到父类的构造函数中，我们在Class组件中应该始终使用props参数来调用父类的构造函数。
3. 由于在`this.state`中添加了data属性，所以要移除`<Clock />`元素中的date属性：
    ```jsx
    ReactDOM.render(
      <Clock />,
      document.getElementById('root')
    );
    ```

经过这三步，我们已经将Clock组件改造完成。
```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

但事实上由于对组件中的state我们并没有用计时器去每秒更新它，所以现在组件还不会刷新。

### 将生命周期方法添加到Class中

在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

对于上例中的Clock组件，它第一次被渲染到DOM中的时候，我们就需要为其设置一个计时器。这在React中被称为“**挂载（mount）**”。

同时，当DOM中的Clock组件被删除的时候，我们应该要清除计时器。这在React中被称为“**卸载（unmount）**”。

我们可以为class组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法，也就是所谓的“生命周期方法”：
```jsx
class Clock extends React.Component {
  /*---生命周期的初始化阶段开始---*/
  //defaultProps //step1:设置组件的默认属性
  constructor(props) {/*---code---*/} //step2:设置组件的初始化状态
  //componentWillMount() //step3:被废弃组件
  //render() //step4:组件渲染
  componentDidMount() {} //step5:组件已经被渲染到页面中后触发
  /*---生命周期的初始化阶段结束---*/
  /*---生命周期的运行阶段开始---*/
  //componentWillReceiveProps(nextProps) {} //step1:被废弃组件，组件接收到属性时触发
  shouldComponentUpdate(nextProps, nextState) {} //step2:当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发,这个生命周期的返回值是布尔值，会显示后续是否进行重新的渲染。详情请查看“高级指引：性能优化”一节
  //componentWillUpdate(nextProps, nextState) {} //step3:被废弃组件，组件即将被更新时触发
  componentDidUpdate(nextProps, nextState) {} //组件被更新完成后触发。页面中产生了新的DOM的元素，可以进行DOM操作
  /*---生命周期的运行阶段结束---*/
  /*---生命周期的销毁阶段开始---*/
  componentWillUnmount() {} //对轮询的请求的清理，或是对定时器的清理。
  /*---生命周期的销毁阶段结束---*/

  render() {/*---code---*/}
}
```

作为对生命周期的描述，我们可以先来看一下这张图：
![react-04-1](../../../img/react-04-1.png)

***

「[组件&Props](./03-组件&Props.md)」<--「[章节列表](../React概述.md)」-->「[事件处理](./05-事件处理.md)」
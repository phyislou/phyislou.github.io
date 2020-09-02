上一节：[React概述](..\React概述.md)

# JSX简介

上一节中已经实现了简单的jsx代码，总结就是：**由<></>、< />包裹的是html语言，由{}包裹的是js语言**  
这一节展开讲讲JSX的更多特性。

### 使用JSX的原因

在传统的前端编程中，渲染逻辑本质上是与其他 UI 逻辑内在耦合的。比如一个网页中有大量的`<div>、<button>、<input>`等标签，需要与JS进行诸如事件绑定、表单提交、移动变换等大量操作，JS与UI展示数据之间交互频繁。  
React 并没有采用“将标记（h5）与逻辑（js）分离到不同文件”这种传统的分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

### JSX运用

上一节已经简单的使用了一下JSX，这次我们将在{}当中，插入函数：
```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```
当然，在括号内部，js语言可以按照正常的换行方式进行换行。

### JSX作为表达式

JSX事实上是一个表达式，编译后它会变成一个函数调用，返回一个函数对象。换句话说，我们可以在if/for循环中使用JSX，可以将JSX赋值给变量，也可以把JSX当作参数传入，也可以将JSX作为函数的返回值：
```js
function func(para) {
  if (para) {
    return <h1>balabala</h1>;
  }
  return <h1>barabara</h1>;
}
```
注意：将JSX作为函数的函数的输入参数与返回值，这一点正是之后用来实现react组件的基本特征。

### JSX中的标签属性

与正常的html标签写法相同，我们可以这样写：
```js
const element = <div tabIndex="0"></div>;
```
相对应的，使用JSX的格式也是可以的
```js
const element = <div tabIndex={table.ind}></div>;
const element = <img src={user.avatarUrl}></img>;
```
注意1：属性值要不是"..."，要不是{...}，两者不要混用哦。
注意2：React DOM使用JS的`camelCase`（小驼峰命名）来定义属性的名称，比如，JSX里的`class`变成了`className`，而`tabindex`则变为`tabIndex`。

### JSX的行内样式

JSX设置外联样式是很简单的：
```js
//文件“style1.css”为：
{
  .divStyle {
    background-color: red;
    font-size: 32;
  }
}

//则在JSX中：
import Style1 from  './style1.css';
const element = <div className={Style1.divStyle}></div>;
```

而如果想要使用行内样式时，我们需要写成：
```js
const element = <div style={{backgroundColor: 'red', fontSize: 32}}></div>;
```
基本格式就是`style={{attr1: val, attr2: val, ...}}`，外面一层大括号表示里面为JS代码，里面一层大括号表示这是一个对象，所以上述代码也可以写成：
```js
const eleStyle = {backgroundColor: 'red', fontSize: 32};
const element = <div style={eleStyle}></div>;
```
其中要注意的有两点：
1. 属性名要从css的格式改变为小驼峰命名格式，并且在JSX中仅支持固定的几十个常用属性名（也就是说不支持像“--customColor”这样的自定义属性）
2. 由于实际上是一个js对象，所以属性值只能为字符串或者数字，像“0.8em”这样的值不能直接写入，请先改成字符串`fontSize: '0.8em'`

<br />

下一节：[元素渲染]()
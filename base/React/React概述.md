# React概述

本节参考网页：

* [官方文档与教程](https://zh-hans.reactjs.org/)
* [github的代码仓库](https://github.com/facebook/react)

### 网页框架

一般而言，前端三大框架说的就是angular、vue、react。以现在的情况来说：
* angular：谷歌开发，相比之下上手难度大，有许多譬如依赖注入之类的后端概念。
* vue：著名的尤雨溪老师开发，上手难度较为简单，国内应用面也很广。
* react：Facebook开发，上手难度一般，应用面最广。

对于框架的选择问题也可以参考[尤大的答案](https://www.quora.com/Which-should-I-learn-Mithril-Vue-or-Angular/answer/Evan-You-3)，但无论如何，先打好JS的基础是必须的。  
我在这里选择的是react，主要的原因还是其组件化的理念与庞大的生态环境，当然，也是为了之后能够融入ReactNative跨平台开发做准备。（选择RN的原因还是从react衔接到RN足够简单，不像flutter与weex(uni-app)）

### 创建一个模板应用

Facebook官方提供了一个已经配置好的SPA单页应用`Create React App`，非常适合新手参考学习。实现方式如下：
```shell
npm install -g create-react-app # 全局安装create-react-app

npx create-react-app my-app # 创建一个my-app的项目

cd my-app # 打开项目目录

npm start # 编译当项目，并打开 http://localhost:3000/
```

打开网页后就可以看到网页效果了（一个react的图标在旋转）。

现在来分析一下网页代码的构成。首先，在`npm start`编译时，程序首先找到根目录下`package.json`配置文件中的`scripts`字段，并执行其中的`start`命令：
```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
```

可以见到，实际上执行的是`react-scripts start`命令，然后在根目录的node_modules（模块管理）文件夹中找到react-scripts插件，在`bin/react-scripts.js`之中有下列代码：
```js
const scriptIndex = args.findIndex(
  x => x === 'build' || x === 'eject' || x === 'start' || x === 'test'
); //寻找指令位置
const script = scriptIndex === -1 ? args[0] : args[scriptIndex]; //找到指令，输入“react-scripts start”的话，此处的值即为“start”
const nodeArgs = scriptIndex > 0 ? args.slice(0, scriptIndex) : [];

if (['build', 'eject', 'start', 'test'].includes(script)) {
  const result = spawn.sync(
    'node',
    nodeArgs
      .concat(require.resolve('../scripts/' + script)) //去文件夹“scripts”下面寻找script（即为start）命令
      .concat(args.slice(scriptIndex + 1)),
    { stdio: 'inherit' }
  );
  /*---code---*/
} else {
  /*---code---*/
  );
}
```

打开`start.js`，我们可以在`compiler -> config -> configFactory -> config -> webpack.config`（即`config/webpack.config.js`）中找到webpack的配置文件，我们的项目代码就定义在entry参数的`paths.appIndexJs`中：
```js
  return {
    entry: [
      /*---notes---*/
      isEnvDevelopment &&
        require.resolve('react-dev-utils/webpackHotDevClient'),
      // Finally, this is your app's code:
      paths.appIndexJs,
      // We include the app code last so that if there is a runtime error during
      // initialization, it doesn't blow up the WebpackDevServer client, and
      // changing JS code would still trigger a refresh.
    ].filter(Boolean),
    /*---code---*/
  }
```
关于如何配置webpack请查看[webpack](base\Webpack\Webpack.md)一章。  
在这里，大部分的路径都配置在`config/paths.js`中，而上面的`appIndexJs`参数就表明入口正是在根目录下的`src/index`中：
```js
module.exports = {
  /*---code---*/
  appIndexJs: resolveModule(resolveApp, 'src/index'),
  /*---code---*/
};
```
于是我们打开根目录下的`src/index.js`，这个入口函数非常简短：
```js
import React from 'react'; //导入react的核心功能
import ReactDOM from 'react-dom'; //将虚拟DOM渲染到DOM中
import './index.css';
import App from './App'; //导入App文件，这里是主体代码的部分
import * as serviceWorker from './serviceWorker'; //导入离线缓存包

ReactDOM.render( //react方法，将ReactElement实例渲染到指定id的元素中
  <React.StrictMode>
    {/*在所有代码（也就是<App />）外包裹一层严格模式，
      *当然，这里面开始需要用JSX语法了*/}
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister(); //正如上方的原文注释，这一行是用来取消注册离线缓存的。
```
`index.js`中做了两件事：1. 给代码添加严格模式；2. 取消注册了离线缓存，这两点都好理解，但难点在于在`ReactDOM.render`中我们碰上了react自创的模板语言JSX。  
首先让我们来看看`ReactDOM.render`方法的API：
```js
ReactDOM.render(element, container[, callback])
```
它在提供的container（DOM节点）中渲染（或更新）element（即react元素），如果有callback，则会在渲染（或更新）完毕后执行回调函数。  
我们查看根目录的`public/index.html`（我们实际操作的网页），其中的主体部分仅有一行`<div id="root"></div>`，


### 接下来看

### 拓展

想要了解create-react-app的构建过程或是要从零开始构建一个react项目的话，请见：[实现create-react-app](base\React\拓展：实现create-react-app.md)

想要了解`ReactDOM.render`的具体运作方式的话，请见：[ReactDOM.render，初次渲染](https://www.jianshu.com/p/2d8062e94ed3)
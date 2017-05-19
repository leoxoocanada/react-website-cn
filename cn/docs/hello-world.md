[文档](docs/hello-world.md) | [教程](tutorial/tutorial.md) | [社区](community/support.md) | [博客](_posts/2017/04/07/react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)


# Hello World

开始使用 React 最简单的方式是使用在 CodePen 上的 [ Hello World 示例](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)。无须安装任何环境，只需要在其它标签打开它然后跟着我们的例子去做，如果你更喜欢使用本地开发环境，可以访问 [安装页面](/cn/docs/installation.md) .

最小的 React 例子如下:


```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```
它在页面上输出一个标题"Hello, world!" 。

接下来的章节将循序渐进的介绍如何使用 React。我们会学习 React 应用的构建块: 元素和组件。一旦你掌握了它们，你可以使用小的可复用片断创建复杂的应用。

## 关于 JavaScript 的说明

React 是一个 JavaScript 库, 所以需要你对 JavaScript 知识有一个基本的了解。如果你不是很自信，我们推荐看一下 [refreshing your JavaScript knowledge](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) ，这样才能更容易跟上脚步。

在示例中我们会使用一些 ES6 语法。因为它比较新，所以我们尽量少的使用它，但是我们鼓励你使用如下常用的语法[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [类](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [模板字符串](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 声明。你可以使用 [Babel 转换器](http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A) 编译E6代码。

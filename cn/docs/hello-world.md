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

下一个章节将循序渐进的带你使用 React。我们将检查 React 应用程序构建块: 元素和组件。一旦你掌握了它们，你可以创建复杂的可复用的应用。

## A Note on JavaScript

React is a JavaScript library, and so it assumes you have a basic understanding of the JavaScript language. If you don't feel very confident, we recommend [refreshing your JavaScript knowledge](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) so you can follow along more easily.

We also use some of the ES6 syntax in the examples. We try to use it sparingly because it's still relatively new, but we encourage you to get familiar with [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) statements. You can use <a href="http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A">Babel REPL</a> to check what ES6 code compiles to.

# Hello World

要开始使用 React 最简单的方式是 [查看在 CodePen 上的 Hello World 示例代码](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)。你不需要安装任何软件，你可以只在另外一个 tab 里打开它，并且通过示例跟着我们一起学习，假如你一定要使用本地的开发环境，请跳转到 [安装页面](/cn/docs/installation.md) .

最简单的 React 例子如下:


```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

It renders a header saying "Hello, world!" on the page.

The next few sections will gradually introduce you to using React. We will examine the building blocks of React apps: elements and components. Once you master them, you can create complex apps from small reusable pieces.

## A Note on JavaScript

React is a JavaScript library, and so it assumes you have a basic understanding of the JavaScript language. If you don't feel very confident, we recommend [refreshing your JavaScript knowledge](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) so you can follow along more easily.

We also use some of the ES6 syntax in the examples. We try to use it sparingly because it's still relatively new, but we encourage you to get familiar with [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) statements. You can use <a href="http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A">Babel REPL</a> to check what ES6 code compiles to.

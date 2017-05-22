[文档](docs/hello-world.md) | [教程](tutorial/tutorial.md) | [社区](community/support.md) | [博客](_posts/2017/04/07/react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

# 安装

React 能被灵活的运用在各种各样的项目里。你能用它来创建新应用，但是你也能循序渐进的将它引入到现有的代码库中，而无须重构。

下面哪些选项最能描述你想要做什么？ 

* 试用 React
* 创新新的应用
* 在现有应用中使用 React

## 试用 React

如果你只想玩一下 React，你可以使用 CodePen。尝试从这个 [Hello World 示例代码](http://codepen.io/gaearon/pen/rrpgNB?editors=0010)开始。你不需要安装任何环境；你只需要对代码做修改，并且看看它是否会生效。

如果你更喜欢用你自己的文本编辑器，你也可以 [下载这个HTML文件](https://facebook.github.io/react/downloads/single-file-example.html) 进行编辑，然后在本地文件系统中使用浏览器打开它，它会执行一个缓慢的运行时代码转换，所以不要在生产环境使用它。

如果你要在一个完整的应用中使用它，有两种比较流行的使用 React 的方式: 使用Create React App、在现有代码库中使用。


## 创建新应用

[Create React App](http://github.com/facebookincubator/create-react-app) 是用来创建一个新的 React 单页应用的最佳方式。它已经为你设置好了开发环境，以便你可以使用最新的 JavaScript 特性，提供一种不错的开发体验，并且可以优化你的生产环境应用。

```bash
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

Create React App 不能处理后端逻辑或数据库；它只是创建一种前端构建管道，所以你可以使用任何你想要的后端语言。在底层它使用像 Babel 和 webpack 这样的构建工具，但是不需要任何配置就可以使用。

当你准备部署到到生产环境，运行 `npm run build` 命令将在 `build` 文件夹为你的应用创建一个优化好的构建。你可以在 [它的 README 文件](https://github.com/facebookincubator/create-react-app#create-react-app-) 和 [用户手册](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#table-of-contents) 中了解有关 Create React App 的更多信息。


## 在现有应用中使用 React

你不必为了开始使用 React 而重写原来的应用。

我们建议你在应用中的一小部分中添加 React , 例如个别小部件，这样你可以看看它是否适用于你的用例。

虽然 React 在没有构建管道的情况下 [也可以使用](/cn/docs/react-without-es6.html) ，我们还是建议你设置一下，这样效率会更高。一个现代的构建管道通常包括这些：

* 一个**包管理器**, 如 [Yarn](https://yarnpkg.com/) 或 [npm](https://www.npmjs.com/)。 它让你可以利用大量的第三方软件包生态系统，并且容易安装或更新它们。
* 一个**打包工具**, 如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/)。 它让你可以编写模块化代码并且可以将它们打包在一起成为一个小包，以实现加载性能的优化，节省加载时间。
* 一个**编译器** 如 [Babel](http://babeljs.io/)。 它可以让你在编写现代 JavaScript 代码的同时兼容旧的浏览器里。

### Installing React

>**Note:**
>
>Once installed, we strongly recommend setting up a [production build process](/react/docs/optimizing-performance.html#use-the-production-build) to ensure you're using the fast version of React in production.

We recommend using [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/) for managing front-end dependencies. If you're new to package managers, the [Yarn documentation](https://yarnpkg.com/en/docs/getting-started) is a good place to get started.

To install React with Yarn, run:

```bash
yarn init
yarn add react react-dom
```

To install React with npm, run:

```bash
npm init
npm install --save react react-dom
```

Both Yarn and npm download packages from the [npm registry](http://npmjs.com/).

### Enabling ES6 and JSX

We recommend using React with [Babel](http://babeljs.io/) to let you use ES6 and JSX in your JavaScript code. ES6 is a set of modern JavaScript features that make development easier, and JSX is an extension to the JavaScript language that works nicely with React.

The [Babel setup instructions](https://babeljs.io/docs/setup/) explain how to configure Babel in many different build environments. Make sure you install [`babel-preset-react`](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-) and [`babel-preset-es2015`](http://babeljs.io/docs/plugins/preset-es2015/#basic-setup-with-the-cli-) and enable them in your [`.babelrc` configuration](http://babeljs.io/docs/usage/babelrc/), and you're good to go.

### Hello World with ES6 and JSX

We recommend using a bundler like [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/) so you can write modular code and bundle it together into small packages to optimize load time.

The smallest React example looks like this:

```js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

This code renders into a DOM element with the id of `root` so you need `<div id="root"></div>` somewhere in your HTML file.

Similarly, you can render a React component inside a DOM element somewhere inside your existing app written with any other JavaScript UI library.

[Learn more about integrating React with existing code.](/react/docs/integrating-with-other-libraries.html#integrating-with-other-view-libraries)

### Development and Production Versions

By default, React includes many helpful warnings. These warnings are very useful in development.

**However, they make the development version of React larger and slower so you should use the production version when you deploy the app.**

Learn [how to tell if your website is serving the right version of React](/react/docs/optimizing-performance.html#use-the-production-build), and how to configure the production build process most efficiently:

* [Creating a Production Build with Create React App](/react/docs/optimizing-performance.html#create-react-app)
* [Creating a Production Build with Single-File Builds](/react/docs/optimizing-performance.html#single-file-builds)
* [Creating a Production Build with Brunch](/react/docs/optimizing-performance.html#brunch)
* [Creating a Production Build with Browserify](/react/docs/optimizing-performance.html#browserify)
* [Creating a Production Build with Rollup](/react/docs/optimizing-performance.html#rollup)
* [Creating a Production Build with Webpack](/react/docs/optimizing-performance.html#webpack)

### Using a CDN

If you don't want to use npm to manage client packages, the `react` and `react-dom` npm packages also provide single-file distributions in `dist` folders, which are hosted on a CDN:

```html
<script src="https://unpkg.com/react@15/dist/react.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.js"></script>
```

The versions above are only meant for development, and are not suitable for production. Minified and optimized production versions of React are available at:

```html
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```

To load a specific version of `react` and `react-dom`, replace `15` with the version number.

If you use Bower, React is available via the `react` package.


---
<details>
  <summary>文档导航</summary>

#### 快速入门

* [**`安装`**](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md")
* [JSX 介绍](/cn/docs/introducing-jsx.md)
* [渲染元素](/cn/docs/rendering-elements.md)
* [组件和Props](/cn/docs/components-and-props.md)
* [State和生命周期](/cn/docs/state-and-lifecycle.md)
* [事件处理](/cn/docs/handling-events.md)
* [条件渲染](/cn/docs/conditional-rendering.md)
* [列表和键](/cn/docs/lists-and-keys.md)
* [表单](/cn/docs/forms.md)
* [状态提升](/cn/docs/lifting-state-up.md)
* [组合 vs 继承](/cn/docs/composition-vs-inheritance.md)
* [用 React 思考](/cn/docs/thinking-in-react.md)

#### 高级教程

* [深入JSX](/cn/docs/jsx-in-depth.md)
* [使用 PropTypes 做类型检查](/cn/docs/typechecking-with-proptypes.md)
* [Refs 和 DOM](/cn/docs/refs-and-the-dom.md)
* [不可控组件](/cn/docs/uncontrolled-components.md)
* [性能优化](/cn/docs/optimizing-performance.md)
* [不使用 ES6 的 React](/cn/docs/react-without-es6.md)
* [不使用 JSX 的 React](/cn/docs/react-without-jsx.md)
* [一致性比较（Reconciliation）](/cn/docs/reconciliation.md)
* [上下文（Context）](/cn/docs/context.md)
* [Web Components](/cn/docs/web-components.md)
* [高阶组件](/cn/docs/higher-order-components.md)
* [与其它类库集成](/cn/docs/integrating-with-other-libraries.md)

#### 参考

* [React](/cn/docs/react-api.md)
* [React.Component](/cn/docs/react-component.md)
* [ReactDOM](/cn/docs/react-dom.md)
* [ReactDOMServer](/cn/docs/react-dom-server.md)
* [DOM 元素](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)

#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

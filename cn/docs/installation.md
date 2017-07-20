[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

---
<details>
  <summary>导航</summary>

#### 快速入门

* [**`安装`**](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md)
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

虽然 React 在没有构建管道的情况下 [也可以使用](/cn/docs/react-without-es6.md) ，我们还是建议你设置一下，这样效率会更高。一个现代的构建管道通常包括这些：

* 一个**包管理器**, 如 [Yarn](https://yarnpkg.com/) 或 [npm](https://www.npmjs.com/)。 它让你可以利用大量的第三方软件包生态系统，并且容易安装或更新它们。
* 一个**打包工具**, 如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/)。 它让你可以编写模块化代码并且可以将它们打包在一起成为一个小包，以实现加载性能的优化，节省加载时间。
* 一个**编译器** 如 [Babel](http://babeljs.io/)。 它可以让你在编写现代 JavaScript 代码的同时兼容旧的浏览器里。

### 安装 React

>**提示:**
>
>一旦安装好，我们强烈建议设置一个 [生产构建过程](/cn/docs/optimizing-performance.md#使用生产版本) 确保你在生产环境中使用最新版本的 React 。

我们建议使用 [Yarn](https://yarnpkg.com/) 或 [npm](https://www.npmjs.com/) 来管理前端依赖。如果你对包管理不熟悉，这篇 [Yarn 文档](https://yarnpkg.com/en/docs/getting-started) 是一个非常好的起点。

通过 Yarn 安装 React，运行：

```bash
yarn init
yarn add react react-dom
```

通过 npm 安装 React，运行：

```bash
npm init
npm install --save react react-dom
```

Yarn 和 npm 都会从 [npm 仓库](http://npmjs.com/) 下载所需要的包。


### 启用 ES6 和 JSX

我们建议你配合 [Babel](http://babeljs.io/) 使用 React，这样可以让你在 JavaScript 代码中使用 ES6 和 JSX。ES6 是一整套现代的 JavaScript 特性，它可以简化开发过程，JSX 是一个JavaScript 语言扩展，可以很好的配合 React 使用。

这份 [Babel 设置说明](https://babeljs.io/docs/setup/) 解释了如何在许多不同的构建环境中配置 Babel 。确保你安装了 [`babel-preset-react`](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-) 和 [`babel-preset-es2015`](http://babeljs.io/docs/plugins/preset-es2015/#basic-setup-with-the-cli-) ，并且在 [`.babelrc` 配置](http://babeljs.io/docs/usage/babelrc/) 中启用了它们，这样就准备就绪了。

### 使用 ES6 和 JSX 的 Hello World

我们推荐使用类似 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 这样的打包工具，以便您可以编写模块化代码，并将其它打包在一起成一个小包，以优化加载时间。

最简化的 React 示例如下：

```js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

这段代码会渲染到id为 `root` 的 DOM 节点，所以在你的 HTML 文件的某个地方需要 `<div id="root"></div>` 。

同样的，你可以在使用其它 Javascript UI 库所写的现有应用中渲染一个 React 组件到 DOM 节点。

[学习更多关于将 React 整合到现有代码](/cn/docs/integrating-with-other-libraries.md#integrating-with-other-view-libraries)

### 开发与生产版本

默认情况下，React 包含许多有用的警告信息。这些警告信息在开发环境中非常有用。

**然而，它会使得开发版本的 React 又大又慢，所以你应当在部署生产环境应用时使用生产版本**

学习 [如何判断你的网站服务是正确的 React 版本](/cn/docs/optimizing-performance.md#use-the-production-build)，以及如何更有效的配置生产环境构建进程：

* [通过 Create React App 创建一个生产环境构建](/cn/docs/optimizing-performance.md#create-react-app)
* [通过 Single-File Builds 创建一个生产环境构建](/cn/docs/optimizing-performance.md#single-file-builds)
* [通过 Brunch 创建一个生产环境构建](/cn/docs/optimizing-performance.md#brunch)
* [通过 Browserify 创建一个生产环境构建](/cn/docs/optimizing-performance.md#browserify)
* [通过 Rollup 创建一个生产环境构建](/cn/docs/optimizing-performance.md#rollup)
* [通过 Webpack 创建一个生产环境构建](/cn/docs/optimizing-performance.md#webpack)

### 使用 CDN

如果你不想使用 npm 来管理软件包， `react` 和 `react-dom` npm包在 `dist` 目录提供了独立版本，同时托管在以下 CDN:

```html
<script src="https://unpkg.com/react@15/dist/react.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.js"></script>
```

上面的版本只适合于开发环境，不适合生产环境，压缩并优化过的 React 版本如下：

```html
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```

要加载不同版本的 `react` 和 `react-dom` ，只要把以上代码中的 `15` 替换为相应的版本号即可。

如果你使用 Bower，React 也可以通过 `react` 包获取。



---

* 下一篇：[Hello World](/cn/docs/hello-world.md)

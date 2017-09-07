[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

---
<details>
  <summary>导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
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

* [React API](/cn/docs/react-api.md)
* [React.Component](/cn/docs/react-component.md)
* [ReactDOM](/cn/docs/react-dom.md)
* [ReactDOMServer](/cn/docs/react-dom-server.md)
* [DOM 元素](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)


#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [**`代码库概述`**](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)

</details>

# 代码库概述

这一节将为你带来 React 代码库的组织结构，约定，和具体实现。

如果你要 [为 React 做贡献](/cn/contributing/how-to-contribute.md) ，我们希望这个指南将帮助你更加方便的进行更改。

我们不一定会在 React 应用中推荐任何这些惯例。 其中许多是由于历史原因而存在的，可能随时间而变化。

### 定制模块系统

在 Facebook 内部，我们使用一个名为 "Haste" 的定制模块系统。它类似于 [CommonJS](https://nodejs.org/docs/latest/api/modules.html) ，也使用 `require()`，但有一些重要的区别，这经常让外部贡献者很容易混淆。

在 CommonJS 里，当你导入一个模块，你需要指定它的相对路径:

```js
// 从相同的目录导入:
var setInnerHTML = require('./setInnerHTML');

// 从不同的目录导入:
var setInnerHTML = require('../utils/setInnerHTML');

// 从深层嵌套目录导入:
var setInnerHTML = require('../client/utils/setInnerHTML');
```

然而，在 Haste 里，React 代码库里 **所有文件名都是全局唯一的**，你能一直通过它的名字从其它模块导入任何模块:

```js
var setInnerHTML = require('setInnerHTML');
```
Haste 最初是为了像 Facebook 这样的巨型应用研发的。它很容易移动文件到不同的目录，并在导入它们时不用考虑相对路径。得益于全局唯一的名字，在编辑中模糊查找文件会让你找到正确的位置。

React 本身是从 Facebook 的代码库里提取出来的，由于历史原因使用了 Haste。在将来，我们或许将 [移植 React 到使用 CommonJS 或 ES Modules](https://github.com/facebook/react/issues/6336) 以跟社区一致。然而，这需要在 Facebook 内部的基础设施里改变，因此它不太可能马上实现。

**如果你还记得一些规则 Haste 将为你带来更多功能:**

* 在 React 源码里所有文件名都是唯一的。这是为什么有时候它显得很冗长。
* 当你添加了一个新文件，请确保你包含了一个 [许可标题](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/client/utils/setInnerHTML.js#L1-L10). 你可以从任何现有文件里复制它。一个许可标题经常包含 [像这样的一行](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/client/utils/setInnerHTML.js#L9). 修改它以匹配你创建的文件名。
* 当导入的时候不要使用相对路径。这么写 `require('setInnerHTML')`，而不是  `require('./setInnerHTML')`。

当你为 npm 编译 React时，一个脚本拷贝所有模块到 [一个名为 `lib` 的单个目录](https://unpkg.com/react@15/lib/) ，并通过 `./` 预先考虑所有的 `require()` 路径。 这样的话，在没有 Haste 的时候，Browserify, Webpack, 和其它工具能理解 React 的构建输出。

**如果你在 GitHub 上阅读 React 源码并需要跳转到一个文件时，请按"t"**

这是 GitHub 快捷键，在当前仓库（repo）中模糊文件名搜索匹配。开始键入你要查找的文件的名称，它将作为第一次匹配显示。

### 外部依赖

React 几乎没有外部依赖。通过，一个 `require()` 指向 React 本身代码库的一个文件。然而，有一些比较少见的例外。

如果你发现一个在 React 仓库里没有对应文件的 `require()`，你可以在一个叫 [fbjs](https://github.com/facebook/fbjs) 的特定仓库查看。例如，`require('warning')` 将指向 [fbjs 中的 `warning` 模块](https://github.com/facebook/fbjs/blob/df9047fec0bbd1e64635ae369c045975777cba7c/packages/fbjs/src/__forks__/warning.js)。

[fbjs 仓库](https://github.com/facebook/fbjs) 的存在是因为 React 会跟像 [Relay](https://github.com/facebook/relay) 这样的库共享一些小程序，我们会保持它们的同步。我们不依赖 NODE 生态体系功能相同的小模块，是因为我们希望 Facebook 的工程师在必要的时候能够对它们进行修改，fbjs 中的任何公用程序都不被认为是公共 API，它们只是用来被类似 React 这样的 Facebook 的项目使用。

### 顶级文件夹

clone 好 [React 仓库](https://github.com/facebook/react) 之后，你将在里面看到一些顶层文件夹：

* [`src`](https://github.com/facebook/react/tree/master/src) 是 React 的源代码。 **如果你改变了相关的代码， `src` 将花费你相当多时间**
* [`docs`](https://github.com/facebook/react/tree/master/docs) 是 React 站点文档。当你修改了 API，请确保更新相关的 Markdown 文件.
* [`fixtures`](https://github.com/facebook/react/tree/master/fixtures) 包含了一些贡献者的 React 小测试应用
* [`packages`](https://github.com/facebook/react/tree/master/packages) 包含了在 React 仓库里所有包的无数据（类似 `package.json`）。然而，它们的源代码还是位于 [`src`](https://github.com/facebook/react/tree/master/src) 里面。
* `build` 是 React 的构建输出。它不是在仓库里，但第一次 [构建](/cn/contributing/how-to-contribute.md#development-workflow) 之后它将出现在你的 React clone 目录里

有一些其它的顶层文件夹，但它们是更多是一些工具使用的，当你做贡献时你可能永远不会需要它们。

### 共同测试

我们没有为单元测试准备一个顶层目录，我们把它们放到相对测试的文件的 `__tests__` 目录

例如， [`setInnerHTML.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/client/utils/setInnerHTML.js) 的测试位于 [`__tests__/setInnerHTML-test.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/client/utils/__tests__/setInnerHTML-test.js) 

### 分享代码

虽然 Haste 允许我们从仓库的任何位置导入任何模块，我们遵循一个约定，以避免循环依赖和其他不愉快的惊喜。 按照惯例，一个文件只可能导入相同目录或下面子目录的文件。

例如， [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client)  里面的文件可能导入相同目录或下面其它目录的其它文件

然而它们不能从 [`src/renderers/dom/stack/server`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/server) 导入模块，因为它不是 [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client) 的子目录

这个规则也有一个例外。有时候我们需要在两个组和模块之间分享函数。在这种情况下，我们把要分享的模块提升到最靠近共同祖先的模块目录中名为 `shared` 的目录。

例如，在 [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client) 和 [`src/renderers/dom/stack/server`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/server) 之间分享的代码放在 [`src/renderers/dom/shared`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/shared).

同样的逻辑，如果 [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client) 需要和 [`src/renderers/native`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/native) 分享一个小程序，这个小程序将放到 [`src/renderers/shared`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/shared).

这个约定不是强制的，但是我们会在 pull 请求审查中检查这个。

### 警告和不变量

React 代码库使用 `warning` 模块来显示警告:

```js
var warning = require('warning');

warning(
  2 + 2 === 4,
  'Math is not working today.'
);
```

**当 `warning` 条件为 `false` 时会显示警告。**

考虑一种方法，这个条件应该反映正常的情况而不是异常情况。

这是一个好主意，以避免重复警告的垃圾信息出现在控制台：

```js
var warning = require('warning');

var didWarnAboutMath = false;
if (!didWarnAboutMath) {
  warning(
    2 + 2 === 4,
    'Math is not working today.'
  );
  didWarnAboutMath = true;
}
```

警告只在开发环境中启用，在生产环境，它们是完全被剔除了。如果你需要阻止一些代码执行，请使用 `invariant` 模块代替:

```js
var invariant = require('invariant');

invariant(
  2 + 2 === 4,
  'You shall not pass!'
);
```

**当 `invariant` 条件为 `false` 时 invariant 会被抛出**

"Invariant" 只是一种表示 "this condition always holds true" 的方式。你可以把它当作一个断言。

保持开发环境和生产环境行为相似是很重要的，因此 `invariant` 在开发环境和生产环境中都抛出。错误消息将自动替换为生产中的错误代码，以避免负面影响字节大小。

### 开发环境与生产环境

你可以在代码库中使用伪全局变量 `__DEV__` 只为开发环境添加代码块。

在编辑阶段它是内联的，在 CommonJS 中构建时它转换为通过 `process.env.NODE_ENV !== 'production'` 来检查

对于独立构建，在没有更改的构建中它将变为 `true` ， 并且在压缩构建中 `if` 块会完全删除。

```js
if (__DEV__) {
  // 这段代码将只运行在开发环境中
}
```

### JSDoc

一些 internal 和公共方法通过 [JSDoc 标注](http://usejsdoc.org/) 来写注释:

```js
/**
  * Updates this component by updating the text content.
  *
  * @param {ReactText} nextText The next text content
  * @param {ReactReconcileTransaction} transaction
  * @internal
  */
receiveComponent: function(nextText, transaction) {
  // ...
},
```

我们试图保持现有的注释随时更新，但我们不会强制执行。在最近写代码的时候我们不使用 JSDoc ，而是使用Flow来记录和强制类型。

### Flow

我们最近开始引入 [Flow](https://flowtype.org/) 来检查代码库。文件通过 `@flow` 注释来标注在 license 头评论里做类型检查

我们接受 pull 请求 [添加 Flow 注释到当前的代码](https://github.com/facebook/react/pull/7600/files)。 Flow 注释看起来像这样:

```js
ReactRef.detachRefs = function(
  instance: ReactInstance,
  element: ReactElement | string | number | null | false,
): void {
  // ...
}
```

如果可能的话，新的代码应该使用 Flow 注释
你可以运行 `npm run flow` 通过 Flow 局部检查你的代码

### Classes 和 Mixins

React 最初是用 ES5 编写的。我们已经通过 [Babel](http://babeljs.io/) 启用 ES6 功能，包含 classes。然而 React 的大多数代码还是通过 ES5 编写

特别是你经常会看到下面的模式:

```js
// Constructor
function ReactDOMComponent(element) {
  this._currentElement = element;
}

// Methods
ReactDOMComponent.Mixin = {
  mountComponent: function() {
    // ...
  }
};

// Put methods on the prototype
Object.assign(
  ReactDOMComponent.prototype,
  ReactDOMComponent.Mixin
);

module.exports = ReactDOMComponent;
```

这段代码中的 `Mixin` 没有涉及 React `mixins` 功能。它只是一个对象下组织一些方法的方式。那些可能稍后会附加到一些其它的类。我们在一些地方使用这个模式，尽管我们试图在新代码中避免这么做。

在 ES6 中等价的代码看起来像这样:

```js
class ReactDOMComponent {
  constructor(element) {
    this._currentElement = element;
  }

  mountComponent() {
    // ...
  }
}

module.exports = ReactDOMComponent;
```

有时我们 [转换老代码到 ES6 classes](https://github.com/facebook/react/pull/7647/files)。然而，对我们来说这不是非常重要，因为有一个 [持续的努力](#fiber-reconciler) 以更少的面向对象的方法来替代React调解器实现，这种方法根本不使用类。

### 动态注入

React 在一些模块中使用动态注入。虽然它总是明确的，但它还是令人遗憾，因为它阻碍了代码的理解。它存在的主要原因是因为 React 最初只支持 DOM 作为目标。 React Native 开始的时候作为 React fork。我们不得不添加动态注入来让 React Native 覆写一些行为。

你可能会看到模块说明它们的动态依赖像这样:

```js
// Dynamically injected
var textComponentClass = null;

// Relies on dynamically injected value
function createInstanceForText(text) {
  return new textComponentClass(text);
}

var ReactHostComponent = {
  createInstanceForText,

  // Provides an opportunity for dynamic injection
  injection: {
    injectTextComponentClass: function(componentClass) {
      textComponentClass = componentClass;
    },
  },
};

module.exports = ReactHostComponent;
```

`injection` 域不以任何方式特别处理。但是，按照约定，它意味着这个模块要有一些 (大概是平台特定的) 依赖注入到运行时

在 React DOM, [`ReactDefaultInjection`](https://github.com/facebook/react/blob/4f345e021a6bd9105f09f3aee6d8762eaa9db3ec/src/renderers/dom/shared/ReactDefaultInjection.js) 注入一个特殊的 DOM 实现：

```js
ReactHostComponent.injection.injectTextComponentClass(ReactDOMTextComponent);
```

在 React Native, [`ReactNativeDefaultInjection`](https://github.com/facebook/react/blob/4f345e021a6bd9105f09f3aee6d8762eaa9db3ec/src/renderers/native/ReactNativeDefaultInjection.js) 注入它自己的实现:

```js
ReactHostComponent.injection.injectTextComponentClass(ReactNativeTextComponent);
```

在代码库中有多个注入点。在将来，我们打算丢弃动态注入机制，并在构建过程中静默连接所有部件

### 多个包

React 是一个 [monorepo](http://danluu.com/monorepo/)。它的仓库饮食多个单独的包，因此它们改变可以一起调整，文档和问题在一个地方存放。

像 `package.json` 文件这样的 NPM 无数据位于 [`packages`](https://github.com/facebook/react/tree/master/packages) 顶层文件夹里。然而在里面几乎没有真正的代码。

例如， [`packages/react/react.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/packages/react/react.js) 重新导出 [`src/isomorphic/React.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/isomorphic/React.js), 这是 NPM 真正的入口。其它包差不多重复这个模式。所有重要的代码都放在 [`src`](https://github.com/facebook/react/tree/master/src).

当代码从源码中分离出来，npm 包和独立的浏览器构建提取出来的包的边界是稍有不同的。

### React Core

React 的 "核心（core）" 包含所有的 [顶层 `React` API](/cn/docs/top-level-api.md#react)，例如:

* `React.createElement()`
* `React.Component`
* `React.Children`

**React 核心（core）只包含定义组件所需要的 API.** 它不包含 [reconciliation](/cn/docs/reconciliation.md) 算法或其它特定平台的代码，它被 React DOM 和 React Native 组件使用.

React 核心代码位于源码树的 [`src/isomorphic`](https://github.com/facebook/react/tree/master/src/isomorphic) ，它可以在 npm 上作为 [`react`](https://www.npmjs.com/package/react) 包获取。独立的浏览器构建名为 `react.js`，它导出一个名为 `React` 的全局变量。

>**注意:**
>
>直到最近， `react` npm 包和 `react.js` 独立构建都包含所有的 React 代码 (包括 React DOM) 而不仅仅是核心。这么做是为了向后兼容和历史原因。直到React 15.4.0，核心才更好的分离在构建输出里。
>
>也有一些额外的名为 `react-with-addons.js` 的浏览器构建，我们将在未来考虑分离。

### 渲染器（Renderers）

React 最初是为 DOM 创造的，但后来也适用于通过 [React Native](http://facebook.github.io/react-native/) 支持原生平台。这引入了 "renderers" 的概念到 React 内部。

**Renderers 管理 React 树如何转换到底层平台调用**

Renderers 位于 [`src/renderers`](https://github.com/facebook/react/tree/master/src/renderers/):

* [React DOM Renderer](https://github.com/facebook/react/tree/master/src/renderers/dom) 渲染 React 组件到 DOM。它作为 [顶层 `ReactDOM` API](/cn/docs/top-level-api.md#reactdom) 并且可通过 [`react-dom`](https://www.npmjs.com/package/react-dom) npm 包获取。也能作为名为  `react-dom.js`  的独立浏览器构建使用，它导出一个 `ReactDOM` 全局变量。
* [React Native Renderer](https://github.com/facebook/react/tree/master/src/renderers/native) 渲染 React 组件到 native 视图. 它由 React Native 内部通过 [`react-native-renderer`](https://www.npmjs.com/package/react-native-renderer) npm 包来使用。在将来它的一份拷贝可能进入到 React Native [仓库](https://github.com/facebook/react-native) ，因此 React Native 能在它自己的节奏里更新 React 。
* [React Test Renderer](https://github.com/facebook/react/tree/master/src/renderers/testing) 渲染 React 组件到 JSON 树。它通过 [Jest](https://facebook.github.io/jest) 的 [快照测试](https://facebook.github.io/jest/blog/2016/07/27/jest-14.html) 功能作为 [react-test-renderer](https://www.npmjs.com/package/react-test-renderer) npm 包在内部使用。

其它唯一官方支持的渲染器是 [`react-art`](https://github.com/reactjs/react-art)。为了避免在我们对React进行更改时意外破坏它，我们在 [`src/renderers/art`](https://github.com/facebook/react/tree/master/src/renderers/art) 里做检查并运行它的测试套件。 然而，它的 [GitHub 仓库](https://github.com/reactjs/react-art) 仍然是源码的来源。

一旦创建定制的 React renderers 在[技术上可行](https://github.com/iamdustan/tiny-react-renderer) ，当前的方案将不再被官方支持。对于定制渲染器没有稳定的公共约定，这是我们将它们全部放在一个地方的另一个原因。

>**注意:**
>
>从技术上来说 [`native`](https://github.com/facebook/react/tree/master/src/renderers/native) renderer 是非常薄的一层，指导 React 和 React Native 实现相互影响。真正的特定平台管理 native 视图代码放在 [React Native 仓库](https://github.com/facebook/react-native) ，和它的组件放在一起。

### 调解器（Reconcilers）

即使是像 React DOM 和 React Native 这样非常不一般的渲染器，也需要共享很多逻辑。特别是 [reconciliation](/cn/docs/reconciliation.md) 算法应该尽可能类似以便声明式渲染，定制组件，状态，生命周期方法，和 refs 在平台之间保持一致。

为了解决这个问题，不同的渲染器在它们之间共享一些代码。我们把 React 这部分称为 "reconciler"。当类似 `setState()` 这样的更新已经被安排好时，reconciler 在组件树上调用 `render()` ，并且挂载、更新、卸载它们。

Reconcilers 没有单独打包，因为它们当前有一些共用的 API，相反，它们仅仅通过类似 React DOM 和 React Native 这样的渲染器使用。

### 堆栈调解器（Stack Reconciler）

"stack" reconciler 是今天提供的所有 React 生产代码中的一个。它位于 [`src/renderers/shared/stack/reconciler`](https://github.com/facebook/react/tree/master/src/renderers/shared/stack) ，并且它在 React DOM 和 React Native 中都有使用。

它是通过 [面向对象的方式](https://en.wikipedia.org/wiki/Composite_pattern) 来写的，并为所有 React 组件维护一份独立的 "内部实例" 树。内部实例存在于用户定义 ("composite") 和平台指定 ("host") 的组件。 内部实例对用户来说是不能直接看到的，并且它们的树没有暴露。

当一个组件挂载、更新、卸载时，它们的 stack reconciler 在内部实例上调用一个方法。这个方法名为 `mountComponent(element)`, `receiveComponent(nextElement)`, 和 `unmountComponent(element)`.

#### 宿主组件（Host Components）

Platform-specific ("host") components, such as `<div>` or a `<View>`, run platform-specific code. For example, React DOM instructs the stack reconciler to use [`ReactDOMComponent`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js) to handle [mounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L517), [updates](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L865), and [unmounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L1140) of DOM components.

Regardless of the platform, both `<div>` and `<View>` handle managing multiple children in a similar way. For convenience, the stack reconciler provides a helper called [`ReactMultiChild`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactMultiChild.js) that both DOM and Native renderers [use](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L1203).

#### 复合组件（Composite Components）

User-defined ("composite") components should behave the same way with all renderers. This is why the stack reconciler provides a shared implementation in [`ReactCompositeComponent`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js). It is always the same regardless of the renderer.

Composite components also implement [mounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L181), [updating](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L703), and [unmounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L524). However, unlike host components, `ReactCompositeComponent` needs to behave differently depending on the user's code. This is why it calls methods, such as `render()` and `componentDidMount()`, on the user-supplied class.

During an update, `ReactCompositeComponent` checks whether the `render()` output has a different `type` or `key` than the last time. If neither `type` nor `key` has changed, it delegates the update to the existing child internal instance. Otherwise, it unmounts the old child instance and mounts a new one. This is described in the [reconciliation algorithm](/cn/docs/reconciliation.md).

#### 递归（Recursion）

During an update, the stack reconciler "drills down" through composite components, runs their `render()` methods, and decides whether to update or replace their single child instance. It executes platform-specific code as it passes through the host components like `<div>` and `<View>`. Host components may have multiple children which are also processed recursively.

It is important to understand that the stack reconciler always processes the component tree synchronously in a single pass. While individual tree branches may [bail out of reconciliation](/cn/docs/advanced-performance.md#avoiding-reconciling-the-dom), the stack reconciler can't pause, and so it is suboptimal when the updates are deep and the available CPU time is limited.

#### 了解更多

**[下一节](/cn/contributing/implementation-notes.md) 描述了当前实现的更多详情**

### Fiber Reconciler

The "fiber" reconciler is a new effort aiming to resolve the problems inherent in the stack reconciler and fix a few long-standing issues.

It is a complete rewrite of the reconciler and is currently [in active development](https://github.com/facebook/react/pulls?utf8=%E2%9C%93&q=is%3Apr%20is%3Aopen%20fiber).

Its main goals are:

* Ability to split interruptible work in chunks.
* Ability to prioritize, rebase and reuse work in progress.
* Ability to yield back and forth between parents and children to support layout in React.
* Ability to return multiple elements from `render()`.
* Better support for error boundaries.

You can read more about it in [React Fiber Architecture](https://github.com/acdlite/react-fiber-architecture). At this moment, it is still very experimental, and far from feature parity with the stack reconciler.

Its source code is located in [`src/renderers/shared/fiber`](https://github.com/facebook/react/tree/master/src/renderers/shared/fiber).

### 事件系统

React 实现了一个合成事件系统，它在 React DOM 和 React Native 中都可以运行。它的源码位于 [`src/renderers/shared/shared/event`](https://github.com/facebook/react/tree/master/src/renderers/shared/shared/event).

这里有一个 [深入代码内部的视频](https://www.youtube.com/watch?v=dRo_egw7tBc) (66 分钟).

### 下一节有什么?

阅读 [下一节](/cn/contributing/implementation-notes.md) 学习关于调解器（Reconcilers）当前实现的更多详情。



---

* 上一篇：[如何贡献](/cn/contributing/how-to-contribute.md)
* 下一篇：[实现说明](/cn/contributing/implementation-notes.md)

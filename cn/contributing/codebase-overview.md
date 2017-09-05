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

For example, code shared between [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client) and [`src/renderers/dom/stack/server`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/server) lives in [`src/renderers/dom/shared`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/shared).

By the same logic, if [`src/renderers/dom/stack/client`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/dom/stack/client) needs to share a utility with something in [`src/renderers/native`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/native), this utility would be in [`src/renderers/shared`](https://github.com/facebook/react/blob/f53854424b33692907234fe7a1f80b888fd80751/src/renderers/shared).

This convention is not enforced but we check for it during a pull request review.

### 警告和不变量

The React codebase uses the `warning` module to display warnings:

```js
var warning = require('warning');

warning(
  2 + 2 === 4,
  'Math is not working today.'
);
```

**The warning is shown when the `warning` condition is `false`.**

One way to think about it is that the condition should reflect the normal situation rather than the exceptional one.

It is a good idea to avoid spamming the console with duplicate warnings:

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

Warnings are only enabled in development. In production, they are completely stripped out. If you need to forbid some code path from executing, use `invariant` module instead:

```js
var invariant = require('invariant');

invariant(
  2 + 2 === 4,
  'You shall not pass!'
);
```

**The invariant is thrown when the `invariant` condition is `false`.**

"Invariant" is just a way of saying "this condition always holds true". You can think about it as making an assertion.

It is important to keep development and production behavior similar, so `invariant` throws both in development and in production. The error messages are automatically replaced with error codes in production to avoid negatively affecting the byte size.

### 开发环境与生产环境

You can use `__DEV__` pseudo-global variable in the codebase to guard development-only blocks of code.

It is inlined during the compile step, and turns into `process.env.NODE_ENV !== 'production'` checks in the CommonJS builds.

For standalone builds, it becomes `true` in the unminified build, and gets completely stripped out with the `if` blocks it guards in the minified build.

```js
if (__DEV__) {
  // This code will only run in development.
}
```

### JSDoc

Some of the internal and public methods are annotated with [JSDoc annotations](http://usejsdoc.org/):

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

We try to keep existing annotations up-to-date but we don't enforce them. We don't use JSDoc in the newly written code, and instead use Flow to document and enforce types.

### Flow

We recently started introducing [Flow](https://flowtype.org/) checks to the codebase. Files marked with the `@flow` annotation in the license header comment are being typechecked.

We accept pull requests [adding Flow annotations to existing code](https://github.com/facebook/react/pull/7600/files). Flow annotations look like this:

```js
ReactRef.detachRefs = function(
  instance: ReactInstance,
  element: ReactElement | string | number | null | false,
): void {
  // ...
}
```

When possible, new code should use Flow annotations.  
You can run `npm run flow` locally to check your code with Flow.

### Classes 和 Mixins

React was originally written in ES5. We have since enabled ES6 features with [Babel](http://babeljs.io/), including classes. However, most of React code is still written in ES5.

In particular, you might see the following pattern quite often:

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

The `Mixin` in this code has no relation to React `mixins` feature. It is just a way of grouping a few methods under an object. Those methods may later get attached to some other class. We use this pattern in a few places although we try to avoid it in the new code.

The equivalent code in ES6 would look like this:

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

Sometimes we [convert old code to ES6 classes](https://github.com/facebook/react/pull/7647/files). However, this is not very important to us because there is an [ongoing effort](#fiber-reconciler) to replace the React reconciler implementation with a less object-oriented approach which wouldn't use classes at all.

### 动态注入

React uses dynamic injection in some modules. While it is always explicit, it is still unfortunate because it hinders understanding of the code. The main reason it exists is because React originally only supported DOM as a target. React Native started as a React fork. We had to add dynamic injection to let React Native override some behaviors.

You may see modules declaring their dynamic dependencies like this:

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

The `injection` field is not handled specially in any way. But by convention, it means that this module wants to have some (presumably platform-specific) dependencies injected into it at runtime.

In React DOM, [`ReactDefaultInjection`](https://github.com/facebook/react/blob/4f345e021a6bd9105f09f3aee6d8762eaa9db3ec/src/renderers/dom/shared/ReactDefaultInjection.js) injects a DOM-specific implementation:

```js
ReactHostComponent.injection.injectTextComponentClass(ReactDOMTextComponent);
```

In React Native, [`ReactNativeDefaultInjection`](https://github.com/facebook/react/blob/4f345e021a6bd9105f09f3aee6d8762eaa9db3ec/src/renderers/native/ReactNativeDefaultInjection.js) injects its own implementation:

```js
ReactHostComponent.injection.injectTextComponentClass(ReactNativeTextComponent);
```

There are multiple injection points in the codebase. In the future, we intend to get rid of the dynamic injection mechanism and wire up all the pieces statically during the build.

### 多个包

React is a [monorepo](http://danluu.com/monorepo/). Its repository contains multiple separate packages so that their changes can be coordinated together, and documentation and issues live in one place.

The npm metadata such as `package.json` files is located in the [`packages`](https://github.com/facebook/react/tree/master/packages) top-level folder. However, there is almost no real code in it.

For example, [`packages/react/react.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/packages/react/react.js) re-exports [`src/isomorphic/React.js`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/isomorphic/React.js), the real npm entry point. Other packages mostly repeat this pattern. All the important code lives in [`src`](https://github.com/facebook/react/tree/master/src).

While the code is separated in the source tree, the exact package boundaries are slightly different for npm packages and standalone browser builds.

### React Core

The "core" of React includes all the [top-level `React` APIs](/react/docs/top-level-api.html#react), for example:

* `React.createElement()`
* `React.Component`
* `React.Children`

**React core only includes the APIs necessary to define components.** It does not include the [reconciliation](/react/docs/reconciliation.html) algorithm or any platform-specific code. It is used both by React DOM and React Native components.

The code for React core is located in [`src/isomorphic`](https://github.com/facebook/react/tree/master/src/isomorphic) in the source tree. It is available on npm as the [`react`](https://www.npmjs.com/package/react) package. The corresponding standalone browser build is called `react.js`, and it exports a global called `React`.

>**Note:**
>
>Until very recently, `react` npm package and `react.js` standalone build contained all React code (including React DOM) rather than just the core. This was done for backward compatibility and historical reasons. Since React 15.4.0, the core is better separated in the build output.
>
>There is also an additional standalone browser build called `react-with-addons.js` which we will consider separately further below.

### 渲染器（Renderers）

React was originally created for the DOM but it was later adapted to also support native platforms with [React Native](http://facebook.github.io/react-native/). This introduced the concept of "renderers" to React internals.

**Renderers manage how a React tree turns into the underlying platform calls.**

Renderers are located in [`src/renderers`](https://github.com/facebook/react/tree/master/src/renderers/):

* [React DOM Renderer](https://github.com/facebook/react/tree/master/src/renderers/dom) renders React components to the DOM. It implements [top-level `ReactDOM` APIs](/react/docs/top-level-api.html#reactdom) and is available as [`react-dom`](https://www.npmjs.com/package/react-dom) npm package. It can also be used as standalone browser bundle called `react-dom.js` that exports a `ReactDOM` global.
* [React Native Renderer](https://github.com/facebook/react/tree/master/src/renderers/native) renders React components to native views. It is used internally by React Native via [`react-native-renderer`](https://www.npmjs.com/package/react-native-renderer) npm package. In the future a copy of it may get checked into the React Native [repository](https://github.com/facebook/react-native) so that React Native can update React at its own pace.
* [React Test Renderer](https://github.com/facebook/react/tree/master/src/renderers/testing) renders React components to JSON trees. It is used by the [Snapshot Testing](https://facebook.github.io/jest/blog/2016/07/27/jest-14.html) feature of [Jest](https://facebook.github.io/jest) and is available as [react-test-renderer](https://www.npmjs.com/package/react-test-renderer) npm package.

The only other officially supported renderer is [`react-art`](https://github.com/reactjs/react-art). To avoid accidentally breaking it as we make changes to React, we checked it in as [`src/renderers/art`](https://github.com/facebook/react/tree/master/src/renderers/art) and run its test suite. Nevertheless, its [GitHub repository](https://github.com/reactjs/react-art) still acts as the source of truth.

While it is [technically possible](https://github.com/iamdustan/tiny-react-renderer) to create custom React renderers, this is currently not officially supported. There is no stable public contract for custom renderers yet which is another reason why we keep them all in a single place.

>**Note:**
>
>Technically the [`native`](https://github.com/facebook/react/tree/master/src/renderers/native) renderer is a very thin layer that teaches React to interact with React Native implementation. The real platform-specific code managing the native views lives in the [React Native repository](https://github.com/facebook/react-native) together with its components.

### 调解器（Reconcilers）

Even vastly different renderers like React DOM and React Native need to share a lot of logic. In particular, the [reconciliation](/react/docs/reconciliation.html) algorithm should be as similar as possible so that declarative rendering, custom components, state, lifecycle methods, and refs work consistently across platforms.

To solve this, different renderers share some code between them. We call this part of React a "reconciler". When an update such as `setState()` is scheduled, the reconciler calls `render()` on components in the tree and mounts, updates, or unmounts them.

Reconcilers are not packaged separately because they currently have no public API. Instead, they are exclusively used by renderers such as React DOM and React Native.

### 堆栈调解器（Stack Reconciler）

The "stack" reconciler is the one powering all React production code today. It is located in [`src/renderers/shared/stack/reconciler`](https://github.com/facebook/react/tree/master/src/renderers/shared/stack) and is used by both React DOM and React Native.

It is written in an [object-oriented way](https://en.wikipedia.org/wiki/Composite_pattern) and maintains a separate tree of "internal instances" for all React components. The internal instances exist both for user-defined ("composite") and platform-specific ("host") components. The internal instances are inaccessible directly to the user, and their tree is never exposed.

When a component mounts, updates, or unmounts, the stack reconciler calls a method on that internal instance. The methods are called `mountComponent(element)`, `receiveComponent(nextElement)`, and `unmountComponent(element)`.

#### 宿主组件（Host Components）

Platform-specific ("host") components, such as `<div>` or a `<View>`, run platform-specific code. For example, React DOM instructs the stack reconciler to use [`ReactDOMComponent`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js) to handle [mounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L517), [updates](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L865), and [unmounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L1140) of DOM components.

Regardless of the platform, both `<div>` and `<View>` handle managing multiple children in a similar way. For convenience, the stack reconciler provides a helper called [`ReactMultiChild`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactMultiChild.js) that both DOM and Native renderers [use](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/dom/shared/ReactDOMComponent.js#L1203).

#### 复合组件（Composite Components）

User-defined ("composite") components should behave the same way with all renderers. This is why the stack reconciler provides a shared implementation in [`ReactCompositeComponent`](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js). It is always the same regardless of the renderer.

Composite components also implement [mounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L181), [updating](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L703), and [unmounting](https://github.com/facebook/react/blob/87724bd87506325fcaf2648c70fc1f43411a87be/src/renderers/shared/stack/reconciler/ReactCompositeComponent.js#L524). However, unlike host components, `ReactCompositeComponent` needs to behave differently depending on the user's code. This is why it calls methods, such as `render()` and `componentDidMount()`, on the user-supplied class.

During an update, `ReactCompositeComponent` checks whether the `render()` output has a different `type` or `key` than the last time. If neither `type` nor `key` has changed, it delegates the update to the existing child internal instance. Otherwise, it unmounts the old child instance and mounts a new one. This is described in the [reconciliation algorithm](/react/docs/reconciliation.html).

#### 递归（Recursion）

During an update, the stack reconciler "drills down" through composite components, runs their `render()` methods, and decides whether to update or replace their single child instance. It executes platform-specific code as it passes through the host components like `<div>` and `<View>`. Host components may have multiple children which are also processed recursively.

It is important to understand that the stack reconciler always processes the component tree synchronously in a single pass. While individual tree branches may [bail out of reconciliation](/react/docs/advanced-performance.html#avoiding-reconciling-the-dom), the stack reconciler can't pause, and so it is suboptimal when the updates are deep and the available CPU time is limited.

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

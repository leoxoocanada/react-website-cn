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
* [**`ReactDOMServer`**](/cn/docs/react-dom-server.md)
* [DOM 元素](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)

#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

# ReactDOMServer

如果从 `<script>` 标签加载 React，则这些顶层 API 可以在全局 `ReactDOMServer` 中使用。 如果您使用带有npm的ES6，可以这么写 `import ReactDOMServer from 'react-dom/server'`。 如果你用npm使用ES5，你可以这么写 `var ReactDOMServer = require('react-dom/server')`。

## 概述

`ReactDOMServer` 类允许你在服务器上渲染组件。

 - [`renderToString()`](#rendertostring)
 - [`renderToStaticMarkup()`](#rendertostaticmarkup)

* * *

## 参考

### `renderToString()`

```javascript
ReactDOMServer.renderToString(element)
```

将 React 元素渲染到其初始 HTML。 这只能在服务器上使用。 React将返回一个HTML字符串。 您可以使用此方法在服务器上生成 HTML，并在初始请求上发送标记以加快页面加载速度，并允许搜索引擎抓取您的页面以进行 SEO 优化。

如果在已经具有此服务器渲染标记的节点上调用 [`ReactDOM.render()`](/cn/docs/react-dom.md#render) ，则React将保留它，并仅附加事件处理程序，从而允许 具有非常优秀的首次体验。

* * *

### `renderToStaticMarkup()`

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

类似于 [`renderToString`](#rendertostring)，除了它不会创建额外的DOM属性，如 `data-reactid`，React在内部使用。 如果要将 React 用作简单的静态页面生成器，这很有用，因为剥离掉额外的属性可以节省大量字节。


---

* 上一篇：[ReactDOM](/cn/docs/react-dom.md)
* 下一篇：[DOM 元素](/cn/docs/dom-elements.md)

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
* [**`ReactDOM`**](/cn/docs/react-dom.md)
* [ReactDOMServer](/cn/docs/react-dom-server.md)
* [DOM 元素](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)

#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

# ReactDOM

如果从 `<script>` 标签加载 React，则这些顶级API可以在全局 `ReactDOM` 中使用。 如果您使用带有 npm 的 ES6，可以这么写 `import ReactDOM from 'react-dom'`。 如果你用 npm 使用 ES5，你可以这么写 `var ReactDOM = require('react-dom')`。

## 预览

`react-dom` 包提供了DOM特定的方法，可以在应用程序的顶层使用，并且如果需要，可以将其作为一个在React模型之外特殊操作DOM的接口。 大多数组件不需要使用此模块。

- [`render()`](#render)
- [`unmountComponentAtNode()`](#unmountcomponentatnode)
- [`findDOMNode()`](#finddomnode)

### 浏览器支持

React支持所有主流的浏览器，包括 Internet Explorer 9 及更高版本。

> 提示
>
> 我们不支持那些不支持ES5方法的旧版浏览器，但如果在页面中使用了诸如 [es5-shim和es5-sham](https://github.com/es-shims/es5-shim) 这样的脚本，您可能会发现您的应用程序在旧版浏览器中可以正常工作。如果你愿意你也可以选择走这条路。

* * *

## 参考

### `render()`

```javascript
ReactDOM.render(
  element,
  container,
  [callback]
)
```

将一个 React 元素渲染到提供的的DOM `container` 中，并返回组件的一个 [引用(reference)](/cn/docs/more-about-refs.md) （或返回 `null` 作为[无状态组件](/cn/docs/components-and-props.md#functional-and-class-components))。

如果将 React 元素预先渲染到 `container`，这样就可以对它进行更新，根据需要改变 DOM，以反映最新的 React 元素。

如果提供了可选的回调函数，它将在组件渲染完成或更新完成后执行。

> 提示:
>
> `ReactDOM.render()` 控制您传入的容器节点的内容。任何里面的现有 DOM 元素都将在第一次调用时被替换。 后续的调用使用 React 的 DOM diffing 算法进行有效的更新。
>
> `ReactDOM.render()` 不修改容器节点（只修改容器的子节点）。 可以将组件插入到现有的 DOM 节点，而不会覆盖现有的子节点。 
>
> `ReactDOM.render()` 返回对根 `ReactComponent` 实例的引用。然而，使用这个返回值是个历史遗留问题，应该避免这么做，因为未来的 React 版本可能会在一些情况下异步渲染组件。如果你需要引用到根 `ReactComponent` 实例，首选的方案是附加一个 [ref 回调](/cn/docs/more-about-refs.md#the-ref-callback-attribute) 到根节点。

* * *

### `unmountComponentAtNode()`

```javascript
ReactDOM.unmountComponentAtNode(container)
```

从 DOM 中删除已挂载的 React 组件，并清除其事件处理程序和状态。 如果没有组件挂装在容器中，调用此函数什么也不做。 如果组件被卸载，则返回 `true` ，如果没有组件卸载，则返回 `false`。

* * *

### `findDOMNode()`

```javascript
ReactDOM.findDOMNode(component)
```
If this component has been mounted into the DOM, this returns the corresponding native browser DOM element. This method is useful for reading values out of the DOM, such as form field values and performing DOM measurements. **In most cases, you can attach a ref to the DOM node and avoid using `findDOMNode` at all.** When `render` returns `null` or `false`, `findDOMNode` returns `null`.

> Note:
>
> `findDOMNode` is an escape hatch used to access the underlying DOM node. In most cases, use of this escape hatch is discouraged because it pierces the component abstraction.
>
> `findDOMNode` only works on mounted components (that is, components that have been placed in the DOM). If you try to call this on a component that has not been mounted yet (like calling `findDOMNode()` in `render()` on a component that has yet to be created) an exception will be thrown.
>
> `findDOMNode` cannot be used on functional components.

---

* 上一篇：[React.Component](/cn/docs/react-component.md)
* 下一篇：[ReactDOMServer](/cn/docs/react-dom-server.md)

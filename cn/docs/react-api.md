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

* [**`React API`**](/cn/docs/react-api.md)
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

# React API

`React` 是 React 库的入口. 如果你是从 `<script>` 标签加载 React, 这些顶层的 API 可以在 `React` 全局变量中使用. 如果你通过 npm 使用 ES6, 可以这么写 `import React from 'react'`. 如果你通过 npm 使用 ES5 , 可以这么写 `var React = require('react')`.

## 概览

### 组件

React 组件让你将 UI 拆分到独立的、可复用的块，并分别考虑每个块。React 组件可以通过 `React.Component` 或 `React.PureComponent` 的子类来定义.

 - [`React.Component`](#react.component)
 - [`React.PureComponent`](#react.purecomponent)

如果你不使用 ES6 类(classes), 你可以使用 `create-react-class` 模块代替. 查看 [不使用 ES6 的 React](/cn/docs/react-without-es6.md)  了解更多信息.

### 创建 React 元素

我们建议 [使用 JSX](/cn/docs/introducing-jsx.md) 来描述你的 UI 看起来应该长什么样. 每个 JSX 元素只是调用 [`React.createElement()`](#createelement) 的语法糖. 如果使用 JSX ，你将不需要直接调用下面的方法.

- [`createElement()`](#createelement)
- [`createFactory()`](#createfactory)

查看 [不使用 JSX 的 React](/cn/docs/react-without-jsx.md) 了解更多信息.

### 转换元素

`React` 还提供了一些其他API：

- [`cloneElement()`](#cloneelement)
- [`isValidElement()`](#isvalidelement)
- [`React.Children`](#react.children)

* * *

## 参考

### `React.Component`

`React.Component` 是使用 [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 来定义 React 组件的基类.

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

查看 [React.Component API 参考](/cn/docs/react-component.md) 查看有关基于 `React.Component` 类的方法和属性列表.

* * *

### `React.PureComponent`

`React.PureComponent` 与 [`React.Component`](#react.component) 完全一样，但使用浅属性和状态来实现 [`shouldComponentUpdate()`](/cn/docs/react-component.md#shouldcomponentupdate) 比较.

如果你的 React 组件的 `render()` 方法在给定同样属性和状态下渲染同样的结果，你能使用 `React.PureComponent` 在某些情况下提升性能。

> 提示

> `React.PureComponent` 的 `shouldComponentUpdate()` 只会做对象浅比较。如果他们包含复杂的数据结构，它可能会产生漏报率，导致更深层次的偏差。 当你预期有简单的属性和状态时，只需要扩展 `PureComponent`，或者当你知道深层次数据结构已经改变时使用 [`forceUpdate()`](/cn/docs/react-component.md#forceupdate) 。或者考虑使用 [不可变对象](https://facebook.github.io/immutable-js/) 促进嵌套数据的快速比较。
>
> Furthermore, `React.PureComponent`'s `shouldComponentUpdate()` skips prop updates for the whole component subtree. Make sure all the children components are also "pure".
> 此外，`React.PureComponent` 的 `shouldComponentUpdate（）` 跳过整个组件子树的属性更新。 确保所有的子组件也是 "纯(pure)"。

* * *

### `createElement()`

```javascript
React.createElement(
  type,
  [props],
  [...children]
)
```

Create and return a new [React element](/react/docs/rendering-elements.html) of the given type. The type argument can be either a tag name string (such as `'div'` or `'span'`), or a [React component](/react/docs/components-and-props.html) type (a class or a function).

Convenience wrappers around `React.createElement()` for DOM components are provided by `React.DOM`. For example, `React.DOM.a(...)` is a convenience wrapper for `React.createElement('a', ...)`. They are considered legacy, and we encourage you to either use JSX or use `React.createElement()` directly instead.

Code written with [JSX](/react/docs/introducing-jsx.html) will be converted to use `React.createElement()`. You will not typically invoke `React.createElement()` directly if you are using JSX. See [React Without JSX](/react/docs/react-without-jsx.html) to learn more.

* * *

### `cloneElement()`

```
React.cloneElement(
  element,
  [props],
  [...children]
)
```

Clone and return a new React element using `element` as the starting point. The resulting element will have the original element's props with the new props merged in shallowly. New children will replace existing children. `key` and `ref` from the original element will be preserved.

`React.cloneElement()` is almost equivalent to:

```js
<element.type {...element.props} {...props}>{children}</element.type>
```

However, it also preserves `ref`s. This means that if you get a child with a `ref` on it, you won't accidentally steal it from your ancestor. You will get the same `ref` attached to your new element.

This API was introduced as a replacement of the deprecated `React.addons.cloneWithProps()`.

* * *

### `createFactory()`

```javascript
React.createFactory(type)
```

Return a function that produces React elements of a given type. Like [`React.createElement()`](#createElement), the type argument can be either a tag name string (such as `'div'` or `'span'`), or a [React component](/react/docs/components-and-props.html) type (a class or a function).

This helper is considered legacy, and we encourage you to either use JSX or use `React.createElement()` directly instead.

You will not typically invoke `React.createFactory()` directly if you are using JSX. See [React Without JSX](/react/docs/react-without-jsx.html) to learn more.

* * *

### `isValidElement()`

```javascript
React.isValidElement(object)
```

Verifies the object is a React element. Returns `true` or `false`.

* * *

### `React.Children`

`React.Children` provides utilities for dealing with the `this.props.children` opaque data structure.

#### `React.Children.map`

```javascript
React.Children.map(children, function[(thisArg)])
```

Invokes a function on every immediate child contained within `children` with `this` set to `thisArg`. If `children` is a keyed fragment or array it will be traversed: the function will never be passed the container objects. If children is `null` or `undefined`, returns `null` or `undefined` rather than an array.

#### `React.Children.forEach`

```javascript
React.Children.forEach(children, function[(thisArg)])
```

Like [`React.Children.map()`](#react.children.map) but does not return an array.

#### `React.Children.count`

```javascript
React.Children.count(children)
```

Returns the total number of components in `children`, equal to the number of times that a callback passed to `map` or `forEach` would be invoked.

#### `React.Children.only`

```javascript
React.Children.only(children)
```

Returns the only child in `children`. Throws otherwise.

#### `React.Children.toArray`

```javascript
React.Children.toArray(children)
```

Returns the `children` opaque data structure as a flat array with keys assigned to each child. Useful if you want to manipulate collections of children in your render methods, especially if you want to reorder or slice `this.props.children` before passing it down.

> Note:
>
> `React.Children.toArray()` changes keys to preserve the semantics of nested arrays when flattening lists of children. That is, `toArray` prefixes each key in the returned array so that each element's key is scoped to the input array containing it.

---

* 上一篇：[与其它类库集成](/cn/docs/integrating-with-other-libraries.md)
* 下一篇：[React.Component](/cn/docs/react-component.md)

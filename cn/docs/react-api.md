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

创建并返回一个给定类型的新 [React 元素](/cn/docs/rendering-elements.md) . type 参数可以是一个标签名字符串 (如 `'div'` 或 `'span'`), 或一个 [React 组件](/cn/docs/components-and-props.md) 类型 (一个类（class）或函数（ function）).

`React.DOM` 提供了 DOM 组件 `React.createElement()` 的快捷包装. 例如, `React.DOM.a(...)` 是 `React.createElement('a', ...)` 的快捷包装. 它们是历史遗留下来的功能，我们鼓励你使用 JSX 或直接使用 `React.createElement()`.

通过 [JSX](/cn/docs/introducing-jsx.md) 写代码将转换为使用 `React.createElement()`. 如果你使用 JSX ，那你将不必直接调用 `React.createElement()` . 查看 [不使用 JSX 的 React](/react/docs/react-without-jsx.html) 了解更多.

* * *

### `cloneElement()`

```
React.cloneElement(
  element,
  [props],
  [...children]
)
```

复制并返回一个使用 `element` 作为起点的新 React 元素. 所产生的元素将拥有源元素作为浅拷贝合并的属性. 新的子节点将替换现有子节点。 源元素中的 `key` 和 `ref` 将被保留.

`React.cloneElement()` 几乎相当于:

```js
<element.type {...element.props} {...props}>{children}</element.type>
```

但是，它也保留了 `ref`。 这意味着如果你通过 `ref` 获得一个子节点，不会在你的祖先节点中丢失。 您将获得与您的新元素相同的 `ref` 。

引入了这个API是作为废弃的 `React.addons.cloneWithProps()`的替代品。

* * *

### `createFactory()`

```javascript
React.createFactory(type)
```

返回一个生成给定类型的 React 元素的函数。类似 [`React.createElement()`](#createElement)，type 参数可以是一个标签名字符串(如 `'div'` 或 `'span'`)，或一个 [React 组件](/cn/docs/components-and-props.md) 类型 (一个类（class）或函数（ function）).

这个辅助方法是一个历史遗留功能，我们鼓励你使用 JSX 或直接使用 `React.createElement()`.

如果你使用 JSX ，那你将不必直接调用 `React.createFactory()` . 查看 [不使用 JSX 的 React](/react/docs/react-without-jsx.html) 了解更多.

* * *

### `isValidElement()`

```javascript
React.isValidElement(object)
```

验证对象是不是一个 React 元素。 返回 `true` 或 `false`。

* * *

### `React.Children`

`React.Children` 提供了处理 `this.props.children` 不透明数据结构的工具。

#### `React.Children.map`

```javascript
React.Children.map(children, function[(thisArg)])
```

在每个包含在 `children` 里的直接子节点上执行一个函数，通过 `this` 设置到 `thisArg`。如果 `children` 是一个键控的片段或数组，它将被遍历：该函数永远不会传递容器对象。如果 `children` 是 `null` 或 `undefined`，返回 `null` 或 `undefined` 而不是一个数组

#### `React.Children.forEach`

```javascript
React.Children.forEach(children, function[(thisArg)])
```

类似 [`React.Children.map()`](#react.children.map) 但不返回数组.

#### `React.Children.count`

```javascript
React.Children.count(children)
```

返回`children`中的组件总数，等于回调传递给 `map` 或 `forEach` 的次数。

#### `React.Children.only`

```javascript
React.Children.only(children)
```

返回 `children` 中唯一的子节点。抛弃其它的子节点。

#### `React.Children.toArray`

```javascript
React.Children.toArray(children)
```

将 `children` 的不透明数据结构作为扁平数组返回，并将键分配给每个子项。 如果您想要在渲染方法中操作子集的集合，尤其是如果要在将其传递下来之前重新排序或 分离 `this.props.children` 时非常有用。

> 注意:
>
> `React.Children.toArray（）`改变键，以便在平铺子节点列表时保留嵌套数组的语义。 也就是说，`toArray` 在返回的数组中的每个键作为前缀，以便每个元素的键被限定到包含它的输入数组。

---

* 上一篇：[与其它类库集成](/cn/docs/integrating-with-other-libraries.md)
* 下一篇：[React.Component](/cn/docs/react-component.md)

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
* [**`React.Component`**](/cn/docs/react-component.md)
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

# React.Component

[组件](/cn/docs/components-and-props.md) 可以让你将 UI 拆分成独立的、可复用的块，并单独思考每部分。 `React.Component` 是由 [`React`](/cn/docs/react-api.md) 提供的.

## 概况

`React.Component` 是一个抽象的基类, 因此直接引用 `React.Component` 一般没有什么意义，相反，你通常需要将它作为子类处理，并至少定义一个 [`render()`](#render) 方法.

通常你会将一个 React 组件定义为一个简单的 [JavaScript 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes):

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

如果你不使用 ES6，你可以使用 `create-react-class` 模块来代替. 查看 [不使用 ES6 的 React](/react/docs/react-without-es6.html) 了解更多.

### 组件生命周期

每个组件都有几个 "生命周期方法"，你可以在这个过程中的特定时间覆盖运行代码。前面带有 **`will`** 的方法在事情发生之前被调用，前端带有 **`did`** 的方法在事情发生之后调用。

#### 加载（Mounting）

当创建组件的实例并将其插入到DOM中时，将调用这些方法：

- [`constructor()`](#constructor)
- [`componentWillMount()`](#componentwillmount)
- [`render()`](#render)
- [`componentDidMount()`](#componentdidmount)

#### 更新（Updating）

更新可能是 属性 或 状态 的改变引起的。 当重新渲染组件时，将调用这些方法：

- [`componentWillReceiveProps()`](#componentwillreceiveprops)
- [`shouldComponentUpdate()`](#shouldcomponentupdate)
- [`componentWillUpdate()`](#componentwillupdate)
- [`render()`](#render)
- [`componentDidUpdate()`](#componentdidupdate)

#### 卸载（Unmounting）

当组件从 DOM 中移除时调用这些方法：

- [`componentWillUnmount()`](#componentwillunmount)

### 其它 API

每个组件也提供一些其它的 API：

  - [`setState()`](#setstate)
  - [`forceUpdate()`](#forceupdate)

### 类（Class）属性

  - [`defaultProps`](#defaultprops)
  - [`displayName`](#displayname)

### 实例（Instance)属性

  - [`props`](#props)
  - [`state`](#state)

* * *

## 参考

### `render()`

```javascript
render()
```

`render()` 是必须的.

调用时，它应该检查 `this.props` 和 `this.state` ，并且返回单个 React 元素. 这个元素可以是一个表示原生 DOM 的组件，类似 `<div />`, 或另一个你自己定义的复合组件.

你也能返回 `null` 或 `false` 用来表明你不做任何渲染。你返回 `null` 或 `false` 时，`ReactDOM.findDOMNode(this)` 将返回 `null`.

`render()` 函数应该是沌洁的，这意味着它不能修改组件状态，每次调用时它返回同样的结果，并且它不直接与浏览器产生影响。如果你需要在浏览器中发生作用，请在 `componentDidMount()` 或其它生命周期方法中执行，保持 `render()` 的纯洁可以确保组件更容易思考。

> 注意
>
> 如果 [`shouldComponentUpdate()`](#shouldcomponentupdate) 返回 false，`render()` 将不会被调用.

* * *

### `constructor()`

```javascript
constructor(props)
```

React 组件的 constructor 在加载前调用。 当执行 `React.Component` 子类的 constructor 时，你应该在其它声明前调用 `super(props)` . 否则, `this.props` 在 constructor 里将是 undefined, 这可能会导致 bug.

constructor 是初始化状态的正确位置。如果你不需要初始化状态，并且不需要绑定方法，你就不需要为 React 组件执行一个constructor。

根据 props 来初始化状态是可以的。这会有效的 "forks" props，并且根据初始化的 props 来设置 state. 这里有一个有效的 `React.Component` 子类 constructor 示例:

```js
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```

当心这种模式, state 将不会随着任何 props 更新而更新。你不是同步 props 到 state, 而是经常需要 [提升 state ](/cn/docs/lifting-state-up.md).

如果你通过使用 state 来 "fork" props, 你可能也要执行 [`componentWillReceiveProps(nextProps)`](#componentwillreceiveprops) 来确保 state 随着他们而改变。但提升 state 通常更容易且更不易出错。

* * *

### `componentWillMount()`

```javascript
componentWillMount()
```

在挂载之前立即调用 `componentWillMount()` 。它在 `render()` 之前执行，因此设置在这个方法里同步设置 state 将不会触发重绘。避免在这个方法里引入任何副作用或订阅。

这是在服务器端渲染唯一的调用的生命周期钩子，我们建议使用 `constructor()` 来代替.

* * *

### `componentDidMount()`

```javascript
componentDidMount()
```

组件挂载之后立即执行 `componentDidMount()` ，应该在这里初始化 DOM 节点。如果你需要从远端加载数据，这里是实例化网络请求的好地方。在这个方法里设置 state 将触发重绘。

* * *

### `componentWillReceiveProps()`

```javascript
componentWillReceiveProps(nextProps)
```

已挂载的组件接收新 prop 之前立即执行 `componentWillReceiveProps()` 。如果你需要更新状态以响应 prop 变化 (例如重置)，你可以在这个方法里比较 `this.props` 和 `nextProps` ，并通过 `this.setState()` 执行状态转换。

注意，即使 prop 没有变化，React 也可能调用这个方法，因此如果你只想在变化时处理，请确保比较当前值和下一个值，这可能会发生在当你的父组件引起的重绘时。

在 [挂载](#mounting) 时的初始化 prop 不会调用 `componentWillReceiveProps`。如果组件的某些 prop 可能更新时，它只会调用这个方法。调用 `this.setState` 通常不会触发 `componentWillReceiveProps`。

* * *

### `shouldComponentUpdate()`

```javascript
shouldComponentUpdate(nextProps, nextState)
```

使用 `shouldComponentUpdate()` 是为了让 React 知道组件的输出是否会被 state 或 prop 当前的变化所影响。默认行为是在每次 state 状态变化时重绘，在绝大多数情况下，你应该依赖默认行为。

`shouldComponentUpdate()` 在新 prop 或 state 被接收并渲染前调用。默认返回 `true`. 这个方法在初始化渲染或 `forceUpdate()` 被使用时不调用。

当*它们的* state 变化时返回 `false` 不会阻止子组件重绘。

目前，如果 `shouldComponentUpdate()` 返回 `false`,  [`componentWillUpdate()`](#componentwillupdate), [`render()`](#render), 和 [`componentDidUpdate()`](#componentdidupdate) 将不会调用。注意，未来 React 可能把 `shouldComponentUpdate()` 看作一个提示而不是一个严格的指令，并且返回 `false` 可能结果还是会导致组件的重绘。

如果你确定特定组件在性能分析后还是很慢，你可以从 [`React.PureComponent`](/cn/docs/react-api.md#react.purecomponent) 继承，它通过一个浅的 prop 和 state 比较来执行 `shouldComponentUpdate()` 。如果你有信心手写功能，你也可以比较比较 `this.props` 和 `nextProps` 、 `this.state` 和 `nextState` ，并返回 `false` 来通知 React 跳过更新。

* * *

### `componentWillUpdate()`

```javascript
componentWillUpdate(nextProps, nextState)
```

当新 prop 或 state 被接收并渲染前立即执行 `componentWillUpdate()` 。使用此作为在更新发生之前做其它准备的机会。初始化渲染不会调用此方法。

注意在这里你不能调用 `this.setState()` 。如果你需要更新 state 以响应 prop 的变化，请使用 `componentWillReceiveProps()` 。

> 提示
>
> 如果 [`shouldComponentUpdate()`](#shouldcomponentupdate) 返回 false，`componentWillUpdate()` 将不会调用

* * *

### `componentDidUpdate()`

```javascript
componentDidUpdate(prevProps, prevState)
```

更新完成后立即调用 `componentDidUpdate()` ，初始化渲染不会调用此方法。

使用此作为在组件已经更新完成后操作 DOM 的机会。如果你要比较当前 prop 和之前的 prop ，这里是一个发送网络请求的好地方 (如果 props 没有变化，网络请求也可能不是必须的).

> 提示
>
> 如果 [`shouldComponentUpdate()`](#shouldcomponentupdate) 返回 false，`componentDidUpdate()` 将不会调用。

* * *

### `componentWillUnmount()`

```javascript
componentWillUnmount()
```

组件卸载和销毁之前立即调用 `componentWillUnmount()` 。 在这个方法里执行一些必须的清除操作，类似初始化定时器，取消网络请求，或清除在 `componentDidMount` 中创建的 DOM 元素。

* * *

### `setState()`

```javascript
setState(updater, [callback])
```

`setState()` 以队列的方式改变组件的 state，并通知 React 此组件及它的子节点需要使用新的 state 重绘。这是用于响应事件处理程序和服务器响应更新用户界面的主要方法。

考虑将 `setState()` 作为一个 *请求* 而不是一个立即执行的命令来更新组件。为了更好的感知性能，React 可能会延迟执行，并一次更新多个组件。React 不保证立即应用 state 的改变。

`setState()` 并不总是立即更新组件。它可能会批量或延迟更新。这样就可能在调用 `setState()' 之后看到 `this.state` 的一个潜在陷阱。相反，使用 `componentDidUpdate` 或 `setState` 回调（`setState(updater，callback)`），两者在应用更新后都被保证触发。 如果需要根据先前的状态设置状态，请阅读下面的 `updater` 参数

除非 `shouldComponentUpdate()` 返回 `false`，否则 `setStat()` 将一直引发重新渲染。 如果正在使用可变对象，并且 `shouldComponentUpdate()` 中无法实现条件渲染逻辑，则仅当新状态与先前状态不同时调用 `setState()` 将避免不必要的重新渲染。

第一个参数是带有签名的 `updater` 函数：

```javascript
(prevState, props) => stateChange
```

`prevState` 是对前一个状态的引用。不应该直接改变。相反，应该根据 `prevState` 和 'props` 的输入构建一个新对象来表示更改。例如，假设我们想通过 `props.step` 在状态中增加一个值：

```javascript
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```

`prevState` 和 `props` 都通过 updater 函数接收可以保证是最新的。 updater 的输出与 `prevState` 进行浅合并。

`setStat()` 的第二个参数是一个可选的回调函数，一旦 `setState` 完成并重新渲染该组件，该函数将被执行。 通常我们建议使用 `componentDidUpdate()` 来代替。

您可以选择将一个对象作为第一个参数传递给 `setState()` 而不是一个函数：

```javascript
setState(stateChange, [callback])
```

这将执行 `stateChange` 的浅合并到新状态，例如调整购物车项目数量：

```javascript
this.setState({quantity: 2})
```

这种形式的 `setState()` 也是异步的，同一周期中的多个调用可能被合并在一起处理。 例如，如果您尝试在同一周期内多次增加项目数量，则将导致相当于：

```javaScript
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```

后面的调用将覆盖同一周期中之前调用的值，因此数量只会增加一次。 如果下一个状态取决于以前的状态，我们建议使用 updater 函数形式：

```js
this.setState((prevState) => {
  return {counter: prevState.quantity + 1};
});
```

要了解更多详情，请查阅 [状态和生命周期指南](/cn/docs/state-and-lifecycle.md).

* * *

### `forceUpdate()`

```javascript
component.forceUpdate(callback)
```

默认情况下，当组件的状态或道具更改时，您的组件将重新渲染。 如果你的 `render()` 方法依赖于一些其他数据，你可以通过调用 `forceUpdate()` 来告诉 React 这个组件需要重新渲染。

调用 `forceUpdate()` 会导致在组件上调用 `render()`，跳过 `shouldComponentUpdate()` 。这将触发子组件的正常生命周期方法，包括每个子级的 `shouldComponentUpdate()` 方法。 如果标记更改，则 React 仍将仅更新DOM。

通常你应该尽量避免使用 `forceUpdate()`，只能从 `render()` 中读取 `this.props` 和 `this.state。

* * *

## 类（Class）属性

### `defaultProps`

`defaultProps` 可以定义为组件类本身的属性，以设置类的默认 prop。 这用于值为 undefined 的 prop，但不用于值为 null 的 prop。 例如：

```js
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
```

如果 `props.color` 未指定，它将以 `'blue'` 作为默认值:

```js
  render() {
    return <CustomButton /> ; // props.color 将设置为 blue
  }
```

如果 `props.color` 设定为 null，它将保持为 null:

```js
  render() {
    return <CustomButton color={null} /> ; // props.color 将保持为 null
  }
```

* * *

### `displayName`

`displayName` 字符串用于调试信息， JSX 会自动设置这个值；查阅 [深入 JSX](/cn/docs/jsx-in-depth.md).

* * *

## 实例（Instance）属性

### `props`

`this.props` 包含由该组件的调用者定义的 prop。 有关 prop 的介绍，请参阅[组件和属性](/cn/docs/components-and-props.md)。

比较特别的是，`this.props.children` 是一个特殊的 prop，通常由JSX表达式中的子标签定义，而不是标签本身。

### `state`

state 包含特定于该组件的数据，该数据可能随时变化。 state 是用户定义的，它应该是一个纯粹的 JavaScript 对象。

如果你不在 `render()' 中使用它，它不应该出现在 state 中。 例如，您可以直接在实例上放置定时器ID。

有关 state 的详细信息，请参阅 [状态和生命周期](/cn/docs/state-and-lifecycle.md) 。

不要直接改变 `this.state`，因为调用 `setState()' 可能会覆盖你所做的改变。 把 'this.state` 看作是不可变数据。

---

* 上一篇：[React API](/cn/docs/react-api.md)
* 下一篇：[ReactDOM](/cn/docs/react-dom.md)

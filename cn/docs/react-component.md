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

The constructor for a React component is called before it is mounted. When implementing the constructor for a `React.Component` subclass, you should call `super(props)` before any other statement. Otherwise, `this.props` will be undefined in the constructor, which can lead to bugs.

The constructor is the right place to initialize state. If you don't initialize state and you don't bind methods, you don't need to implement a constructor for your React component.

It's okay to initialize state based on props. This effectively "forks" the props and sets the state with the initial props. Here's an example of a valid `React.Component` subclass constructor:

```js
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```

Beware of this pattern, as state won't be up-to-date with any props update. Instead of syncing props to state, you often want to [lift the state up](/react/docs/lifting-state-up.html).

If you "fork" props by using them for state, you might also want to implement [`componentWillReceiveProps(nextProps)`](#componentwillreceiveprops) to keep the state up-to-date with them. But lifting state up is often easier and less bug-prone.

* * *

### `componentWillMount()`

```javascript
componentWillMount()
```

`componentWillMount()` is invoked immediately before mounting occurs. It is called before `render()`, therefore setting state synchronously in this method will not trigger a re-rendering. Avoid introducing any side-effects or subscriptions in this method.

This is the only lifecycle hook called on server rendering. Generally, we recommend using the `constructor()` instead.

* * *

### `componentDidMount()`

```javascript
componentDidMount()
```

`componentDidMount()` is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. If you need to load data from a remote endpoint, this is a good place to instantiate the network request. Setting state in this method will trigger a re-rendering.

* * *

### `componentWillReceiveProps()`

```javascript
componentWillReceiveProps(nextProps)
```

`componentWillReceiveProps()` is invoked before a mounted component receives new props. If you need to update the state in response to prop changes (for example, to reset it), you may compare `this.props` and `nextProps` and perform state transitions using `this.setState()` in this method.

Note that React may call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.

React doesn't call `componentWillReceiveProps` with initial props during [mounting](#mounting). It only calls this method if some of component's props may update. Calling `this.setState` generally doesn't trigger `componentWillReceiveProps`.

* * *

### `shouldComponentUpdate()`

```javascript
shouldComponentUpdate(nextProps, nextState)
```

Use `shouldComponentUpdate()` to let React know if a component's output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.

`shouldComponentUpdate()` is invoked before rendering when new props or state are being received. Defaults to `true`. This method is not called for the initial render or when `forceUpdate()` is used.

Returning `false` does not prevent child components from re-rendering when *their* state changes.

Currently, if `shouldComponentUpdate()` returns `false`, then [`componentWillUpdate()`](#componentwillupdate), [`render()`](#render), and [`componentDidUpdate()`](#componentdidupdate) will not be invoked. Note that in the future React may treat `shouldComponentUpdate()` as a hint rather than a strict directive, and returning `false` may still result in a re-rendering of the component.

If you determine a specific component is slow after profiling, you may change it to inherit from [`React.PureComponent`](/react/docs/react-api.html#react.purecomponent) which implements `shouldComponentUpdate()` with a shallow prop and state comparison. If you are confident you want to write it by hand, you may compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` to tell React the update can be skipped.

* * *

### `componentWillUpdate()`

```javascript
componentWillUpdate(nextProps, nextState)
```

`componentWillUpdate()` is invoked immediately before rendering when new props or state are being received. Use this as an opportunity to perform preparation before an update occurs. This method is not called for the initial render.

Note that you cannot call `this.setState()` here. If you need to update state in response to a prop change, use `componentWillReceiveProps()` instead.

> Note
>
> `componentWillUpdate()` will not be invoked if [`shouldComponentUpdate()`](#shouldcomponentupdate) returns false.

* * *

### `componentDidUpdate()`

```javascript
componentDidUpdate(prevProps, prevState)
```

`componentDidUpdate()` is invoked immediately after updating occurs. This method is not called for the initial render.

Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed).

> Note
>
> `componentDidUpdate()` will not be invoked if [`shouldComponentUpdate()`](#shouldcomponentupdate) returns false.

* * *

### `componentWillUnmount()`

```javascript
componentWillUnmount()
```

`componentWillUnmount()` is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any DOM elements that were created in `componentDidMount`

* * *

### `setState()`

```javascript
setState(updater, [callback])
```

`setState()` enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the updated state. This is the primary method you use to update the user interface in response to event handlers and server responses.

Think of `setState()` as a *request* rather than an immediate command to update the component. For better perceived performance, React may delay it, and then update several components in a single pass. React does not guarantee that the state changes are applied immediately.

`setState()` does not always immediately update the component. It may batch or defer the update until later. This makes reading `this.state` right after calling `setState()` a potential pitfall. Instead, use `componentDidUpdate` or a `setState` callback (`setState(updater, callback)`), either of which are guaranteed to fire after the update has been applied. If you need to set the state based on the previous state, read about the `updater` argument below.

`setState()` will always lead to a re-render unless `shouldComponentUpdate()` returns `false`. If mutable objects are being used and conditional rendering logic cannot be implemented in `shouldComponentUpdate()`, calling `setState()` only when the new state differs from the previous state will avoid unnecessary re-renders.

The first argument is an `updater` function with the signature:

```javascript
(prevState, props) => stateChange
```

`prevState` is a reference to the previous state. It should not be directly mutated. Instead, changes should be represented by building a new object based on the input from `prevState` and `props`. For instance, suppose we wanted to increment a value in state by `props.step`:

```javascript
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```

Both `prevState` and `props` received by the updater function are guaranteed to be up-to-date. The output of the updater is shallowly merged with `prevState`.

The second parameter to `setState()` is an optional callback function that will be executed once `setState` is completed and the component is re-rendered. Generally we recommend using `componentDidUpdate()` for such logic instead.

You may optionally pass an object as the first argument to `setState()` instead of a function:

```javascript
setState(stateChange, [callback])
```

This performs a shallow merge of `stateChange` into the new state, e.g., to adjust a shopping cart item quantity:

```javascript
this.setState({quantity: 2})
```

This form of `setState()` is also asynchronous, and multiple calls during the same cycle may be batched together. For example, if you attempt to increment an item quantity more than once in the same cycle, that will result in the equivalent of:

```javaScript
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```

Subsequent calls will override values from previous calls in the same cycle, so the quantity will only be incremented once. If the next state depends on the previous state, we recommend using the updater function form, instead:

```js
this.setState((prevState) => {
  return {counter: prevState.quantity + 1};
});
```

For more detail, see the [State and Lifecycle guide](/react/docs/state-and-lifecycle.html).

* * *

### `forceUpdate()`

```javascript
component.forceUpdate(callback)
```

By default, when your component's state or props change, your component will re-render. If your `render()` method depends on some other data, you can tell React that the component needs re-rendering by calling `forceUpdate()`.

Calling `forceUpdate()` will cause `render()` to be called on the component, skipping `shouldComponentUpdate()`. This will trigger the normal lifecycle methods for child components, including the `shouldComponentUpdate()` method of each child. React will still only update the DOM if the markup changes.

Normally you should try to avoid all uses of `forceUpdate()` and only read from `this.props` and `this.state` in `render()`.

* * *

## 类（Class）属性

### `defaultProps`

`defaultProps` can be defined as a property on the component class itself, to set the default props for the class. This is used for undefined props, but not for null props. For example:

```js
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
```

If `props.color` is not provided, it will be set by default to `'blue'`:

```js
  render() {
    return <CustomButton /> ; // props.color will be set to blue
  }
```

If `props.color` is set to null, it will remain null:

```js
  render() {
    return <CustomButton color={null} /> ; // props.color will remain null
  }
```

* * *

### `displayName`

The `displayName` string is used in debugging messages. JSX sets this value automatically; see [JSX in Depth](/react/docs/jsx-in-depth.html).

* * *

## 实例（Instance）属性

### `props`

`this.props` contains the props that were defined by the caller of this component. See [Components and Props](/react/docs/components-and-props.html) for an introduction to props.

In particular, `this.props.children` is a special prop, typically defined by the child tags in the JSX expression rather than in the tag itself.

### `state`

The state contains data specific to this component that may change over time. The state is user-defined, and it should be a plain JavaScript object.

If you don't use it in `render()`, it shouldn't be on the state. For example, you can put timer IDs directly on the instance.

See [State and Lifecycle](/react/docs/state-and-lifecycle.html) for more information about the state.

Never mutate `this.state` directly, as calling `setState()` afterwards may replace the mutation you made. Treat `this.state` as if it were immutable.


---

* 上一篇：[React API](/cn/docs/react-api.md)
* 下一篇：[ReactDOM](/cn/docs/react-dom.md)

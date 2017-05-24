[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

---
<details>
  <summary>导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md")
* [JSX 介绍](/cn/docs/introducing-jsx.md)
* [渲染元素](/cn/docs/rendering-elements.md)
* [**`组件和Props`**](/cn/docs/components-and-props.md)
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

# 组件和Props

组件让你把 UI 分割成独立的、可复用的块，并且可以对每块进行单独的思考。

从概念上讲，组件像 JavaScript 的函数。它接受任意的输入 (称为 "props") ，并且返回用于描述屏幕上显示内容的 React 元素。

## 函数式组件和类组件

最简单的定义一个组件的方式是写一个 Javascript 函数：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

函数是一个合法的 React 组件，因为它接受一个 "props" 对象参数并且返回一个 React 元素。我们称这类组件叫 "函数式(Functional)"，因为从字面上看它就是一个 Javascript 函数。 

你也可以使用 [ES6 类(class)](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 来定义一个组件：

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上面两个组件从 React 的角度是等价的。

类(Classes)有一些附加的特性，我们将在 [下一章节](/cn/docs/state-and-lifecycle.md) 来讨论。在这之前，我们将使用函数式(functional)组件，因为他们比较简单。

## 渲染一个组件

之前，我们只遇到过表示 DOM 标签的 React 元素：

```js
const element = <div />;
```

然而，元素也能表示用户自定义组件：

```js
const element = <Welcome name="Sara" />;
```

当 React 遇到一个表示用户自定义组件的元素时，它将 JSX 属性给作为一个单独的对象传递给这个组件，我们称它为 "props" 对象。

例如，这段代码在页面上渲染 "Hello, Sara" ：

```js{1,5}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在 CodePen 中尝试一下](http://codepen.io/gaearon/pen/YGYmEG?editors=0010)

让我们简要的重述下上面这个示例：

1. 我们调用 `ReactDOM.render()` 方法，并将 `<Welcome name="Sara" />` 作为参数传入。
2. React 调用 `Welcome` 组件，并将 `{name: 'Sara'}` 作为 props 对象传入。
3. 我们的 `Welcome` 组件返回一个 `<h1>Hello, Sara</h1>` 元素。
4. React DOM 迅速更新 DOM 使其显示为 `<h1>Hello, Sara</h1>`。

>**警告:**
>
>组件名总是以大写字母开头。
>
>例如，`<div />` 表示一个 DOM 标签，但是 `<Welcome />` 表示一个组件，并且需要在作用域中有一个 `Welcome` 组件。

## 构成组件

组件能在它们的返回中引用其它组件。这让我们可以使用相同的组件来抽象到任意的层级。按钮、表单、弹层、屏幕：在 React 应用中，所有这些通常都描述为组件。

例如，我们能创建一个 `App` 组件渲染 `Welcome` 组件很多次：

```js{8-10}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/KgQKPr?editors=0010)

通常，新的 React 应用有一个 `APP` 组件在最上层，然后，如果你将 React 融入到一个已有的应用中，你可能需要自下而上的，从类似 `Button` 这样的小组件开始，逐渐整合到视图最顶层。

>**警告:**
>
>组件必须返回一个根元素，这是为什么我们添加一个 `<div>` 来包裹所有 `<Welcome />` 元素的原因。

## Extracting Components

Don't be afraid to split components into smaller components.

For example, consider this `Comment` component:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/VKQwEo?editors=0010)

It accepts `author` (an object), `text` (a string), and `date` (a date) as props, and describes a comment on a social media website.

This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let's extract a few components from it.

First, we will extract `Avatar`:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

The `Avatar` doesn't need to know that it is being rendered inside a `Comment`. This is why we have given its prop a more generic name: `user` rather than `author`.

We recommend naming props from the component's own point of view rather than the context in which it is being used.

We can now simplify `Comment` a tiny bit:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Next, we will extract a `UserInfo` component that renders an `Avatar` next to user's name:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

This lets us simplify `Comment` even further:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/rrJNJY?editors=0010)

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be a reusable component.

## Props are Read-Only

Whether you declare a component [as a function or a class](#functional-and-class-components), it must never modify its own props. Consider this `sum` function:

```js
function sum(a, b) {
  return a + b;
}
```

Such functions are called ["pure"](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.

In contrast, this function is impure because it changes its own input:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React is pretty flexible but it has a single strict rule:

**All React components must act like pure functions with respect to their props.**

Of course, application UIs are dynamic and change over time. In the [next section](/react/docs/state-and-lifecycle.html), we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.

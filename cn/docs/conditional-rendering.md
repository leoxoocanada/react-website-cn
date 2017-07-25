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
* [**`条件渲染`**](/cn/docs/conditional-rendering.md)
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

# 条件渲染

在 React 中, 你能创建不同的组件封装你所需要的行为。然后，只渲染它们中的一些，取决于你应用中的状态。

在 React 中的条件渲染就和在 Javascript 中的条件渲染一样. 使用像 [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 或 [条件运算符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) 这样的运算符来创建表示当前状态的元素, 并且让 React 更新匹配的 UI .

考虑这2个组件:

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

我们将创建一个 `Greeting` 组件，根据用户是否登录来显示这些组件中的一个:

```javascript{3-7,11,12}
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // 修改为 isLoggedIn={true} 试试:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

这个示例根据 `isLoggedIn` 属性的值渲染不同的 greeting。

### 元素变量

你能使用变量来存储元素。这能帮助你有条件的渲染组件的一部分，而输出其余部分不会更改。

思考这2个表示退出和登录按钮的新组件：

```js
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

在这个示例下面，我们将创建一个 [有状态的组件](/cn/docs/state-and-lifecycle.md#adding-local-state-to-a-class) `LoginControl`.

它将渲染 `<LoginButton />` 或 `<LogoutButton />` ，根据它的当前状态. 同时渲染前面提到的 `<Greeting />` :

```javascript{20-25,29,30}
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

虽然声明一个变量且使用一个 `if` 声明是一个有条件的渲染一个组件的好办法，有时候你可能想要使用一个更短的语法。在 JSX 中有几种内联条件方法，如下所述：

### 使用逻辑 && 操作符的内联 if 用法

你可以通过用花括号包裹它们 [在 JSX 中嵌入一些表达式](/cn/docs/introducing-jsx.md#embedding-expressions-in-jsx) . 这也包括 JavaScript 逻辑 `&&` 运算符. 它有助于有条件的包含一个元素:

```js{6-10}
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

它可以正常运行，因为在 JavaScript 中, `true && expression` 总是评估为 `expression`, 而 `false && expression` 总是评估为 `false`.

因此, 假如条件是 `true`, 在 `&&` 右侧的元素将在输出中显示. 如果它是 `false`, React 将忽略并跳过它.

### 使用条件运算符的内联 If-Else 

另一个用于条件渲染元素的内联方法是使用 JavaScript 的 条件运算符 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

在下面这个示例中，我们使用它来条件渲染一个小的文本块。

```javascript{5}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

它也能被用在更大的表达式上，尽管不太明显发生了什么：

```js{5,7,9}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```
就像在 Javascript 中一样，你可以根据你和你的团队认为更易于阅读的方式选择合适的风格。还要记住，无论何时何地，当条件变得太复杂时，可能是 [提取组件](/cn/docs/components-and-props.md#extracting-components) 的好时机。

### 防止组件渲染

在一些罕见的案例中，你可能要隐藏一个组件自身，即使它已经被另外一个组件渲染。为此，返回 `null` 而不是渲染输出。

在下面这个示例中，根据名为 `warn` 的属性值渲染 `<WarningBanner />` 。如果这个属性的值是 `false` ，这个组件将不会渲染：

```javascript{2-4,29}
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

一个组件的 `render` 方法返回 `null`，不影响这个组件的第一个生命周期方法。例如， `componentWillUpdate` 和 `componentDidUpdate` 将还是会被调用。

---

* 上一篇：[事件处理](/cn/docs/handling-events.md)
* 下一篇：[列表和键](/cn/docs/lists-and-keys.md)

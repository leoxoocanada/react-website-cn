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
* [**`上下文（Context）`**](/cn/docs/context.md)
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

# 上下文（Context）

>注意： 
> 从 React v15.5 开始 ，`React.PropTypes` 助手函数已被弃用，我们建议使用  [`prop-types` 库](https://www.npmjs.com/package/prop-types)  来定义 `contextTypes`.

使用React，您可以轻松地通过React组件跟踪数据流。 当你看一个组件，你可以看到哪些属性被传递，这使得你的应用程序容易理解。

在某些情况下，您希望通过组件树传递数据，而无需在每个级别手动传递属性。
您可以使用强大的"context" API在React中直接执行此操作。

## 为什么不使用 Context

绝大多数应用不需要使用 context。

如果您希望应用程序稳定，请勿使用context。 它是一个实验API，它可能在将来的React版本中改变。

如果您不熟悉像[Redux]（https://github.com/reactjs/redux）或[MobX]（https://github.com/mobxjs/mobx）等状态管理库，请勿使用context。 对于许多实际应用，这些库及其React绑定是管理与许多组件相关的状态的不错选择。 Redux更可能是您的问题的正确解决方案，而context不是正确的解决方案。

如果您不是经验丰富的React开发人员，请勿使用context。 通常使用props 和 state来实现功能是更好方法。

尽管有这些警告，如果你坚持使用context，尝试在一个小的隔离区域使用context，避免在可能的情况下直接使用 context API ，以便当API变动时比较容易升级。

## 如何使用 Context

假设你有一个像这样的结构：

```javascript
class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.props.color}}>
        {this.props.children}
      </button>
    );
  }
}

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button color={this.props.color}>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  render() {
    const color = "purple";
    const children = this.props.messages.map((message) =>
      <Message text={message.text} color={color} />
    );
    return <div>{children}</div>;
  }
}
```

在这个例子中，我们手动传递一个 `color` 属性使得 `Button` 和 `Message` 组件正确的显示样式。使用 context，我们能通过树自动的传递：

```javascript
const PropTypes = require('prop-types');

class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.context.color}}>
        {this.props.children}
      </button>
    );
  }
}

Button.contextTypes = {
  color: PropTypes.string
};

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  getChildContext() {
    return {color: "purple"};
  }

  render() {
    const children = this.props.messages.map((message) =>
      <Message text={message.text} />
    );
    return <div>{children}</div>;
  }
}

MessageList.childContextTypes = {
  color: PropTypes.string
};
```

通过添加 `childContextTypes` 和 `getChildContext` 到 `MessageList` (context 提供者),React 自动往下传递信息，在子树上的任意组件(在这里是 `Button`)能通过定义`contextTypes`接收到它。

如果 `contextTypes` 没有定义,  `context` 将是一个空对象.

## 父子耦合

Context 也能让你构建一个父子通讯的 API。例如，一个通过这种方式运行的库是 [React Router V4](https://reacttraining.com/react-router):

```javascript
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr />

      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/topics" component={Topics} />
    </div>
  </Router>
);
```

通过从 `Router` 组件往下传递一些信息, 每个 `Link` 和 `Route` 都能回传到包含的容器 `Router` .

在使用与此类似的API构建组件之前，请考虑是否有更干净的选择。 例如，如果您愿意，您可以将整个React组件作为 属性 传递。

## 在生命周期方法中引用 Context

如果在组件里定义 `contextTypes` ，下面的 [生命周期方法](/cn/docs/react-component.md#the-component-lifecycle) 将接收一个附加参数 `context` 对象:

- [`constructor(props, context)`](/cn/docs/react-component.md#constructor)
- [`componentWillReceiveProps(nextProps, nextContext)`](/cn/docs/react-component.md#componentwillreceiveprops)
- [`shouldComponentUpdate(nextProps, nextState, nextContext)`](/cn/docs/react-component.md#shouldcomponentupdate)
- [`componentWillUpdate(nextProps, nextState, nextContext)`](/cn/docs/react-component.md#componentwillupdate)
- [`componentDidUpdate(prevProps, prevState, prevContext)`](/cn/docs/react-component.md#componentdidupdate)

## 在无状态的函数式组件中引用 Context

如果 `contextTypes` 作为一个函数的属性定义，无状态的函数式组件也可能引用 `context` 。下面的代码显示了一个作为无状态函数组件编写的 `Button` 组件。

```javascript
const PropTypes = require('prop-types');

const Button = ({children}, context) =>
  <button style={{background: context.color}}>
    {children}
  </button>;

Button.contextTypes = {color: PropTypes.string};
```

## 更新 Context

不要这么做

React 有一个更新 context 的 API , 但是它打破了基本流程，不应该使用。

当state 或 props变化时，`getChildContext` 函数将被调用 。为了在context里更新数据, 通过 `this.setState` 触发本地的状态更新. 这将触发一个新的 context ，子节点将接收到变化.

```javascript
const PropTypes = require('prop-types');

class MediaQuery extends React.Component {
  constructor(props) {
    super(props);
    this.state = {type:'desktop'};
  }

  getChildContext() {
    return {type: this.state.type};
  }

  componentDidMount() {
    const checkMediaQuery = () => {
      const type = window.matchMedia("(min-width: 1025px)").matches ? 'desktop' : 'mobile';
      if (type !== this.state.type) {
        this.setState({type});
      }
    };

    window.addEventListener('resize', checkMediaQuery);
    checkMediaQuery();
  }

  render() {
    return this.props.children;
  }
}

MediaQuery.childContextTypes = {
  type: PropTypes.string
};
```

问题是，如果由组件提供的 context 值更改，则使用该值的后代如果中间父项从`shouldComponentUpdate'返回`false`将不会更新。 这完全不能使用 context 来控制组件，所以基本上没有办法可靠地更新 context。 [这个博客文章](https://medium.com/@mweststrate/how-to-safely-use-react-context-b7e343eff076) 有一个很好的解释，为什么这是一个问题，以及如何绕过它。


---

* 上一篇：[一致性比较（Reconciliation）](/cn/docs/reconciliation.md)
* 下一篇：[Web Components](/cn/docs/web-components.md)


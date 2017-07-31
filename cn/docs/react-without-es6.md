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
* [**`不使用 ES6 的 React`**](/cn/docs/react-without-es6.md)
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

# 不使用 ES6 的 React

通常你将以纯 Javascript 类的方式定义一个 React 组件：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

如果你还没有使用 ES6，你可以使用 `create-react-class` 模块：

```javascript
var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
```

除了一些例外，ES6 classes(类) API 非常类似于函数 createReactClass() 。

## 声明默认属性（Props）

通过函数式和 ES6 类 `defaultProps` 被定义作为组件自身的一个属性：

```javascript
class Greeting extends React.Component {
  // ...
}

Greeting.defaultProps = {
  name: 'Mary'
};
```

通过 `createReactClass()`,你需要定义 `getDefaultProps()` 作为传递对象的函数：

```javascript
var Greeting = createReactClass({
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },

  // ...

});
```

## 设置初始化状态（State）

在 ES6 classes（类）里，你能通过在 constructor 里赋值 `this.state` 定义一个初始化状态：

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
  // ...
}
```

通过 `createReactClass()`, 你必须指定一个单独的 `getInitialState` 方法返回初始化状态：

```javascript
var Counter = createReactClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // ...
});
```

## 自动绑定

在 ES6 classes（类）声明的 React 组件里，方法遵循常规 ES6 classes（类）相同的语义。这意味着它们不能自动绑定 `this` 到实例。你必须在 constructor  里明确的使用 `.bind(this)` ：

```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
    // 这行很重要!
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    alert(this.state.message);
  }

  render() {
    // 因为 `this.handleClick` 已经绑定，我们能作为事件处理器使用它
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

通过 `createReactClass()`, 这不是必须的，因为它给所有方法都绑定了:

```javascript
var SayHello = createReactClass({
  getInitialState: function() {
    return {message: 'Hello!'};
  },

  handleClick: function() {
    alert(this.state.message);
  },

  render: function() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
});
```

这意味着写 ES6 classes（类）会为事件处理器带来更多的样本代码，但是优势是在大型的应用里会带来轻微的性能提升：

如果对你而言样本代码非常不美观，你能通过 Babel 启用**实验性的** [Class Properties（类属性）](https://babeljs.io/docs/plugins/transform-class-properties/) 语法提案:

```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
  }
  // 警告: 这个语法是实验性的！
  // 使用一个箭头函数绑定方法:
  handleClick = () => {
    alert(this.state.message);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

请注意上面这个语法是**实验性的**，并且这个语法可能有变化，或者提案可能不会进入语言。

如果你想安全的使用它，你有几个选择：

* 在 constructor 里绑定方法
* 使用箭头函数，例如。`onClick={(e) => this.handleClick(e)}`.
* 继续使用 `createReactClass`.

## Mixins

>**注意:**
>
>ES6 是不支持 mixin 的. 因此，当你在 React 里使用 ES6 classes 时它是不支持 mixins.
>
>**我们在使用 mixins 时也发现了许多问题，, [所以在新的代码中不推荐使用](/cn/blog/2016/07/13/mixins-considered-harmful.md).**
>
>以下部分仅用来参考

有时一些不同的组件可能需要分享一些通用的方法。这些方法被称为[横切关注点(cross-cutting concerns)](https://en.wikipedia.org/wiki/Cross-cutting_concern). [`createReactClass`](/cn/docs/top-level-api.md#react.createclass) 使得你可以使用一个 `mixins`。

一个通用的案例是一个组件要间隔一段时间自我更新。使用 `setInterval()` 很容易实现，但是为了节省内存空间必须在不使用时取消。React 提供了 [生命周期方法](/cn/docs/working-with-the-browser.md#component-lifecycle) 可以通知你组件是创建还是销毁。我们来创建一个简单的 mixin，通过使用这些方法实现一个简单的 `setInterval()` 函数，它将在你的组件被销毁的时候自动被清除。

```javascript
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.forEach(clearInterval);
  }
};

var createReactClass = require('create-react-class');

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // 使用 mixin
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // 调用在 mixin 上的方法
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

ReactDOM.render(
  <TickTock />,
  document.getElementById('example')
);
```

如果一个组件是使用多个 mixins，且几个 mixins 定时相同的命名空间方法（也就是说当组件被销毁时几个 mixins 要做一些清除的事），所有的命名空间方法都会被调用。在组件内部的生命周期方法执行完毕后，mixin中的方法将会按照mixin的顺序依次执行。


---

* 上一篇：[性能优化](/cn/docs/optimizing-performance.md)
* 下一篇：[不使用 JSX 的 React](/cn/docs/react-without-jsx.md)

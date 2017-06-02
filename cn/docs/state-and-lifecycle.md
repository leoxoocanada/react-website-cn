[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

---
<details>
  <summary>导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md")
* [JSX 介绍](/cn/docs/introducing-jsx.md)
* [渲染元素](/cn/docs/rendering-elements.md)
* [组件和Props](/cn/docs/components-and-props.md)
* [**`State和生命周期`**](/cn/docs/state-and-lifecycle.md)
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

# State和生命周期

思考 [前面章节](/cn/docs/rendering-elements.md#更新和渲染元素) 中提到的时钟例子。

到目前为止我们只学习了一种更新 UI 的方法。

我们调用 `ReactDOM.render()` 方法改变渲染输出：

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

在这一节中，我们将学习如何使 `Clock` 组件变得真正的可复用和封装。它将创建自己的定时器并且每秒更新它自己。

我们将从如何封装时钟开始：

```js{3-6,12}
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)

然而，它漏掉了一个关键的要求：`Clock` 设置定时器并每秒自动更新 UI ，应该是 `Clock` 自身实现的一部分。

理想情况下我们应该只写一次`Clock` ，它自己更新自己：

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

要实现这点，我们需要添加 "状态（state）" 到 `Clock` 组件。

状态跟属性相似，但是它是私有的并且完成由组件来控制。

我们 [之前提到](/cn/docs/components-and-props.md#函数组件和类组件) 过通过类定义组件会有一些额外的特性。局部状态恰好是类组件才有的一种特性。

## 函数组件转换成类组件

通过下面5步你可以将一个类似 `Clock` 的函数组件转换为类组件：

1. 用同样的名字通过 `React.Component` 创建一个 [ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 。

2. 添加一个名为 `render()` 的空方法。

3. 将函数的内容移动到 `render()` 方法里。

4. 在 `render()` 里面用 `this.props` 代替 `props` 。

5. 删除剩下的空函数声明。

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)

`Clock` 现在是通过类定义而不是函数。

这让我们可以使用像局部状态和生命周期钩子的额外特性。

## 添加局部状态到类

我们通过以下3步将 `date` 从属性中移动到状态中：

1) 在 `render()` 中用 `this.state.date` 替换 `this.props.date` ：

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) 添加 [类构造函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor) 来初始化 `this.state`：

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
注意我们如何将 props 传递给基础构造函数：

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

类组件应始终使用 props 调用基础构造函数。

3) 从 `<Clock />` 元素中移除 `date` 属性：

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

我们稍后再添加定时器代码到组件里面。

目前的结果如下：

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)

接下来，我们将使得 `Clock` 组件创建自己的定时器，并每秒更新它自己。

## 给类添加生命周期方法

一个具有许多组件的应用程序中，在组件被销毁时释放所占用的资源是非常重要的。

我们将在 `Clock` 第一次渲染到 DOM 后 [创建定时器](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) 。这在 React 中称为 "挂载(mounting)" 。

我们也要在 `Clock` 产生的 DOM 被移除后 [清除定时器](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) ，这在 React 中称为 "卸载(unmounting)" 。

当一个组件挂载和卸载的时候我们可以在组件类中声明一个特殊的方法来运行一些代码：

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

这些方法被称为 "生命周期钩子".

`componentDidMount()` 钩子在组件被渲染到 DOM 后运行。这是一个非常适合用来创建定时器的地方：

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

注意我们如何把定时器 ID 存储在 `this` 上。

虽然 `this.props` 由 React 自己创建，并且 `this.state` 有特殊的含义，但是如果你需要一些不用于可视化输出的内容，可以手动的去创建额外的字段到这个类。

如果在 `render()` 中没有被引用，它不应该出现在 state 中。

我们将在 `componentWillUnmount()` 生命周期钩子中取消这个定时器：

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

最后，我们将每秒运行一次 `tick()` 方法。

它将使用 `this.setState()` 周期性的更新到组件局部状态：

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/amqdNA?editors=0010)

现在时钟每秒运转一次。

让我们快速回顾一下上面的过程以及方法调用的顺序：

1) 当 `<Clock />` 传递到 `ReactDOM.render()`, React 调用 `Clock` 组件的构造函数。 由于 `Clock` 需要显示当前时间, 它通过一个包含当前时间的对象初始化 `this.state`。我们将稍后更新这个状态。

2) 然后 React 调用 `Clock` 组件的`render()` 方法。 就这样 React 得到了需要在屏幕上显示的内容。然后 React 更新 DOM 以匹配 `Clock` 的渲染输出。

3) 当 `Clock` 输出插入到了 DOM, React 调用 `componentDidMount()` 生命周期钩子，在该方法中， `Clock` 组件要求浏览器创建一个定时器每秒钟调用 `tick()` 一次。

4) 每秒钟浏览器调用 `tick()` 方法。 在该方法中,  `Clock` 组件通过调用 `setState()` 方法，并传递一个包含当前时间的对象来安排一个 UI 更新。由于调用 `setState()` , React 知道状态被改变了, 并且调用 `render()` 方法再次得到了应该显示在屏幕上的内容。这次，在 `render()` 方法中 `this.state.date` 将不一样, 因此渲染输出将包含更新的时间。于是 React 更新 DOM。

5) 如果 `Clock` 组件永远从 DOM 中移除, React 调用 `componentWillUnmount()` 生命周期钩子，于是定时器也停止了。

## 正确使用状态

关于 `setState()` 有三件事你应该要知道：

### 不要直接修改状态

例如，这样将不会重新渲染组件：

```js
// Wrong
this.state.comment = 'Hello';
```

相反要使用 `setState()`:

```js
// Correct
this.setState({comment: 'Hello'});
```

唯一可以给 `this.state` 赋值的地方是在 constructor 里面.

### 状态更新可能是异步的

React 为了优化性能可能会将多个 `setState()` 调用合并为一次更新。

因为 `this.props` 和 `this.state` 可能是异步更新，你不应该依靠他们的值来计算下一个状态（state）。

例如，下面的代码更新 counter 可能会失败：

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

要修复它，使用 `setState()` 第2种格式，接收一个函数而不是对象，这个函数将接收上一个状态作为第一个参数，并且将更新后的 props 值作为第2个参数：

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

上面我们使用了一个 [箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) , 但是它也可以使用普通函数：

```js
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### 状态更新会被合并

当你调用 `setState()`, React 将合并你提供的对象到当前状态(state)。

例如，你的状态可能包含几个独立的变量：

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

然后你可以通过不同的 `setState()` 调用独立的更新它们：

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

这是一个浅合并，所以 `this.setState({comments})` 不会修改 `this.state.posts` 的值, 但是会完全替换 `this.state.comments`.

## 数据向下流动

不管是父组件还是子组件，都不知道一个组件是否有状态，同时也不需要关心它是函数定义还是类定义。

这就是 state(状态) 经常被称为 本地状态 或 封装状态的原因。它不能被拥有并设置它的组件以外的任何组件访问。 

一个组件可以选择将 state(状态) 向下传递，作为其子组件的 props(属性)：

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

这也适用于用户自定义组件：

```js
<FormattedDate date={this.state.date} />
```

`FormattedDate` 组件通过 props 接收 `date` 的值，但它不知道该值是来自于 `Clock` 的状态（state）、属性（props）、或手动创建的:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)

这通过被称为“自上而下”或“单向”数据流。任何状态(state)始终由一些特有的组件所有，并且从该 state(状态) 导出的任何数据 或 UI 只能影响树中 “下方” 的组件。

如果把组件树想像为 props(属性) 的瀑布,所有组件的 state(状态) 就如同一个额外的水源汇入主流，且只能随着主流的方向向下流动。 

要证明所有组件都是完全独立的，我们可以创建一个 `App` 组件渲染3个 `<Clock>`:

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)

每个 `Clock` 创建它自己的定时顺并且独立更新。

在 React 应用中, 一个组件不管是有状态还是无状态，都被认为是组件的实现细节，随时都可能会改变。你可以在有状态的组件里使用无状态组件， 反之亦然。

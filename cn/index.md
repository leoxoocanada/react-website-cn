[文档](docs/hello-world.md) | [教程](tutorial/tutorial.md) | [社区](community/support.md) | [博客](_posts/2017/04/07/react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)


# React 

    一个构建用户交互界面的 JavaScript 库

[开始](docs/hello-world.md) | [获取教程](tutorial/tutorial.md)

| 声明式 | 组件化 | 仅需学习一次，编写任何平台 |
| ---- | ---- | ---- |
| React 让你轻松的创建可交互用户界面。只要为每个状态设计轻量的视图，当数据更新时， React 将有效的更新渲染对应的组件。声明式视图使你的代码更加可控，且易于调试。| 构建封装的组件，每个组件管理自己的状态，然后组合各个组件从而构建复杂的界面。由于组件的逻辑代码使用 Javascript 而不是模板，所以你能在应用中方便的传递丰富的数据，同时将状态跟 DOM 分离。| 我们不假设你需要掌握其它的技术， 所以你可以使用 React 开发新功能而不用重构旧代码。React 还可以在服务端通过 Node 渲染，并且使用 [React Native](https://facebook.github.io/react-native/) 做移动端开发。|

------

## 简单的组件

React 组件实现了一个 `render()` 方法，接收数据参数，返回展示的内容。这个示例中使用一种叫 JSX 的类似于 XML 的语法。传递给组件的数据可以在 `render()`方法中通过`this.props`读取。

**在 React 中 JSX 不是必须的** 

```
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(<HelloMessage name="Jane" />, mountNode);
```


## 有状态的组件

除了获取输入数据 (通过访问 `this.props`)，组件还可以维护内部状态数据(通过访问 `this.state`)。如果组件的状态数据发生变化，会再次调用`render()`方法，更新展示的内容。

```
class Timer extends React.Component {
  constructor(props) {
    super(props);
    this.state = {secondsElapsed: 0};
  }

  tick() {
    this.setState((prevState) => ({
      secondsElapsed: prevState.secondsElapsed + 1
    }));
  }

  componentDidMount() {
    this.interval = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.interval);
  }

  render() {
    return (
      <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
    );
  }
}

ReactDOM.render(<Timer />, mountNode);
```



## 应用程序

通过使用 `props` 和 `state`，我们可以创建一个小的 Todo 应用程序。这个示例使用 `state` 来追踪最新的列表项，以及用户最近输入的文字。尽管事件处理器看起来好像是内联的形式，不过这些事件将被收集起来，然后通过事件代理实现。

```
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.state = {items: [], text: ''};
  }

  render() {
    return (
      <div>
        <h3>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <input onChange={this.handleChange} value={this.state.text} />
          <button>{'Add #' + (this.state.items.length + 1)}</button>
        </form>
      </div>
    );
  }

  handleChange(e) {
    this.setState({text: e.target.value});
  }

  handleSubmit(e) {
    e.preventDefault();
    var newItem = {
      text: this.state.text,
      id: Date.now()
    };
    this.setState((prevState) => ({
      items: prevState.items.concat(newItem),
      text: ''
    }));
  }
}

class TodoList extends React.Component {
  render() {
    return (
      <ul>
        {this.props.items.map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
      </ul>
    );
  }
}

ReactDOM.render(<TodoApp />, mountNode);
```

## A Component Using External Plugins

React is flexible and provides hooks that allow you to interface with
other libraries and frameworks. This example uses **remarkable**, an
external Markdown library, to convert the textarea's value in real time.


```
class MarkdownEditor extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {value: 'Type some *markdown* here!'};
  }

  handleChange(e) {
    this.setState({value: e.target.value});
  }

  getRawMarkup() {
    var md = new Remarkable();
    return { __html: md.render(this.state.value) };
  }

  render() {
    return (
      <div className="MarkdownEditor">
        <h3>Input</h3>
        <textarea
          onChange={this.handleChange}
          defaultValue={this.state.value} />
        <h3>Output</h3>
        <div
          className="content"
          dangerouslySetInnerHTML={this.getRawMarkup()}
        />
      </div>
    );
  }
}

ReactDOM.render(<MarkdownEditor />, mountNode);
```

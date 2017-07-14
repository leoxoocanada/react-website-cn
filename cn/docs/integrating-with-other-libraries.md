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
* [**`与其它类库集成`**](/cn/docs/integrating-with-other-libraries.md)

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

# 与其它类库集成

React 可以在任何web应用中使用。它能嵌入其它的应用，并且只要小心一点，其它的应用也能嵌入到 React。本指南将检查一些更常见的用例，重点是 [jQuery](https://jquery.com/) 和 [Backbone](http://backbonejs.org/) 的集成，但是同样的想法也能应用于将组件和任何现有代码集成。

## 与 DOM 操作插件集成

React 不知道在 React 以外对 DOM 的修改。它决定更新是基于它自己的内部的表现，并且如果同样的 DOM 节点被其它库操作，React 会感到困惑并且没有办法恢复。

这并不意味着将React与影响DOM的其他方式相结合是不可能的，甚至是一定难以理解的，你只需要注意每个人在做什么。

避免冲突最简单的方式是防止 React 组件更新。您可以通过渲染React的元素来完成此操作，无需更新，像一个空的  `<div />`.

### 如何解决问题

为了演示这个，我们绘制一个包裹通用 jQuery 插件的容器。

我们将附加一个[ref](/cn/docs/refs-and-the-dom.md) 到根 DOM 节点。在 `componentDidMount` 里，我们将获取一个到它的引用，以便我们能将它传递到 jQuery 插件。

为了防止 React 在挂载之后接触 DOM，我们将从 `render()` 方法中返回一个空的 `<div />` 。这个 `<div />` 元素没有属性或子元素，因此 React 无须更新它，让 jQuery 插件可以自由的管理 DOM 的一部分：

```js{3,4,8,12}
class SomePlugin extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.somePlugin();
  }

  componentWillUnmount() {
    this.$el.somePlugin('destroy');
  }

  render() {
    return <div ref={el => this.el = el} />;
  }
}
```

注意我们定义了 `componentDidMount` 和 `componentWillUnmount` 两个[生命周期钩子](/cn/docs/react-component.md#the-component-lifecycle). 一些 jQuery 插件附加 DOM 事件监听，因此在 `componentWillUnmount` 中分离它们是非常重要的。如果插件没有指定清除的方法，你或许不得不自己提供，记住移除插件注册的任何事件监听防止内存泄漏。

### 与 jQuery 选择插件集成

对于这些概念的一个更具体的例子，让我们为插件[Chosen]（https://harvesthq.github.io/chosen/）写一个最小的包装，这增加了`<select>`的输入。

>**注意:**
>
>只是因为这是可能的，并不意味着它是React应用程序的最佳方法。如果可以的话，我们鼓励你使用React组件。 React 组件在 React 应用程序中更容易重用，并且通常可以更好地控制其行为和外观。

首先，让我们看看 Chosen 对 DOM 做了什么

如果你在 `<select>` DOM节点上调用它，它读取原始 DOM 节点的属性，通过一个内联样式隐藏它，然后附加一个带有自身视觉表现的独立 DOM 节点在 `<select>` 后面。然后它触发 jQuery 事件去通知我们有关变动。

假设这是我们正在使用的 `<Chosen>` 包装器 React 组件的API。

```js
function Example() {
  return (
    <Chosen onChange={value => console.log(value)}>
      <option>vanilla</option>
      <option>chocolate</option>
      <option>strawberry</option>
    </Chosen>
  );
}
```

为简单起见，我们将作为一个 [非受控组件](/react/docs/uncontrolled-components.html) 来实现。

首先，我们将创建一个带有 `render()` 方法的空组件，它返回一个包裹在 `<div>` 里的`<select>`：

```js{4,5}
class Chosen extends React.Component {
  render() {
    return (
      <div>
        <select className="Chosen-select" ref={el => this.el = el}>
          {this.props.children}
        </select>
      </div>
    );
  }
}
```

注意我们如何将 `<select>` 包裹到一个额外的 `<div>`，这是必要的，因为 Chosen 将在传递给它的 `<select>` 节点后面附加另外的 DOM 元素。然而，就 React 而言，`<div>` 经常只有单个子节点。这是我们如何确保 React 更新将不会和额外的 DOM 节点冲突。重要的是，如果你在 React 外部修改 DOM 时，你必须确保 React 没有理由接触到 DOM 节点。

接下来，我们将实现生命周期钩子。我们需要使用'componentDidMount`中的`<select>`节点的引用来初始化Chosen，并且在 `componentWillUnmount` 里销毁它:

```js{2,3,7}
componentDidMount() {
  this.$el = $(this.el);
  this.$el.chosen();
}

componentWillUnmount() {
  this.$el.chosen('destroy');
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/qmqeQx?editors=0010)

注意 React 对 `this.el` 字段没有特别的含义。它只是可以工作，因为我们在 `render()` 方法里已经预先把 `ref` 赋值给了这个字段；

```js
<select className="Chosen-select" ref={el => this.el = el}>
```

这足够让我们的组件渲染，但是我希望得到关于值变化的通知。为此，我们将在通过 Chosen 管理的 `<select>` 上订阅 jQuery `change` 事件。

我们将不会直接传递 `this.props.onChange` 给 Chosen，因为组件的属性（props）可能随时会改变，并且包含事件处理器。相反，我们将声明一个 `handleChange()` 方法去调用 `this.props.onChange`，并且将其订阅到 jQuery `change` 事件：

```js{5,6,10,14-16}
componentDidMount() {
  this.$el = $(this.el);
  this.$el.chosen();

  this.handleChange = this.handleChange.bind(this);
  this.$el.on('change', this.handleChange);
}

componentWillUnmount() {
  this.$el.off('change', this.handleChange);
  this.$el.chosen('destroy');
}

handleChange(e) {
  this.props.onChange(e.target.value);
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/bWgbeE?editors=0010)

最后，还有一件事情要做，在 React 里，属性（props）随时会变化。例如，假如父组件的状态（state）变化时， `<Chosen>` 组件能获取不同的子节点。这意味着在集成点上，重要的是我们手动更新 DOM 在响应更新，直到我们不再让 React 为我们管理 DOM。

Chosen 文档建议我们能使用 jQuery `trigger()` API 来通知它有关原始 DOM 元素的变动。我们将让 React 注意更新 `<select>` 内部的 `this.props.children` ，但是我们也将添加一个 `componentDidUpdate()` 生命周期钩子来通知 Chosen 关于子节点列表的变化：

```js{2,3}
componentDidUpdate(prevProps) {
  if (prevProps.children !== this.props.children) {
    this.$el.trigger("chosen:updated");
  }
}
```

这样，当 React 更改管理的 `<select>` 子节点时，Chosen 将知道更新它的 DOM 元素。

`Chosen` 组件完整的实现看起来像这样：

```js
class Chosen extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.chosen();

    this.handleChange = this.handleChange.bind(this);
    this.$el.on('change', this.handleChange);
  }

  componentDidUpdate(prevProps) {
    if (prevProps.children !== this.props.children) {
      this.$el.trigger("chosen:updated");
    }
  }

  componentWillUnmount() {
    this.$el.off('change', this.handleChange);
    this.$el.chosen('destroy');
  }

  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    return (
      <div>
        <select className="Chosen-select" ref={el => this.el = el}>
          {this.props.children}
        </select>
      </div>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/xdgKOz?editors=0010)

## Integrating with Other View Libraries

React can be embedded into other applications thanks to the flexibility of [`ReactDOM.render()`](/react/docs/react-dom.html#render).

Although React is commonly used at startup to load a single root React component into the DOM, `ReactDOM.render()` can also be called multiple times for independent parts of the UI which can be as small as a button, or as large as an app.

In fact, this is exactly how React is used at Facebook. This lets us write applications in React piece by piece, and combine it with our existing server-generated templates and other client-side code.

### Replacing String-Based Rendering with React

A common pattern in older web applications is to describe chunks of the DOM as a string and insert it into the DOM like so: `$el.html(htmlString)`. These points in a codebase are perfect for introducing React. Just rewrite the string based rendering as a React component.

So the following jQuery implementation...

```js
$('#container').html('<button id="btn">Say Hello</button>');
$('#btn').click(function() {
  alert('Hello!');
});
```

...could be rewritten using a React component:

```js
function Button() {
  return <button id="btn">Say Hello</button>;
}

ReactDOM.render(
  <Button />,
  document.getElementById('container'),
  function() {
    $('#btn').click(function() {
      alert('Hello!');
    });
  }
);
```

From here you could start moving more logic into the component and begin adopting more common React practices. For example, in components it is best not to rely on IDs because the same component can be rendered multiple times. Instead, we will use the [React event system](/react/docs/handling-events.html) and register the click handler directly on the React `<button>` element:

```js{2,6,9}
function Button(props) {
  return <button onClick={props.onClick}>Say Hello</button>;
}

function HelloButton() {
  function handleClick() {
    alert('Hello!');
  }
  return <Button onClick={handleClick} />;
}

ReactDOM.render(
  <HelloButton />,
  document.getElementById('container')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/RVKbvW?editors=1010)

You can have as many such isolated components as you like, and use `ReactDOM.render()` to render them to different DOM containers. Gradually, as you convert more of your app to React, you will be able to combine them into larger components, and move some of the `ReactDOM.render()` calls up the hierarchy.

### Embedding React in a Backbone View

[Backbone](http://backbonejs.org/) views typically use HTML strings, or string-producing template functions, to create the content for their DOM elements. This process, too, can be replaced with rendering a React component.

Below, we will create a Backbone view called `ParagraphView`. It will override Backbone's `render()` function to render a React `<Paragraph>` component into the DOM element provided by Backbone (`this.el`). Here, too, we are using [`ReactDOM.render()`](/react/docs/react-dom.html#render):

```js{1,5,8,12}
function Paragraph(props) {
  return <p>{props.text}</p>;
}

const ParagraphView = Backbone.View.extend({
  render() {
    const text = this.model.get('text');
    ReactDOM.render(<Paragraph text={text} />, this.el);
    return this;
  },
  remove() {
    ReactDOM.unmountComponentAtNode(this.el);
    Backbone.View.prototype.remove.call(this);
  }
});
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/gWgOYL?editors=0010)

It is important that we also call `ReactDOM.unmountComponentAtNode()` in the `remove` method so that React unregisters event handlers and other resources associated with the component tree when it is detached.

When a component is removed *from within* a React tree, the cleanup is performed automatically, but because we are removing the entire tree by hand, we must call it this method.

## Integrating with Model Layers

While it is generally recommended to use unidirectional data flow such as [React state](/react/docs/lifting-state-up.html), [Flux](http://facebook.github.io/flux/), or [Redux](http://redux.js.org/), React components can use a model layer from other frameworks and libraries.

### Using Backbone Models in React Components

The simplest way to consume [Backbone](http://backbonejs.org/) models and collections from a React component is to listen to the various change events and manually force an update.

Components responsible for rendering models would listen to `'change'` events, while components responsible for rendering collections would listen for `'add'` and `'remove'` events. In both cases, call [`this.forceUpdate()`](/react/docs/react-component.html#forceupdate) to rerender the component with the new data.

In the example below, the `List` component renders a Backbone collection, using the `Item` component to render individual items.

```js{1,7-9,12,16,24,30-32,35,39,46}
class Item extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.model.on('change', this.handleChange);
  }

  componentWillUnmount() {
    this.props.model.off('change', this.handleChange);
  }

  render() {
    return <li>{this.props.model.get('text')}</li>;
  }
}

class List extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange();
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.collection.on('add', 'remove', this.handleChange);
  }

  componentWillUnmount() {
    this.props.collection.off('add', 'remove', this.handleChange);
  }

  render() {
    return (
      <ul>
        {this.props.collection.map(model => (
          <Item key={model.cid} model={model} />
        ))}
      </ul>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/GmrREm?editors=0010)

### Extracting Data from Backbone Models

The approach above requires your React components to be aware of the Backbone models and collections. If you later plan to migrate to another data management solution, you might want to concentrate the knowledge about Backbone in as few parts of the code as possible.

One solution to this is to extract the model's attributes as plain data whenever it changes, and keep this logic in a single place. The following is [a higher-order component](/react/docs/higher-order-components.html) that extracts all attributes of a Backbone model into state, passing the data to the wrapped component.

This way, only the higher-order component needs to know about Backbone model internals, and most components in the app can stay agnostic of Backbone.

In the example below, we will make a copy of the model's attributes to form the initial state. We subscribe to the `change` event (and unsubscribe on unmounting), and when it happens, we update the state with the model's current attributes. Finally, we make sure that if the `model` prop itself changes, we don't forget to unsubscribe from the old model, and subscribe to the new one.

Note that this example is not meant to be exhaustive with regards to working with Backbone, but it should give you an idea for how to approach this in a generic way:

```js{1,5,10,14,16,17,22,26,32}
function connectToBackboneModel(WrappedComponent) {
  return class BackboneComponent extends React.Component {
    constructor(props) {
      super(props);
      this.state = Object.assign({}, props.model.attributes);
      this.handleChange = this.handleChange.bind(this);
    }

    componentDidMount() {
      this.props.model.on('change', this.handleChange);
    }

    componentWillReceiveProps(nextProps) {
      this.setState(Object.assign({}, nextProps.model.attributes));
      if (nextProps.model !== this.props.model) {
        this.props.model.off('change', this.handleChange);
        nextProps.model.on('change', this.handleChange);
      }
    }

    componentWillUnmount() {
      this.props.model.off('change', this.handleChange);
    }

    handleChange(model) {
      this.setState(model.changedAttributes());
    }

    render() {
      const propsExceptModel = Object.assign({}, this.props);
      delete propsExceptModel.model;
      return <WrappedComponent {...propsExceptModel} {...this.state} />;
    }
  }
}
```

To demonstrate how to use it, we will connect a `NameInput` React component to a Backbone model, and update its `firstName` attribute every time the input changes:

```js{4,6,11,15,19-21}
function NameInput(props) {
  return (
    <p>
      <input value={props.firstName} onChange={props.handleChange} />
      <br />
      My name is {props.firstName}.
    </p>
  );
}

const BackboneNameInput = connectToBackboneModel(NameInput);

function Example(props) {
  function handleChange(e) {
    model.set('firstName', e.target.value);
  }

  return (
    <BackboneNameInput
      model={props.model}
      handleChange={handleChange}
    />
  );
}

const model = new Backbone.Model({ firstName: 'Frodo' });
ReactDOM.render(
  <Example model={model} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/PmWwwa?editors=0010)

This technique is not limited to Backbone. You can use React with any model library by subscribing to its changes in the lifecycle hooks and, optionally, copying the data into the local React state.

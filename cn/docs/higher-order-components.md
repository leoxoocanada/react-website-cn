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
* [**`高阶组件`**](/cn/docs/higher-order-components.md)
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

# 高阶组件(Higher-Order Components)

高阶组件(HOC)是 React 中可复用组件逻辑的高级技术。HOCs 不是 React API 的一部分。它源自 React 生态体系。

具体地来说，**高阶组件是一个函数，能够接收一个组件并且返回一个组件**

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

组件是将属性转换成 UI，而高阶组件是将一个组件转换成另一个组件。

HOCs 在第三方 React 库中比较常见，像 Redux's [`connect`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options) 和 Relay's [`createContainer`](https://facebook.github.io/relay/docs/api-reference-relay.html#createcontainer-static-method).

在这篇文档里，我们将讨论为什么高阶组件是有用的，并且如何编写你自己的高阶组件。

## 在横切关注点中使用高阶组件

> **注意**
>
> 我们以前推荐 mixins 作为处理横切关注点的一种方式。我们已经意识到 mixins 相比它创造的价值会带来更多麻烦。[阅读更多](/cn/blog/2016/07/13/mixins-considered-harmful.md) 关于为什么我们已经放弃了 mixins，并且你如何过渡你目前的组件。

组件是 React 中代码复用的主要单位。然而，你将发现某些模式不是简单适用于传统的组件。

例如，假设你有一个`CommentList` 组件可以订阅一个外部的数据源，用来渲染一个评论列表：

```js
class CommentList extends React.Component {
  constructor() {
    super();
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" 是一些全局数据源
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // 更改订阅
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // 清除监听
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // 当数据源变化时更新组件
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```

之后，你写了一个组件用于订阅一个简单的博客贴子，它类似于下面的模型：

```js
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

`CommentList` 和 `BlogPost` 不是完全相同的，它们调用`DataSource`上不同的方法, 并且它们渲染不同的输出。但它们大多数实现是相同的：

- 在挂载阶段，添加一个变化监听到 `DataSource`.
- 在监听内部，每当数据源变化时调用 `setState` .
- 在卸载阶段，移除变化监听.

你能想像一下在大型应用中，这种订阅 `DataSource` 和调用 `setState` 的模式将反复不断地发生。我们需要一个抽象允许我们在一个地方去定义这个逻辑，并且将它们分享到更多组件。这是高阶组件擅长的地方。

我们能写一个创建组件的函数，像`CommentList` 和 `BlogPost` ，在那订阅 `DataSource`。该函数将接受作为其参数之一的子组件，该子组件接收订阅的数据作为属性。让我们叫这个函数为 `withSubscription`：

```js
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
});
```

第一个参数是容器组件。第二个参数检索我们感兴趣的数据，给出一个 `DataSource` 和当前的属性。

当 `CommentListWithSubscription` 和 `BlogPostWithSubscription` 被渲染时， `CommentList` 和 `BlogPost` 将被传递一个 `data` 属性，并从 `DataSource` 检索最新的数据：

```js
// 此函数需要一个组件...
function withSubscription(WrappedComponent, selectData) {
  // ...并且返回另外一个组件...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... 它负责订阅 ...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... 从最新的数据渲染容器组件!
      // 注意我们传递一些附加的属性
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

请注意， HOC 不会修改输入组件，也不使用继承来拷贝它的行为。相反，HOC 通过 *包裹* 在一个容器组件里来 *组成* 原始组件。HOC 是一个没有副作用的纯函数。

就是这样！容器组件接收所有的容器属性，以及一个新的属性 `data`，它用于渲染输出。HOC 不关心如何或为什么数据被使用，容器组件不关心数据从哪里来。

因为 `withSubscription` 是一个普通的函数，所以你能添加任意数据的参数。例如，你可能希望可以配置 `data` 的名字，以进一步将 HOC 和 容器组件隔离。或你可能接受配置 `shouldComponentUpdate` 参数，或配置数据源的参数。这些都是可能，因为 HOC 完全控制组件的定义。

与组件一样， `withSubscription` 和容器组件之间的联系完全是基于属性的。 这样可以方便地将一个HOC替换为不同的HOC，只要它们为容器组件提供相同的属性。如果更改数据获取库，这可能很有用，例如。

## 不要改变原始组件，而是使用组合

抵制在HOC内部修改组件原型（或以其他方式修改）的诱惑。

```js
function logProps(InputComponent) {
  InputComponent.prototype.componentWillReceiveProps(nextProps) {
    console.log('Current props: ', this.props);
    console.log('Next props: ', nextProps);
  }
  // 事实上，我们返回的原始输入意味着它已经被修改
  return InputComponent;
}

// EnhancedComponent 将在接收到属性时记录
const EnhancedComponent = logProps(InputComponent);
```

这会有一些问题。一是输入组件不能与 enhanced 组件分开复用。更关键的是，如果你应用其它 HOC 到 `EnhancedComponent` ，那么*也*将修改 `componentWillReceiveProps`， 第一个 HOC 的功能将被覆盖！ 这个HOC也不适用于没有生命周期方法的功能组件。

具有修改功能的 HOC 是一种有漏洞的抽象 - 用户必须知道如何实施它们，以避免与其他HOC的冲突。

相比较修改，HOCs 应该使用组合，通过包裹输入组件在一个容器组件里：

```js
function logProps(WrappedComponent) {
  return class extends React.Component {
    componentWillReceiveProps(nextProps) {
      console.log('Current props: ', this.props);
      console.log('Next props: ', nextProps);
    }
    render() {
      // 包裹输入组件在一个容器组件里, 没有修改它，非常好!
      return <WrappedComponent {...this.props} />;
    }
  }
}
```

这个 HOC 有跟修改版本同样的功能，同时避免了潜在的冲突。它在 class 和 functional 组件里同样运行的非常好。并且因为它是纯函数，它可以和其它 HOCs 甚至它自己组合。

你可能已经注意到 HOCs 和 **容器组件**模式 有相似之处。容器组件是将责任分为高级和低级关注点策略的一部分。容器管理类似于订阅和状态的东西，传递属性到处理类似于渲染UI之类的组件。HOCs 使用容器作为它们实现的一部分。你可以将 HOCs 作为参数容器组件来定义。

## Convention: Pass Unrelated Props Through to the Wrapped Component

HOCs add features to a component. They shouldn't drastically alter its contract. It's expected that the component returned from an HOC has a similar interface to the wrapped component.

HOCs should pass through props that are unrelated to its specific concern. Most HOCs contain a render method that looks something like this:

```js
render() {
  // Filter out extra props that are specific to this HOC and shouldn't be
  // passed through
  const { extraProp, ...passThroughProps } = this.props;

  // Inject props into the wrapped component. These are usually state values or
  // instance methods.
  const injectedProp = someStateOrInstanceMethod;

  // Pass props to wrapped component
  return (
    <WrappedComponent
      injectedProp={injectedProp}
      {...passThroughProps}
    />
  );
}
```

This convention helps ensure that HOCs are as flexible and reusable as possible.

## Convention: Maximizing Composability

Not all HOCs look the same. Sometimes they accept only a single argument, the wrapped component:

```js
const NavbarWithRouter = withRouter(Navbar);
```

Usually, HOCs accept additional arguments. In this example from Relay, a config object is used to specify a component's data dependencies:

```js
const CommentWithRelay = Relay.createContainer(Comment, config);
```

The most common signature for HOCs looks like this:

```js
// React Redux's `connect`
const ConnectedComment = connect(commentSelector, commentActions)(Comment);
```

*What?!* If you break it apart, it's easier to see what's going on.

```js
// connect is a function that returns another function
const enhance = connect(commentListSelector, commentListActions);
// The returned function is an HOC, which returns a component that is connected
// to the Redux store
const ConnectedComment = enhance(CommentList);
```
In other words, `connect` is a higher-order function that returns a higher-order component!

This form may seem confusing or unnecessary, but it has a useful property. Single-argument HOCs like the one returned by the `connect` function have the signature `Component => Component`. Functions whose output type is the same as its input type are really easy to compose together.

```js
// Instead of doing this...
const EnhancedComponent = connect(commentSelector)(withRouter(WrappedComponent))

// ... you can use a function composition utility
// compose(f, g, h) is the same as (...args) => f(g(h(...args)))
const enhance = compose(
  // These are both single-argument HOCs
  connect(commentSelector),
  withRouter
)
const EnhancedComponent = enhance(WrappedComponent)
```

(This same property also allows `connect` and other enhancer-style HOCs to be used as decorators, an experimental JavaScript proposal.)

The `compose` utility function is provided by many third-party libraries including lodash (as [`lodash.flowRight`](https://lodash.com/docs/#flowRight)), [Redux](http://redux.js.org/docs/api/compose.html), and [Ramda](http://ramdajs.com/docs/#compose).

## Convention: Wrap the Display Name for Easy Debugging

The container components created by HOCs show up in the [React Developer Tools](https://github.com/facebook/react-devtools) like any other component. To ease debugging, choose a display name that communicates that it's the result of an HOC.

The most common technique is to wrap the display name of the wrapped component. So if your higher-order component is named `withSubscription`, and the wrapped component's display name is `CommentList`, use the display name `WithSubscription(CommentList)`:

```js
function withSubscription(WrappedComponent) {
  class WithSubscription extends React.Component {/* ... */}
  WithSubscription.displayName = `WithSubscription(${getDisplayName(WrappedComponent)})`;
  return WithSubscription;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
```


## Caveats

Higher-order components come with a few caveats that aren't immediately obvious if you're new to React.

### Don't Use HOCs Inside the render Method

React's diffing algorithm (called reconciliation) uses component identity to determine whether it should update the existing subtree or throw it away and mount a new one. If the component returned from `render` is identical (`===`) to the component from the previous render, React recursively updates the subtree by diffing it with the new one. If they're not equal, the previous subtree is unmounted completely.

Normally, you shouldn't need to think about this. But it matters for HOCs because it means you can't apply an HOC to a component within the render method of a component:

```js
render() {
  // A new version of EnhancedComponent is created on every render
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // That causes the entire subtree to unmount/remount each time!
  return <EnhancedComponent />;
}
```

The problem here isn't just about performance — remounting a component causes the state of that component and all of its children to be lost.

Instead, apply HOCs outside the component definition so that the resulting component is created only once. Then, its identity will be consistent across renders. This is usually what you want, anyway.

In those rare cases where you need to apply an HOC dynamically, you can also do it inside a component's lifecycle methods or its constructor.

### Static Methods Must Be Copied Over

Sometimes it's useful to define a static method on a React component. For example, Relay containers expose a static method `getFragment` to facilitate the composition of GraphQL fragments.

When you apply an HOC to a component, though, the original component is wrapped with a container component. That means the new component does not have any of the static methods of the original component.

```js
// Define a static method
WrappedComponent.staticMethod = function() {/*...*/}
// Now apply an HOC
const EnhancedComponent = enhance(WrappedComponent);

// The enhanced component has no static method
typeof EnhancedComponent.staticMethod === 'undefined' // true
```

To solve this, you could copy the methods onto the container before returning it:

```js
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  // Must know exactly which method(s) to copy :(
  Enhance.staticMethod = WrappedComponent.staticMethod;
  return Enhance;
}
```

However, this requires you to know exactly which methods need to be copied. You can use [hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics) to automatically copy all non-React static methods:

```js
import hoistNonReactStatic from 'hoist-non-react-statics';
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  hoistNonReactStatic(Enhance, WrappedComponent);
  return Enhance;
}
```

Another possible solution is to export the static method separately from the component itself.

```js
// Instead of...
MyComponent.someFunction = someFunction;
export default MyComponent;

// ...export the method separately...
export { someFunction };

// ...and in the consuming module, import both
import MyComponent, { someFunction } from './MyComponent.js';
```

### Refs Aren't Passed Through

While the convention for higher-order components is to pass through all props to the wrapped component, it's not possible to pass through refs. That's because `ref` is not really a prop — like `key`, it's handled specially by React. If you add a ref to an element whose component is the result of an HOC, the ref refers to an instance of the outermost container component, not the wrapped component.

If you find yourself facing this problem, the ideal solution is to figure out how to avoid using `ref` at all. Occasionally, users who are new to the React paradigm rely on refs in situations where a prop would work better.

That said, there are times when refs are a necessary escape hatch — React wouldn't support them otherwise. Focusing an input field is an example where you may want imperative control of a component. In that case, one solution is to pass a ref callback as a normal prop, by giving it a different name:

```js
function Field({ inputRef, ...rest }) {
  return <input ref={inputRef} {...rest} />;
}

// Wrap Field in a higher-order component
const EnhancedField = enhance(Field);

// Inside a class component's render method...
<EnhancedField
  inputRef={(inputEl) => {
    // This callback gets passed through as a regular prop
    this.inputEl = inputEl
  }}
/>

// Now you can call imperative methods
this.inputEl.focus();
```

This is not a perfect solution by any means. We prefer that refs remain a library concern, rather than require you to manually handle them. We are exploring ways to solve this problem so that using an HOC is unobservable.

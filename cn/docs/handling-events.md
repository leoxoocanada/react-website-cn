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
* [State和生命周期](/cn/docs/state-and-lifecycle.md)
* [**`事件处理`**](/cn/docs/handling-events.md)
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

# 事件处理

在 React 中处理事件跟在 DOM 元素中处理事件是类似的。这里有一些语法上的差异：

* React 事件名使用驼峰写法而不是小写。
* 在 JSX 中你传递的是一个函数而不是字符串。

例如，看下面这个 HTML:

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

在 React 中略微有一些不同:

```js{1}
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

其它不同的地方在于在 React 中你不能返回 `false` 阻止默认行为。你必须明确调用 `preventDefault` . 例如, 在一般的 HTML 中, 要阻止默认的链接行为打开一个新页面，你可以这么写：

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

在 React 中, 你应该这么写:

```js{2-5,8}
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

在这里, `e` 是一个合成事件. React 按照 [W3C 规则](https://www.w3.org/TR/DOM-Level-3-Events/) 定义他们的合成事件, 所以你不需要担心它的跨浏览器兼容性. 查阅 [`合成事件(SyntheticEvent)`](/cn/docs/events.md) 参考指南了解更多.

当你使用 React 的时候，通过不需要在 DOM 元素被创建后调用 `addEventListener` 来添加事件监听器. 相反, 只要在元素初始渲染时提供一个监听器就可以.

当你使用 [ES6 类（class）](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 来定义一个组件的时候, 通常的一个事件处理程序是类上的一个方法. 例如, 这个 `Toggle` 组件渲染一个按钮让用户在 "ON" 和 "OFF" 状态之间切换:

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 这个绑定是必须的，用来使 `this` 在回调函数中起作用
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

在 JSX 回调中你必须注意 `this` 的指向. 在 JavaScript 中, 类（class）方法默认 [绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) . 如果你忘了绑定 `this.handleClick` 并将它传递给了 `onClick`, 当这个函数实际调用的时候 `this` 将会是 `undefined` .

这不是 React 特有的行为; 这是 [在 JavaScript 中函数如何工作](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/) 的一部分. 通常来说, 如果你引用一个后面没有跟 `()` 的方法, 比如 `onClick={this.handleClick}`, 你应该绑定该方法.

如果调用 `bind` 让你很烦恼, 有2个方法可以解决这个问题. 如果你使用实验性的 [属性初始化语法](https://babeljs.io/docs/plugins/transform-class-properties/), 你能使用属性初始化来正确的绑定（bind）回调:

```js{2-6}
class LoggingButton extends React.Component {
  // 这个语法保证 `this` 绑定在 handleClick 中.
  // 警告: 这是 *实验性* 语法.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

这个语法在 [Create React App](https://github.com/facebookincubator/create-react-app) 中默认开启.

如果你不使用属性初始化语法，你可以在函数中使用一个 [箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) :

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 这个语法确保 `this` 绑定在 handleClick 中
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

这个语法的问题是每次 `LoggingButton` 渲染的时候都会创建一个不同的回调。多数情况下, 这是没有问题的. 然而, 如果这个回调是作为属性(prop)传递到下级组件，这些组件可能需要额外的重复渲染. 我们通常建议在 constructor  中绑定或使用属性初始化语法，以避开这种性能问题。

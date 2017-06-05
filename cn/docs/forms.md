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
* [事件处理](/cn/docs/handling-events.md)
* [条件渲染](/cn/docs/conditional-rendering.md)
* [列表和键](/cn/docs/lists-and-keys.md)
* [**`表单`**](/cn/docs/forms.md)
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

# 表单

HTML 表单元素与 React 中其它 DOM 元素有一些不一样，因为表单元素天生带有一些内部状态。例如，这个纯 HTML 表单接受一个单独的 name:

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

这个表单有默认的 HTML 表单行为，当用户提交这个表单时它会打开一个新页面。如果你希望在 React 中保持这个行为，它也可以工作。但是在多数情况下，用一个处理表单提交并访问用户输入到表单中的数据的 Javascript 函数来是更方便的。实现这一点的标准方法是使用一种称为“受控组件(controlled components)”的技术。

## 受控组件

在 HTML 中，表单元素如`<input>`, `<textarea>`,  `<select>` 通常保持它们自己的状态，并根据用户输入进行更新。在 React 中，可变状态通常保持在组件状态（state ）属性里，并且只能通过 [`setState()`](/cn/docs/react-component.md#setstate) 来更新。

我们能通过使 React 状态变成 “单一数据源原则” 将2者结合起来。然后渲染表单的组件也能控制随后用户的输入。一个输入表单元素的值被 React 控制的方式被称为 “受控组件(controlled components)”。

例如，如果我要让上一个例子当它提交的时候打印名字，我们可以将表单写为受控组件：

```javascript{4,10-12,24}
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

设置表单元素的 `value` 属性之后，显示的值总是 `this.state.value`，使 React 状态满足同一数据理念， `handleChange` 在每次键盘输入之后运行用来更新 React 状态，显示的值将作为用户的输入更新。

在受控组件里，每个状态的变化将有一个相关的控制函数。这使得修改或验证用户输入变得简单。例如，如果我们要强制写入的名字都使用大写字母，我们可以这么写 `handleChange` ：

```javascript{2}
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

## textarea 标签

在 HTML 里，`<textarea>` 元素通过它的子节点定义文本：

```html
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

在 React 里， `<textarea>` 使用一个 `value` 属性替代。这种方式，使用 `<textarea>` 写的表单非常类似于使用单行 input 的表单：

```javascript{4-6,12-14,26}
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

请注意 `this.state.value` 是在 constructor 里被初始化，所以这些文本一开始就出现在文本区域中。

## select 标签

在 HTML 里，`<select>` 创建一个下拉列表。例如，这段 HTML 创建了一个口味（flavors）下拉列表：

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

注意 Coconut 选项是初始选中项，因为它有 `selected` 标签。在 React 中，通过在 `select` 根标签中使用 `value` 属性来替代使用 `selected` 标签。这在受控组件中更方便，因为你只需要在一个地方更新它。例如：

```javascript{4,10-12,24}
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

总的来说，这使`<input type="text">`, `<textarea>`, 和 `<select>` 都以类似的方式工作，他们都接受一个 `value` 属性用来实现受控组件。

## 处理多个输出元素

When you need to handle multiple controlled `input` elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

For example:

```javascript{15,18,28,37}
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

Note how we used the ES6 [computed property name](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) syntax to update the state key corresponding to the given input name:

```js{2}
this.setState({
  [name]: value
});
```

It is equivalent to this ES5 code:

```js{2}
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

Also, since `setState()` automatically [merges a partial state into the current state](/cn/docs/state-and-lifecycle.md#state-updates-are-merged), we only needed to call it with the changed parts.

## Alternatives to Controlled Components

It can sometimes be tedious to use controlled components, because you need to write an event handler for every way your data can change and pipe all of the input state through a React component. This can become particularly annoying when you are converting a preexisting codebase to React, or integrating a React application with a non-React library. In these situations, you might want to check out [uncontrolled components](/dn/docs/uncontrolled-components.md), an alternative technique for implementing input forms.

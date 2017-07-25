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
* [**`状态提升`**](/cn/docs/lifting-state-up.md)
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

# 状态提升

通常情况下，相同的数据变化需要在几个组件中体现。我们建议提升共享状态到他们最近的通用祖先组件里。让我们看看这是如何运作的。

在这个章节中，我们将创建一个温度计算器，给定一个温度计算水是否会沸腾。

我们将通过一个叫 `BoilingVerdict` 的组件开始。它接受 `celsius（摄氏温度）` 作为属性，并且打印是否足够让水沸腾；

```js{3,5}
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

下一步，我们将创建一个叫 `Calculator` 的组件。它返回一个 `<input>` 让你输入一个温度，并且在 `this.state.temperature` 中保存它的值。

此外，它根据当前的输入值渲染 `BoilingVerdict`  。

```js{5,9,13,17-21}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/valscion/pen/VpZJRZ?editors=0010)

## 添加第2个输入

我们的新需求是，除了一个 Celsius（摄氏温度） 输入框外，我们再提供一个 Fahrenheit（华氏温度） 输入框，并且两者保持同步。

我们可以从 `Calculator` 中提取一个 `TemperatureInput` 组件开始。我们将给它添加一个`scale` 属性，值可能是  `"c"` 或 `"f"`:

```js{1-4,19,22}
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

我们现在可以修改 `Calculator` 来渲染2个独立的温度输入：

```js{5,6}
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/valscion/pen/GWKbao?editors=0010)

现在我们有2个输入框，但是当你在它们中的任何一个中输入温度时，另外一个不会更新。这不。符合我们的需求：我们要保持两者的同步

我们也不能在 `Calculator` 中显示 `BoilingVerdict` 。 `Calculator` 不知道当前的温度，因为它是隐藏在 `TemperatureInput` 中。

## 编写转换函数

首先，我们将写2个函数用来将 Celsius（摄氏温度）和 Fahrenheit（华氏温度）互相转换：

```js
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

这两个函数用来转换数字。我们将写另外一个函数用来接收一个字符串 `temperature` 和一个转换函数作为参数并返回一个字符串。这个函数用来在两个输入之间进行相互转换。

对于无效的 `temperature` 值，它返回一个空字符串，输出结果保留3位小数：

```js
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

例如，`tryConvert('abc', toCelsius)` 返回一个空字符串，`tryConvert('10.22', toFahrenheit)` 返回`'50.396'`.

## 状态提升(Lifting State Up)

当前，2个 `TemperatureInput` 组件各自在本地状态中保存它们的值：

```js{5,9,13}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
```

然而，我们希望这两个输入是相互同步的。当我们更新 Celsius 输入时，Fahrenheit 输入应显示转换后的温度，反之亦然。

在 React 中，共享状态是通过移动它到最接近它的共同祖先组件来实现的。这叫做 状态提升(Lifting State Up)。我们将从 `TemperatureInput` 中删除本地状态，并且把它移动到 `Calculator` 里。

如果 `Calculator` 拥有共享状态，它将变成当前两个温度输入框的“单一数据来源”。它能指示它们始终都拥有一样的值。由于两个 `TemperatureInput` 组件的属性都是来自同一个父级 `Calculator` 组件，这两个输入框将始终保持同步。

让我们一步步来看看它是如何运行的。

首先，我们将 `TemperatureInput` 组件中的 `this.state.temperature` 用 `this.props.temperature` 替换。目前，让我们假装 `this.props.temperature` 已经存在，虽然将来我们将需要从 `Calculator` 中传递过来：

```js{3}
  render() {
    // 之前是：const temperature = this.state.temperature;
    const temperature = this.props.temperature;
```

我们知道 [属性是只读的](/cn/docs/components-and-props.md#props-are-read-only)。当 `temperature` 是本地状态时， `TemperatureInput` 只能通过调用 `this.setState()` 来改变它。然而，现在 `temperature`是从父级中作为一个属性传入， `TemperatureInput` 就无法控制它。

在 React 中，这通常通过使组件 "controlled（受控）" 来实现。就像DOM `<input>` 接受一个 `value` 和一个 `onChange` 属性一样，所以可以定制 `TemperatureInput` 接受来自父级 `Calculator` 的 `temperature` 和 `onTemperatureChange` 属性。

现在，当 `TemperatureInput` 要更新它的温度，它调用 `this.props.onTemperatureChange`:

```js{3}
  handleChange(e) {
    // 之前是: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
```

注意，在自定义的组件中 `temperature` 或 `onTemperatureChange` 没有特殊的含义。我们也能用其它任意名称命名它们，像命名它们为 `value` 和 `onChange` ，是一个常见的惯例。

`onTemperatureChange` 属性将和 `temperature` 属性一起提供通过父级 `Calculator` 组件提供。它将通过修改自己的本地状态来处理变更，这样就可以用新的值来重新渲染两个输入。我们不久将看到一个新的 `Calculator` 实现。

在修改 `Calculator` 前，让我们简要回顾下对 `TemperatureInput` 的修改。我们已经从中移除了本地状态，不是读取 `this.state.temperature`，我们现在读取 `this.props.temperature` 。当我们要更改时，不是调用`this.setState()` ，我们现在调用 `this.props.onTemperatureChange()`, 这将由 `Calculator` 提供:

```js{8,12}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

现在让我们来看一下 `Calculator` 组件。

我们将当前输入的 `temperature` 和 `scale` 存储到本地状态。这是我们从输入 "提升" 的的状态，并且它将充当两个输入的 "单一数据源" ，为了渲染两个输入，我们需要知道所有数据的最小表示。

例如，如果我们在 Celsius 输入框中输入 37， `Calculator` 组件的状态将是：

```js
{
  temperature: '37',
  scale: 'c'
}
```

如果我们随后编辑 Fahrenheit 字段为 212， `Calculator` 的状态应该是：

```js
{
  temperature: '212',
  scale: 'f'
}
```

我们可以存储两个输入框的值，但事实证明是不必要的。存储最近变化的输入值以及它所表示的度量衡就够了。然后，我们可以基于当前的 temperature(温度) 和 scale(度量衡) 来推断其他输入的值。

输入框保持同步，因为他们的值是从同样的状态中计算的：

```js{6,10,14,18-21,27-28,31-32,34}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```

[在 CodePen 中试试](http://codepen.io/valscion/pen/jBNjja?editors=0010)

现在，不管你编辑哪个输入框，在 `Calculator` 中的 `this.state.temperature` 和 `this.state.scale` 都会更新。其中一个输入框获取值，所以任何用户输入都被保留，并且另一个输入总是基于它重新计算值。

让我们简要回顾一下当你编辑输入框时发生了什么；

* React 调用 DOM `<input>` 上指定的函数 `onChange` 。在我的案例中，它是在 `TemperatureInput` 组件上的 `handleChange` 方法。
* 在 `TemperatureInput` 组件上的 `handleChange` 方法使用新的期望值调用 `this.props.onTemperatureChange()` 。在它的属性（props）里包含 `onTemperatureChange`，通过它的父级 `Calculator` 组件指定。
* 当它预渲染时， `Calculator` 已经指定了摄氏 `TemperatureInput` 的 `onTemperatureChange` 是 `Calculator` 的 `handleCelsiusChange` 方法，且华氏  `TemperatureInput` 是 `Calculator` 的 `handleFahrenheitChange` 方法。所以会根据我们编辑的输入框分别调用这两个 `Calculator` 方法。
* 在这些方法中， `Calculator` 组件要求 React 通过使用新的输入值和刚刚编辑的输入框的当前度量衡来调用 `this.setState()` 重新渲染自己。
* React 调用 `Calculator` 组件的 `render` 方法来了解 UI 应该长什么样子。两个输入的值基于当前温度和激活的度量衡重新计算。在这里执行温度转换。
* React 使用 Calculator 指定的新 props(属性) 调用各个 TemperatureInput 组件的 render 方法。 它了解 UI 外观应该是什么样子。
* React DOM 更新 DOM 以匹配期望的输入值。我们刚刚编辑的输入框接收当前值，另一个输入框更新为转换后的温度。

每次更新都会执行相同的步骤，以便保持输入同步。

## 经验教训

在一个 React 应用中，对于任何可变的数据都应该循序“单一数据源”原则。通常情况下，state 首先被添加到需要它进行渲染的组件。然后，如果其它的组件也需要它，你可以提升状态到它们最近的祖先组件。你应该依赖 [从上到下的数据流向](/cn/docs/state-and-lifecycle.md#the-data-flows-down)，而不是试图在不同的组件中同步状态。

提升状态相对于双向绑定方法需要写更多的“模板”代码，但是有一个好处，它可以更方便的找到和隔离 bugs。由于任何 state(状态) 都 “存活” 在若干的组件中，而且可以分别对其独立修改，所以发生错误的可能大大减少。另外，你可以实现任何定制的逻辑来拒绝或者转换用户输入。

如果某个东西可以从 props(属性) 或者 state(状态) 得到，那么它可能不应该在 state(状态) 中。例如，我们只保存最后编辑的 temperature 和它的 scale，而不是保存 celsiusValue 和 fahrenheitValue 。另一个输入框的值总是在 render() 方法中计算得来的。这使我们对其进行清除和四舍五入到其他字段同时不会丢失用户输入的精度。

当你看到 UI 中的错误，你可以使用 [React 开发者工具](https://github.com/facebook/react-devtools) 来检查 props ，并向上遍历树，直到找到负责更新状态的组件。这使你可以跟踪到 bug 的源头：

<img src="/cn/img/docs/react-devtools-state.gif" alt="Monitoring State in React DevTools" width="100%">

---

* 上一篇：[表单](/cn/docs/forms.md)
* 下一篇：[组合 vs 继承](/cn/docs/composition-vs-inheritance.md)

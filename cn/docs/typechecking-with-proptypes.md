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
* [表单](/cn/docs/forms.md)
* [状态提升](/cn/docs/lifting-state-up.md)
* [组合 vs 继承](/cn/docs/composition-vs-inheritance.md)
* [用 React 思考](/cn/docs/thinking-in-react.md)

#### 高级教程

* [深入JSX](/cn/docs/jsx-in-depth.md)
* [**`使用 PropTypes 做类型检查`**](/cn/docs/typechecking-with-proptypes.md)
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

# 使用 PropTypes 做类型检查

> 注意:
> `React.PropTypes` 从 React v15.5 开始弃用. 请使用 [ `prop-types` 库](https://www.npmjs.com/package/prop-types) 代替.

随着你的应用程序的增长，你可以通过类型检测捕获许多的bug。对于一些应用，你可能需要使用类似于 [Flow](https://flowtype.org/) 或者 [TypeScript](https://www.typescriptlang.org/) 等 JavaScript 扩展来对你整个应用做类型检测。但即使你不使用这些，React 内置了类型检测的功能。要在组件中进行类型检测，你可以赋值特别的  `propTypes`  属性。

```javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

`PropTypes` 输出了一系列的验证器，可以用来确保接收到的参数是有效的。在这个示例中，我们使用 `PropTypes.string`. 当一个无效的值被作为属性提供, 一个警告将显示在 JavaScript 控制台. 出于性能的原因，`propTypes` 仅在开发模式中检测.

### PropTypes

这里是一个示例记录提供的不同的验证器：

```javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你能声明一个属性是特定的原始类型。默认情况下，它们都是可选的
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何东西都可以被渲染：数字, 字符串, 元素 或一个包含这些类型的数组（或片断）
  optionalNode: PropTypes.node,

  // React 元素
  optionalElement: PropTypes.element,

  // 你也能声明一个属性是类实例，使用 JS 的 instanceof 运算符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你能确保你的属性是限于枚举的特定值
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可能是许多类型中的一个
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个某些类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 属性值为某种类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个特定形式的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 你可以使用 `isRequired' 链接上述任何一个，以确保在没有提供 prop 的情况下显示警告。
  requiredFunc: PropTypes.func.isRequired,

  // 任何数据类型的值
  requiredAny: PropTypes.any.isRequired,

  // 你也能指定一下定制的验证器。假如这个验证失败，它应该返回一个错误（Error）对象。
  // 不要使用 `console.warn` 或 throw ，因为它不会在 `oneOfType` 中起作用
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 你也能提供一个定制的验证器给 `arrayOf` 和 `objectOf`.
  // 假如这个验证失败，它应该返回一个错误（Error）对象。这个验证器将在数组或对象的每一个元素上调用。
  // 验证器的前两个参数是数组或对象自身，以及当前元素的键值
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

### 要求单独的 Child

通过 `PropTypes.element` 你能指定仅一个单一的子元素能被传递到组件，作为子节点

```javascript
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // 这里必须是一个元素，否则它将报错
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};
```

### 默认属性值

你能通过赋值特定的 `defaultProps` 属性，为你的 `props` 定义默认值

```javascript
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 指定 props 的默认值：Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染为 "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

`defaultProps` 将被用来确保如果它是没有通过父组件指定值的时候 `this.props.name` 将有一个值。`propTypes` 类型检查发生在 `defaultProps` 解析之后，因此也会对 `defaultProps` 进行类型检查.

---

* 上一篇：[深入JSX](/cn/docs/jsx-in-depth.md)
* 下一篇：[Refs 和 DOM](/cn/docs/refs-and-the-dom.md)

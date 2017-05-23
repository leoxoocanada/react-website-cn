[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

# JSX 介绍

考虑一下这个变量声明：

```js
const element = <h1>Hello, world!</h1>;
```

这个有趣的标签语法既不是一个字符串也不是一个HTML。

这就是 JSX，它是 JavaScript 的一个语法扩展，我们建议在 React 中使用这种语法来描述 UI 信息。JSX 可能让你想到模板语言，但它拥有 JavaScript 的全部能力。

JSX 可以生成 React 元素。我们将在 [下一篇](/cn/docs/rendering-elements.md) 中探索如何将它渲染到 DOM 上，接下来，你能找到 JSX 的基础知识，以帮助您开始使用。

### 在 JSX 中嵌入表达式

你可以在 JSX 中嵌入任意的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) ，通过花括号包起来。

例如 `2 + 2`, `user.firstName`, 和 `formatName(user)` 都是有效的表达式：

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在 CodePen 尝试](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)

为了便于阅读，我们将 JSX 分割成多行。我们也建议将它们包含在圆括号里，虽然它不是必须的，当我们这么做时可以避免 [自动分号插入](http://stackoverflow.com/q/2846283) 的陷阱。

### JSX 也是一个表达式

编译完成后，JSX 表达式将变成普通的 JavaScript 对象。

这意味着你可以在 JSX 中使用 `if` 声明和 `for` 循环，用它给变量赋值，作为参数接收，或者作为函数的返回值：

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### 在 JSX 中指定属性值

你可以使用双引号指定字符串字面量作为属性：

```js
const element = <div tabIndex="0"></div>;
```

你也可以使用花括号嵌入一个 JavaScript 表达式作为属性值：

```js
const element = <img src={user.avatarUrl}></img>;
```

在属性中当嵌入 JavaScript 表达式时不要使用双引号包裹花括号，否则 JSX 将它属性当成一个字符串字面量而不是一个表达式，你应该使用双引号 (字符串值) 或花括号 (表达式)其中一个，但两都不能用于同一属性。

### 在 JSX 中指定子元素

如果标签是空的，你应该像 XML 一样直接通过 `/>` 来闭合标签：

```js
const element = <img src={user.avatarUrl} />;
```

JSX 标签可以包含子元素：

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

>**警告:**
>
>由于 JSX 相比 HTML 更接近于 JavaScript，React DOM 使用驼峰属性命名约定代替 HTML 属性名称。
>
>例如，在 JSX 中 `class` 变成 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) ，并且 `tabindex` 变成 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### JSX 防止注入攻击

在 JSX 中嵌入用户输入是安全的：

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

默认情况下，React DOM 在 渲染他们之前会 [转义](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) 任何嵌入 JSX 的值。从而保证用户无法注入任何应用之外的代码。在被渲染之前任何东西都会被转换成字符串。这将帮助你防止 [XSS (跨站脚本)](https://en.wikipedia.org/wiki/Cross-site_scripting) 攻击。

### JSX 表示对象

Babel 将 JSX 编译成 `React.createElement()` 调用。

这两个例子是完全相同的：

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` 执行一些检查以帮助你写出没有bug的代码，本质上它是创建了一个如下的对象：

```js
// 提示：这是一个简化了的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

这些对象被称作 "React 元素"。你可以把它们想像成为你想在屏幕上显示内容的一种描述。React 读取这些对象，使用它们构建 DOM，并且保持它们的不断更新。

我们将在下一章节探索如何将 React 元素渲染到 DOM。

>**提示:**
>
>我们建议你去搜索一下你选择的编辑器的 "Babel" 语法方案，以便 ES6 和 JSX 代码能够被正确的高亮。




---
<details>
  <summary>文档导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md")
* [**`JSX 介绍`**](/cn/docs/introducing-jsx.md)
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

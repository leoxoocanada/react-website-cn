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

* [**`深入JSX`**](/cn/docs/jsx-in-depth.md)
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

# 深入JSX

从本质上讲, JSX 只是为 `React.createElement(component, props, ...children)` 函数提供的语法糖。JSX 代码:

```js
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

编译为:

```js
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

如果没有子节点的话，你也能使用自闭合格式的标签. 例如:

```js
<div className="sidebar" />
```

编译为:

```js
React.createElement(
  'div',
  {className: 'sidebar'},
  null
)
```
如果你要测试一些特殊的 JSX 如何转换为 JavaScript，你可以尝试 [在线 Babel 编译](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&code=function%20hello()%20%7B%0A%20%20return%20%3Cdiv%3EHello%20world!%3C%2Fdiv%3E%3B%0A%7D).

## 指定 React 元素类型

一个 JSX 标签的的开始部分决定了 React 元素的类型。

首大写的 JSX 标签表示一个 React 组件。这些标签会被编译为一个直接引用的命名变量，所以如果你使用 JSX `<Foo />` 表达式， `Foo` 必须在作用域中。

### React 必须在作用域中

由于 JSX 编译为 `React.createElement` 的调用，所以 `React` 库也必须一直在你的 JSX 代码的作用域中。

例如，在这段代码中两个 imports 是必须的，即使 `React` 和 `CustomButton` 没有直接从 JavaScript 中引用：

```js{1,2,5}
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```

如果你不需要使用 JavaScript 打包器，而是通过 `<script>` 标签加载 React，它已经作为全局 `React` 存在。

### 为 JSX 类型使用点符号

你也能在 JSX 中通过点符号引用一个 React 组件。如果你有一个独立的模块导出多个 React 组件的话，它是很方便的。例如，如果 `MyComponents.DatePicker` 是一个组件，你能直接在 JSX 中使用它：

```js{10}
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

### 用户自定义组件必须首字母大写

当一个元素类型以小写字母开头，它引用一个像 `<div>` 或 `<span>` 这样的内置组件，会给 `React.createElement` 传递 `'div'` 或 `'span'` 字符串。像 `<Foo />` 这样以大写字母开头的类型，会被编译到 `React.createElement(Foo)` ，对应一个在你的 JavaScript 文件中定义或导入的组件。

我们建议用一个大写首字母给组件命名。如果你已经有一个以小写字母开头的组件，在 JSX 中使用前将它赋值给一个大字书字母开头的变量。

例如，这段代码将不会以预期的方式运行：

```js{3,4,10,11}
import React from 'react';

// 错误！这是一个组件，首字母应该大写：
function hello(props) {
  // 正确！这种使用 <div> 是合法的，因为 div 是一个有效的HTML标记：
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 错误！React 认为 <hello /> 是一个 HTML 标签，因为它首字母应不是大写的：
  return <hello toWhat="World" />;
}
```

为了修复这个问题，我们将 `hello` 重命名为 `Hello` ，并且使用在引用时使用 `<Hello />` :

```js{3,4,10,11}
import React from 'react';

// 正确！ 这是一个组件，首字母应该大写：
function Hello(props) {
  // 正确！这种使用 <div> 是合法的，因为 div 是一个有效的HTML标记：
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 正确! React 认为 <Hello /> 是一个组件，因为首字母大写
  return <Hello toWhat="World" />;
}
```

### 在运行时选择类型

你不能使用一个普通的表达式作为 React 元素类型。如果你要使用一个普通的表达式来表示元素类型，首先需要将它赋值给首字母大写的变量。这经常发生在当你要根据一个属性渲染一个不同的组件的时候：

```js{10,11}
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 错误！JSX 类型不能是表达式
  return <components[props.storyType] story={props.story} />;
}
```

要修复这个问题，我们首先将它赋值给首字母大写的变量：

```js{10-12}
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 类型可以是首字母大写的变量.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

## JSX 中的属性(props)

有一些不同的方式来指定 JSX 中的属性(props)

### JavaScript 表达式作为 Props

你可以传递任何 JavaScript 表达式作为一个属性，通过 `{}` 包裹起来。例如，在这个 JSX 中：

```js
<MyComponent foo={1 + 2 + 3 + 4} />
```

对于 `MyComponent`, `props.foo` 的值将是 `10` ，因为表达式 `1 + 2 + 3 + 4` 会被计算估值.

在 JavaScript 里 `if` 声明和 `for` 循环不是表达式，因为它们不能被直接在 JSX 里使用。你可以把它们放在附近的代码块中。例如：

```js{3-7}
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

你能在想着章节中了解有关 [条件渲染](/cn/docs/conditional-rendering.md) 和 [循环](/cn/docs/lists-and-keys.md) 的更多信息.

### 字符串字面量

你能传递一个字符串字面量作为一个属性。这两个 JSX 表达式是等价的：

```js
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

当你传递一个字符串字面量，它的值是未转义的 HTML(HTML-unescaped) 。因此这两个 JSX 表达式是等价的：

```js
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

这种行为通常是不相关的。它在这里提及只是为了完整性。

### 属性（Props）默认是 "True"

如果你传递一个没有值的属性，它的默认值是 `true` 。这两个 JSX 表达式是等价的：

```js
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

通常，我们不建议使用这种方式，因为这会与 [ES6 的 shorthand 对象](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015) 搞混。ES6 shorthand 中 `{foo}` 指的是 `{foo: foo}` 的简写，而不是 `{foo: true}` 。这种行为只是为了跟 HTML 的行为相匹配。

### 属性扩展

如果你已经有一个 object 类型的 `props` ，并且你希望在 JSX 中传入，你可以使用 `...` 作为一个扩展操作符传入整个属性对象。这两个表达式是等价的：

```js{7}
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```
当你构建一个普通的容器时，属性扩展非常有用。然而，它们也能让你的代码非常的凌乱，因为这非常容易传入许多不相干且不需要的属性到组件中。我们建议你谨慎的使用这些语法。

## JSX 中的 Children

在 JSX 表达式中可以包含开放标签和闭合标签，标签中的内容会被传递一个特殊的 props(属性) ： props.children，下面有好几种方式传递 children ：

### 字符串字面量

您可以在开放标签和闭合标签中放入一个字符串，那么 props.children 就是那个字符串。这对于内置很多 HTML 元素时非常有用，例如:

```js
<MyComponent>Hello world!</MyComponent>
```

这是一个有效的 JSX, 在 `MyComponent` 里的 `props.children` 值为字符串 `"Hello world!"`. HTML 是非转义的, 所以你能像写 HTML 一样写 JSX:

```html
<div>This is valid HTML &amp; JSX at the same time.</div>
```

JSX会删除每行开头和结尾的空格，并且也会删除空行。邻接标签的空行也会被移除，字符串之间的空格会被压缩成一个空格，因此下面的渲染效果都是相同的：

```js
<div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

### JSX Children

你可以提供多个 JSX 元素作为 children。这对显示嵌套组件非常有用：

```js
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```

你可以混合不同类型的 children(子元素) 在一起使用，所以你可以混用字符串字面量和 JSX children。这是 JSX 与 HTML 另一点相似的地方，因此下面是 HTML 和 JSX 均是有效的：

```html
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>
```

React组件不能返回多个React元素，但是单个JSX表达式可以有多个子元素，因此如果你想要渲染多个元素，你可以像上面一样，将其包裹在 div 中。

### JavaScript 表达式作为 Children(子元素)

你能传递任何 JavaScript 表达式作为 children(子元素) , 用 `{}` 包裹它们. 例如，这些表达式是等价的：

```js
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

这对于渲染长度不定的 JSX 表达式列表非常有用。例如，这渲染了一个 HTML 列表：

```js{2,9}
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```

JavaScript 表达式可以和其他类型的子元素混用，这对于字符串模板非常有用：

```js{2}
function Hello(props) {
  return <div>Hello {props.addressee}!</div>;
}
```

### Functions(函数) 作为 Children(子元素)

通常情况下，嵌入到 JSX 中的 JavaScript 表达式会被认为是一个字符串、React元素 或者是这些内容的一个列表。然而， props.children 类似于其他的 props(属性) ，可以被传入任何数据，而不是仅仅只是 React 可以渲染的数据。例如，如果有自定义组件，其 props.children 的值可以是回调函数：

```js{4,13}
// 子元素回调运行 numTimes 创建一个重复的组件
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

Children passed to a custom component can be anything, as long as that component transforms them into something React can understand before rendering. This usage is not common, but it works if you want to stretch what JSX is capable of.

### Booleans, Null, and Undefined Are Ignored

`false`, `null`, `undefined`, and `true` are valid children. They simply don't render. These JSX expressions will all render to the same thing:

```js
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

This can be useful to conditionally render React elements. This JSX only renders a `<Header />` if `showHeader` is `true`:

```js{2}
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

One caveat is that some ["falsy" values](https://developer.mozilla.org/en-US/docs/Glossary/Falsy), such as the `0` number, are still rendered by React. For example, this code will not behave as you might expect because `0` will be printed when `props.messages` is an empty array:

```js{2}
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
```

To fix this, make sure that the expression before `&&` is always boolean:

```js{2}
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```

Conversely, if you want a value like `false`, `true`, `null`, or `undefined` to appear in the output, you have to [convert it to a string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#String_conversion) first:

```js{2}
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```

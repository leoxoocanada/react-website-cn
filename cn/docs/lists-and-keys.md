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
* [**`列表和键`**](/cn/docs/lists-and-keys.md)
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

# 列表和键

首先，让我们回顾一下在 Javascript 中你是如何转换列表的。

给定下面的代码，我们使用 [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 函数处理使 `numbers` 数组的值翻倍。我们将 `map()` 返回的新数组赋值给变量 `doubled` 并且打印它：

```javascript{2}
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

这段代码将在 console 中打印 `[2, 4, 6, 8, 10]` 

在 React 中，转换数组到 [元素](/cn/docs/rendering-elements.md) 列表几乎是一样的。

### 渲染多个组件

你可以构建元素的集合，并且通过花括号 `{}` [在 JSX 中包含它们](/cn/docs/introducing-jsx.md#embedding-expressions-in-jsx)。

下面，我们使用 Javascript [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 函数依次遍历 `numbers` 数组。我们为每一项返回一个 `<li>` 元素。最后，我们将元素结果数组赋值给 `listItems`:

```javascript{2-4}
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

我们用 `<ul>` 元素包含全部的 `listItems` 数组，并且 [将它渲染到 DOM](/cn/docs/rendering-elements.md#rendering-an-element-into-the-dom):

```javascript{2}
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)

这段代码显示一个1到5的数字项目列表。

### 基本列表组件

通常你会在一个 [组件](/cn/docs/components-and-props.md) 中渲染列表.

我们能重构上一个示例到一个组件，它接收一个 `numbers` 数组并输出一个无序列表元素。

```javascript{3-5,7,13}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

当你运行这段代码，你将得到一个警告，提示一个 key 应该被提供给列表项。一个 "key" 是一个特殊的字符属性，当你创建一个元素列表时你应该包含它。我们将在下一章节讨论为什么它很重要。

让我们指定一个 `key` 到我们在 `numbers.map()` 里面的列表项，并且修复缺少 key 的问题。

```javascript{4}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

## 键(Keys)

键(Keys)帮助 React 识别哪一项已经改变了，是添加或者被移除。数组中的每一个元素都应该有一个唯一不变的键(Keys)来标识：

```js{3}
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

挑选 key 最好的方式是使用一个在它的同辈元素中不重复的标识字符串。多数情况你可以使用数据中的 IDs 作为 keys：
。
```js{2}
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

当要渲染的数据项中没有不重复的 IDs 时，你可能使用数据项的索引（index）作为 key 的最后选择：

```js{2,3}
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

如果数据项可能被重新排序时，我们不建议使用索引作为 keys ，这么做将会变得很慢。如果你有兴趣可以阅读 [深入理解为什么 keys 是必要的](/cn/docs/reconciliation.md#recursing-on-children) 

### 使用 Keys 提取组件

Keys 只在数组的上下文中有意义。

例如，如果你 [提取](/cn/docs/components-and-props.md#extracting-components) 一个 `ListItem` 组件，你应该在 `<ListItem />` 元素中保持 key，而不是在 `ListItem` 自身的 `<li>` 根元素上。

**示例：错误的 Key 用法**

```javascript{4,5,14,15}
function ListItem(props) {
  const value = props.value;
  return (
    // 错误！不需要在这里指定 key：
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 错误！key 应该在这里指定：
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**示例：正确的 Key 用法**

```javascript{2,3,9,10}
function ListItem(props) {
  // 正确！这里不需要指定 key ：
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 正确！key 应该在这里被指定
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/rthor/pen/QKzJKG?editors=0010)

一个好的经验是调用 `map()` 的元素中需要 keys。

### keys 在同辈元素中必须是唯一的

Keys used within arrays should be unique among their siblings. However they don't need to be globally unique. We can use the same keys when we produce two different arrays:

```js{2,5,11,12,19,21}
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)

Keys serve as a hint to React but they don't get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name:

```js{3,4}
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

With the example above, the `Post` component can read `props.id`, but not `props.key`.

### Embedding map() in JSX

In the examples above we declared a separate `listItems` variable and included it in JSX:

```js{3-6}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX allows [embedding any expressions](/cn/docs/introducing-jsx.md#embedding-expressions-in-jsx) in curly braces so we could inline the `map()` result:

```js{5-8}
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

[在 CodePen 中试试](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

Sometimes this results in clearer code, but this style can also be abused. Like in JavaScript, it is up to you to decide whether it is worth extracting a variable for readability. Keep in mind that if the `map()` body is too nested, it might be a good time to [extract a component](/cn/docs/components-and-props.md#extracting-components).

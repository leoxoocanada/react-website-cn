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
* [**`用 React 思考`**](/cn/docs/thinking-in-react.md)

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

# 用 React 思考

在我们看来，React 是通过 JavaScript 用来构建大型、高性能 Web 应用的最佳方式。它在 Facebook 和 Instagram 中都应用的非常好。

React 众多好的部分之一是让你思考如何构建应用。在本文档中，我们将跟你一起完成使用 React 构建一个可搜索的产品数据表的思考过程。

## 从一个 Mock 开始

想像一下我们已经有了一个 JSON API，以及从设计师那里拿到的 mock。这个 mock 看起来像这样：

![Mockup](/cn/img/blog/thinking-in-react-mock.png)

我们的 JSON API 返回一些像这样的数据:

```
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## 第一步: 将 UI 拆解到组件层次结构中

你将要做的第一件事是在mock里画一个容器包裹所有的组件（和子组件），并给它们命名。如果你和一个设计师一起工作，它们可能已经做好了这些，所以去跟他们交流一下！他们的 Photoshop 图层名可能就是你的 React 组件名！

但是你怎么知道什么应该是组件自己的呢？其实就像你创建一个新的函数或对象一样就可以。一种方法是 [单一职责原则](https://en.wikipedia.org/wiki/Single_responsibility_principle), 即一个组件理论上只做一件事。如果它持续成长，它应该分解到更小的子组件。

由于你经常向用户显示 JSON 数据模型, 你将发现如果你的模型已经正确的构建，你的 UI (以及你的组件结构) 将很好的映射. 那是因为 UI 和 数据模型往往遵循同样的 *信息结构*, 这意味着拆分你的 UI 到组件的工作是微不足道的，只需要将它拆分为精确对应对应数据模型片断即可。

![组件图解](/cn/img/blog/thinking-in-react-components.png)

在这里你将看到在我们的简单应用里有5个组件。我已经用斜体字标注了每个组件代表的数据。

  1. **`FilterableProductTable` (橙色):** 包含全部示例
  2. **`SearchBar` (蓝色):** 接收所有 *用户输入*
  3. **`ProductTable` (绿色):** 显示和过滤基于 *用户输入* 的 *数据集合*
  4. **`ProductCategoryRow` (宝石绿):** 为每个 *category* 显示一个头部
  5. **`ProductRow` (红色):** 为每个 *product* 显示一行

如果你看到了 `ProductTable`, 你将看到表格头(包含 "Name" 和 "Price" 标签) 不是独立的组件。 这是个人喜好问题，并且有一些用其它方式实现的争论。在这个示例中,我们把它分离出来作为 `ProductTable` 的一部分，是因为渲染 *数据集合* 是 `ProductTable` 职责的一部分。然而，这个头部变得非常复杂(例如：如果我们添加支持排序)的时候, 这时候创建一个独立的 ProductTableHeader 组件更合适。

现在我们已经在我们的 mock 里对组件作了标识，让我们对它们做一个层次结构排列。这是容易的。在 mock 中出现在其它组件中的组件，应该在层次结构上显示为子组件：

  * `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
      * `ProductCategoryRow`
      * `ProductRow`

## 第二步：用 React 构建一个静态版本

[在 Codepen 中看效果](https://codepen.io/lacker/pen/vXpAgj)

既然你有了组件的层次结构，那是时候实现你的应用了。最简单方法是构建一个采用数据模型并渲染 UI 但没有交互行为的版本。最好解耦这个过程，因为创建一个静态版本需要大量代码和少量的思考，而添加交互行为需要大量的思考和少量的代码。我们将看看为什么。

构建一个应用的静态版本用来渲染你的数据模型，你将要构建可复用其它组件的组件，并且使用 *props* 传递数据， *props* 是一种从父级向子级传递数据的方式。如果你熟悉 *state* 的概念， 构建静态版本时 **不要使用state** . State 只用来处理交互行为, 也就是说, 数据可以随时被改变。由于这是应用的一个静态版本，所以你不需要使用它。

你可以从上到下的构建，也可以从下到上的构建。也就是说, 你既可以从构建层次结构的顶端开始(例如： 从`FilterableProductTable`开始)，也可以从底层开始(`ProductRow`). 在更简单的示例中, 通常从上而下更容易，在大型项目中，从下而上为构建编写测试更容易.

在这一步结束时, 你已经有了一个可重用的组件库，用于渲染你的数据模型。由于这是应用的一个静态版本，所以组件将只有 `render()` 方法. 层次结构顶部的组件 (`FilterableProductTable`) 将接收你的数据模型作为一个属性（porp）。如果你对基础数据模型做了更改，并再次调用 `ReactDOM.render()` , UI 将更新. 这有利于查看 UI 的更新以及相关的数据变化在哪里， 因为那里没有什么复杂的事情发生。React 的**单向数据注** (也叫 *单向绑定*) 保持所有都是模块化和高性能.

如果执行这一步你需要帮助，可以简单的参考 [React docs](/react/docs/) .

### A Brief Interlude: Props vs State

There are two types of "model" data in React: props and state. It's important to understand the distinction between the two; skim [the official React docs](/react/docs/interactivity-and-dynamic-uis.html) if you aren't sure what the difference is.

## Step 3: Identify The Minimal (but complete) Representation Of UI State

To make your UI interactive, you need to be able to trigger changes to your underlying data model. React makes this easy with **state**.

To build your app correctly, you first need to think of the minimal set of mutable state that your app needs. The key here is DRY: *Don't Repeat Yourself*. Figure out the absolute minimal representation of the state your application needs and compute everything else you need on-demand. For example, if you're building a TODO list, just keep an array of the TODO items around; don't keep a separate state variable for the count. Instead, when you want to render the TODO count, simply take the length of the TODO items array.

Think of all of the pieces of data in our example application. We have:

  * The original list of products
  * The search text the user has entered
  * The value of the checkbox
  * The filtered list of products

Let's go through each one and figure out which one is state. Simply ask three questions about each piece of data:

  1. Is it passed in from a parent via props? If so, it probably isn't state.
  2. Does it remain unchanged over time? If so, it probably isn't state.
  3. Can you compute it based on any other state or props in your component? If so, it isn't state.

The original list of products is passed in as props, so that's not state. The search text and the checkbox seem to be state since they change over time and can't be computed from anything. And finally, the filtered list of products isn't state because it can be computed by combining the original list of products with the search text and value of the checkbox.

So finally, our state is:

  * The search text the user has entered
  * The value of the checkbox

## Step 4: Identify Where Your State Should Live

[在 Codepen 中看效果](https://codepen.io/lacker/pen/ORzEkG)

OK, so we've identified what the minimal set of app state is. Next, we need to identify which component mutates, or *owns*, this state.

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. **This is often the most challenging part for newcomers to understand,** so follow these steps to figure it out:

For each piece of state in your application:

  * Identify every component that renders something based on that state.
  * Find a common owner component (a single component above all the components that need the state in the hierarchy).
  * Either the common owner or another component higher up in the hierarchy should own the state.
  * If you can't find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

Let's run through this strategy for our application:

  * `ProductTable` needs to filter the product list based on state and `SearchBar` needs to display the search text and checked state.
  * The common owner component is `FilterableProductTable`.
  * It conceptually makes sense for the filter text and checked value to live in `FilterableProductTable`

Cool, so we've decided that our state lives in `FilterableProductTable`. First, add an instance property `this.state = {filterText: '', inStockOnly: false}` to `FilterableProductTable`'s `constructor` to reflect the initial state of your application. Then, pass `filterText` and `inStockOnly` to `ProductTable` and `SearchBar` as a prop. Finally, use these props to filter the rows in `ProductTable` and set the values of the form fields in `SearchBar`.

You can start seeing how your application will behave: set `filterText` to `"ball"` and refresh your app. You'll see that the data table is updated correctly.

## Step 5: Add Inverse Data Flow

[在 Codepen 中看效果](https://codepen.io/lacker/pen/qRqmjd)

So far, we've built an app that renders correctly as a function of props and state flowing down the hierarchy. Now it's time to support data flowing the other way: the form components deep in the hierarchy need to update the state in `FilterableProductTable`.

React makes this data flow explicit to make it easy to understand how your program works, but it does require a little more typing than traditional two-way data binding.

If you try to type or check the box in the current version of the example, you'll see that React ignores your input. This is intentional, as we've set the `value` prop of the `input` to always be equal to the `state` passed in from `FilterableProductTable`.

Let's think about what we want to happen. We want to make sure that whenever the user changes the form, we update the state to reflect the user input. Since components should only update their own state, `FilterableProductTable` will pass callbacks to `SearchBar` that will fire whenever the state should be updated. We can use the `onChange` event on the inputs to be notified of it. The callbacks passed by `FilterableProductTable` will call `setState()`, and the app will be updated.

Though this sounds complex, it's really just a few lines of code. And it's really explicit how your data is flowing throughout the app.

## And That's It

Hopefully, this gives you an idea of how to think about building components and applications with React. While it may be a little more typing than you're used to, remember that code is read far more than it's written, and it's extremely easy to read this modular, explicit code. As you start to build large libraries of components, you'll appreciate this explicitness and modularity, and with code reuse, your lines of code will start to shrink. :)

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

如果执行这一步你需要帮助，可以简单的参考 [React docs](/cn/docs/) .

### 小插曲: Props(属性) vs State(状态)

在 React 里有两种类型的 "模型" 数据: props 和 state. 理解两者之间的差别非常重要; 如果你不确定有什么不同可以浏览 [React 官方文档](/cn/docs/interactivity-and-dynamic-uis.md).

## 第三步：确定 UI 状态（State）最小（但完整）的表示

使你的 UI 有交互性, 你需要能够触发更底层的数据模型。React通过 **state** 让它变得更简单 .

要正确的构建你的应用, 你首先需要思考你的应用需要的可变状态的最小集合。这里的关键是 DRY: *不要重复你自己（Don't Repeat Yourself）*. 找出你的应用程序所需 state(状态) 的绝对最小表示，并且可以以此计算出你所需的所有其他数据内容。例如, 如果你正在构建一个 TODO 列表, 只保留一个 TODO 元素数组即可；不需要为数量保留一个单独的 state(状态) 变量。相反, 当你要渲染 TODO 数量, 简单的获取 TODO 列表数组长度就可以。

思考在我们示例应用中的所有数据。我们有：

  * 原始产品列表
  * 用户已经输入的搜索文本
  * checkbox 的值
  * 过滤后的产品列表

让我们仔细检查每个数据，并且找出哪个是状态。简单的提出关于第块数据的3个问题：

  1. 是否通过 props 从父级传入？如果是，它可能不是 state。
  2. 是否永远保持不变？如果是，它可能不是 state。
  3. 是否能基于组件中的其它 state 或 props 计算得出？如果是，这不是 state。

原始产品列表作为 props 传入， 所以它不是 state. 搜索文本和 checkbox 看起来像是 state ，因为它们随时会变化，并且不能从其它数据计算得出. 最后, 产品过滤列表不是 state，因为它可以通过结合原始产品列表、搜索文本和 checkbox 计算得出.

所以最终, 我们的 state 是:

  * 用户输入的搜索文本
  * checkbox 的值

## 第四步：确定你的状态的位置

[在 Codepen 中看效果](https://codepen.io/lacker/pen/ORzEkG)

好了，我们已经确定了应用状态（state）的最小集合。下一步，我们需要确定哪个组件可变，或者说 *拥有（owns）* 这些状态（state）

记住：React 单向数据流在组件中从上而下的进行。这样有可能不能立即判断状态属于哪个组件。**这是经常对新手理解最有挑战的一部分,** 按下面的步骤把它弄明白：

对于你应用中的每一个状态（state）:

  * 确定每个基于状态渲染的组件
  * 找出公共父级组件 (一个单独的组件，在层次结构中位于所有需要状态的组件之上).
  * 公共父级组件或另一个在层次结构中更高级的组件应该拥有这个状态.
  * 如果你不能找到一个拥有该state的合适组件, 可以创建一个简单的新组件来保留这个状态，并将它添加到公共父级组件的上层层次结构的任意位置.

让我们在我们的应用中贯穿这个策略:

  * `ProductTable` 需要基于状态过滤产品列表， `SearchBar` 需要显示搜索文本和选中状态.
  * 公共父组件是 `FilterableProductTable`.
  * 它从概念上讲适用于过滤文本和选中值，应该存在于 `FilterableProductTable`

那么我们已经决定我们的状态保存在 `FilterableProductTable`. 首先, 添加一个实例属性 `this.state = {filterText: '', inStockOnly: false}` 到 `FilterableProductTable` 的 `constructor` 来反映应用的初始化状态. 然后, 传递 `filterText` 和 `inStockOnly` 到 `ProductTable` 和 `SearchBar` 作为一个属性. 最后, 使用这些属性来过滤 `ProductTable` 中的行，并设置 `SearchBar` 中的表单字段的值.

你可以看一下你的应用的行为： 设置 `filterText` 为 `"ball"` 并刷新你的应用。你将看到数据表格被正确的更新。

## Step 5: Add Inverse Data Flow

[在 Codepen 中看效果](https://codepen.io/lacker/pen/qRqmjd)

So far, we've built an app that renders correctly as a function of props and state flowing down the hierarchy. Now it's time to support data flowing the other way: the form components deep in the hierarchy need to update the state in `FilterableProductTable`.

React makes this data flow explicit to make it easy to understand how your program works, but it does require a little more typing than traditional two-way data binding.

If you try to type or check the box in the current version of the example, you'll see that React ignores your input. This is intentional, as we've set the `value` prop of the `input` to always be equal to the `state` passed in from `FilterableProductTable`.

Let's think about what we want to happen. We want to make sure that whenever the user changes the form, we update the state to reflect the user input. Since components should only update their own state, `FilterableProductTable` will pass callbacks to `SearchBar` that will fire whenever the state should be updated. We can use the `onChange` event on the inputs to be notified of it. The callbacks passed by `FilterableProductTable` will call `setState()`, and the app will be updated.

Though this sounds complex, it's really just a few lines of code. And it's really explicit how your data is flowing throughout the app.

## And That's It

Hopefully, this gives you an idea of how to think about building components and applications with React. While it may be a little more typing than you're used to, remember that code is read far more than it's written, and it's extremely easy to read this modular, explicit code. As you start to build large libraries of components, you'll appreciate this explicitness and modularity, and with code reuse, your lines of code will start to shrink. :)

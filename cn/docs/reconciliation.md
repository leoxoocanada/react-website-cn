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
* [**`一致性比较（Reconciliation）`**](/cn/docs/reconciliation.md)
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

# 一致性比较（Reconciliation）

React 提供了一个声明式的 API，因此您不必担心每次更新会有什么变化。这使得编写应用程序容易得多，但是在React中如何实现它可能不是很明显。本文介绍了我们在React的"差异（diffing）"算法中做出的选择，以便组件更新是可预测的，同时对于高性能应用程序来说足够快。

## 动机

当您使用React时，在单个时间点，您可以将 `render()` 函数视为创建一个React元素树。 在下一个状态或道具更新中，`render（）`函数将返回一个不同的React元素树。 然后需要确定如何有效地更新UI以匹配最近的树。

对于生成最小数量的操作以将一棵树转换为另一棵树的算法问题，存在一些通用的解决方案。 然而，[现有技术算法](http://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf) 具有为O(n3)的复杂度,其中n是树中元素的数量。

如果我们在React中使用这种算法显示1000个元素将需要十亿次比较。这样的代价过于昂贵，相反，React 基于以下两个假设实现了时间复杂度为 O(n) 的算法:

1. 不同类型的两个元素将会产生不同的树。
2. 开发人员可以提示使用 `key` 属性来指示在不同渲染器上哪些子元素可能是稳定的。

实际上，这些假设对于几乎所有的实际用例都是有效的。

## Diffing 算法

当比较两棵树时，React首先比较两个根元素。 根据根元素的类型，行为是不同的。

### 元素类型不相同

无论什么时候当根节点有不同的类型时，React 将销毁原先的树并重建新的树。从 `<a>` 到 `<img>`, 或从 `<Article>` 到 `<Comment>`, 或从 `<Button>` 到 `<div>` ，任何一个都将导致完全的重建。

当销毁一颗树时，老的 DOM 节点将被销毁。组件实例执行 `componentWillUnmount()`.当构建一颗新树时，新的 DOM 节点将被插入到 DOM。组件实例执行 `componentWillMount()` ，然后执行  `componentDidMount()`.与之前旧的树相关的 state 都会丢失。

根节点以下的任何组件都会被卸载(unmounted)，其 state(状态)都会丢失。例如，当比较:

```xml
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

这将销毁老的 `Counter` 并重新装载一个新的。

### 元素类型相同

当比较两个相同类型的React DOM元素时，React会查看两者的属性，保留相同的底层DOM节点，并且只更新已更改的属性。 例如：

```xml
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

比较这两个元素，React只会修改底层 DOM 节点的 `className` 。

当更新 `style`，React也只会更新变动的属性。例如：

```xml
<div style={{'{{'}}color: 'red', fontWeight: 'bold'}} />

<div style={{'{{'}}color: 'green', fontWeight: 'bold'}} />
```

当在这两个元素之间转换时，React知道只修改`color`样式，而不是`fontWeight`。

处理DOM节点后，React然后对子节点进行递归。

### 相同类型的组件

当组件更新时，实例保持不变，从而在渲染之间保持状态。 React更新底层组件实例的属性以匹配新元素，并调用底层实例上的 `componentWillReceiveProps()` 和 `componentWillUpdate()` 。

下一步， `render()` 方法被调用，并且 diff 算法在上一次结果中和新的结果中递归。

### 子元素递归

默认情况下，当对DOM节点的子节点进行递归时，React只会同时遍历两个子元素列表，并在有差异时生成突变（mutation）。

例如，当在子节点末尾添加一个元素时，这两棵树之间的转换会正常进行：

```xml
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

React 将比较两个 `<li>first</li>` 树，比较两个 `<li>second</li>` 树，然后插入 `<li>third</li>` 树。

如果你在开始处插入一个节点也是这样简单地实现，那么性能将会很差。例如，在下面两棵树的转化中性能就不佳。

```xml
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

React 将会改变每一个子节点而没有意识到需要保留 `<li>Duke</li>` 和 `<li>Villanova</li>` 两个子树。这种低效是一个问题。

### Keys

为了解决这个问题，React支持一个`key`属性。 当子节点有`key`时，React使用`key`将原始树中的子节点与随后的树中的子节点相比较。 例如，在上面的低效示例中添加一个`key` 可以使树的转换更有效：

```xml
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

现在，React知道 key 为 `'2014'` 的元素是新的元素，而带有`'2015'` 的 `'2016'` key 的元素仅仅只是移动罢了。

实际上，找到 key 通常并不困难。 您要显示的元素可能已经具有唯一的ID，因此 key 可能来自您的数据：

```js
<li key={item.id}>{item.name}</li>
```

当不是这种情况时，您可以向模型添加新的ID属性，或者将内容的某些部分加入生成 key。 key 只要在兄弟节点之间是独一无二的就可以，而不是全局唯一的。

作为最后的手段，您可以将数组中的项目索引作为 key。 如果项目永远不会重新排序，这可能会很好，但是重新排序会很慢。

## 权衡利弊

需要记住的是 reconciliation 算法(reconciliation algorithm)仅仅只是一个实现细节。React 会在每个操作上重新渲染整个应用，最终的结果可能是相同的。我们经常细化启发式算法，以便优化性能。

在当前的实现中，您可以表达一个事实，即一个子树已被移到其兄弟之间，但是您不能指出它已经移动到别的地方。 该算法将重新渲染完整的子树。

因为React依靠启发式，如果它们背后的假设没有得到满足，性能就会受到影响。

1. 该算法不会尝试匹配不同组件类型的子树。 如果您看到自己在两个组件类型之间交替使用非常相似的输出，您可能希望使其成为相同的类型。 实际上，我们没有发现这是一个问题。

2. Keys 应该是稳定的，可预测的和唯一的。 不稳定的键（如由 `Math.random()`）生成的键将导致许多组件实例和DOM节点被不必要地重新创建，这会导致子组件的性能下降和丢失状态。


---

* 上一篇：[不使用 JSX 的 React](/cn/docs/react-without-jsx.md)
* 下一篇：[上下文（Context）](/cn/docs/context.md)

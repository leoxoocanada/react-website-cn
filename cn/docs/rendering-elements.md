[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

---
<details>
  <summary>导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md)
* [JSX 介绍](/cn/docs/introducing-jsx.md)
* [**`渲染元素`**](/cn/docs/rendering-elements.md)
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


# 渲染元素

元素是 React 应用中最小的构建块。

一个元素描述了你将在屏幕上看到的内容：

```js
const element = <h1>Hello, world</h1>;
```

不同于浏览器 DOM 元素，React 是普通的对象，非常容易创建，React DOM 负责更新 DOM 以匹配 React 元素。

>**注意:**
>
>有人可能会将元素跟更广为人知的“组件”概念混淆，我们将在 [下一章节](/cn/docs/components-and-props.md) 学习“组件”。组件是由元素构成，所以我们鼓励你看完这个章节再看下个章节。

## 渲染元素到 DOM

让我们假设你的 HTML 文件的什么地方有这么一个 `<div>` ：

```html
<div id="root"></div>
```

我们把它叫做 DOM "根"节点，因为该节点内的所有内容都将由 React DOM 管理。

通过 React 构建的应用通常只有单个 DOM 根节点。如果你将 React 整合到当前的应用中，你可能需要有一些相互独立的 DOM 根节点。

要将 React 元素渲染到一个 DOM 根节点，把它们传递给 `ReactDOM.render()` 方法：

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[在 CodePen 中试用](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)

它将在页面上显示 "Hello, world" 。

## 更新已渲染的元素

React 元素是 [不可变的](https://en.wikipedia.org/wiki/Immutable_object)。一旦你创建了一个元素，就不能修改它的子元素或属性。一个元素就像电影中的一帧：它表示某一特定时间点的 UI。

就我们目前掌握的知识而言，唯一更新 UI 的方法就是创建一个新的元素，并且传递参数到 `ReactDOM.render()` 方法。

思考一下这个时钟的例子：

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[在 CodePen 中尝试一下](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

它每秒钟通过 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) 回调 `ReactDOM.render()` 方法。

>**提示:**
>
>事实上，更多 React 应用只一次调用 `ReactDOM.render()` ，在下个章节我们将学习如何将代码封装进 [带状态的组件](/cn/docs/state-and-lifecycle.md).
>
>我们建议你不要跳过任何一节主题，因为它们彼此关联。

## React 仅做必要的更新

React DOM 会将元素和它的子元素与之前的版本相对比，并只对有必要的 DOM 进行更新，以达到 DOM 所需要的状态。

你可以通过浏览器工具对 [上一个例子](http://codepen.io/gaearon/pen/gwoJZk?editors=0010) 进行检查来验证这一点：

![DOM inspector showing granular updates](/cn/img/docs/granular-dom-updates.gif)

虽然我们每隔一秒都重建了整个 UI 界面，但是整个内容中只有文本节点通被 React DOM 更新。

根据我们的经验，关注每个时间点UI的表现, 而不是关注随着时间不断更新UI的状态, 可以减少很多奇怪的 bug 。



---

* 上一篇：[JSX 介绍](/cn/docs/introducing-jsx.md)
* 下一篇：[组件和Props](/cn/docs/components-and-props.md)

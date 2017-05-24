[文档](/cn/docs/hello-world.md) | [教程](/cn/tutorial/tutorial.md) | [社区](/cn/community/support.md) | [博客](/cn/_posts/2017-04-07-react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)

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

我们把它叫做 "root" DOM 节点，因为该节点内的所有内容都将由 React DOM 管理。

Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.

To render a React element into a root DOM node, pass both to `ReactDOM.render()`:

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)

It displays "Hello, world" on the page.

## Updating the Rendered Element

React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`.

Consider this ticking clock example:

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

[Try it on CodePen.](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

It calls `ReactDOM.render()` every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.

>**Note:**
>
>In practice, most React apps only call `ReactDOM.render()` once. In the next sections we will learn how such code gets encapsulated into [stateful components](/cn/docs/state-and-lifecycle.md).
>
>We recommend that you don't skip topics because they build on each other.

## React Only Updates What's Necessary

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

You can verify by inspecting the [last example](http://codepen.io/gaearon/pen/gwoJZk?editors=0010) with the browser tools:

![DOM inspector showing granular updates](/cn/img/docs/granular-dom-updates.gif)

Even though we create an element describing the whole UI tree on every tick, only the text node whose contents has changed gets updated by React DOM.

In our experience, thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.


---
<details>
  <summary>文档导航</summary>

#### 快速入门

* [安装](/cn/docs/installation.md)
* [Hello World](/cn/docs/hello-world.md")
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

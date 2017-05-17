[文档](docs/hello-world.md) | [教程](tutorial/tutorial.md) | [社区](community/support.md) | [博客](_posts/2017/04/07/react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)


# React 

    一个构建用户交互界面的 JavaScript 库

[开始](docs/hello-world.md) | [获取教程](tutorial/tutorial.md)

| 声明式 | 组件化 | 仅需学习一次，编写任何平台 |
| ---- | ---- | ---- |
| React 让你轻松的创建可交互用户界面。只要为每个状态设计轻量的视图，当数据更新时， React 将有效的更新渲染对应的组件。声明式视图使你的代码更加可控，且易于调试。| 构建封装的组件，每个组件管理自己的状态，然后组合各个组件从而构建复杂的界面。由于组件的逻辑代码使用 Javascript 而不是模板，所以你能在应用中方便的传递丰富的数据，同时将状态跟 DOM 分离。| 我们不假设你需要掌握其它的技术， 所以你可以使用 React 开发新功能而不用重构旧代码。React 还可以在服务端通过 Node 渲染，并且使用 [React Native](https://facebook.github.io/react-native/) 做移动端开发。|

<hr />

<section class="home-section">
  <div id="examples">
    <div class="example">
      <h3>A Simple Component</h3>
      <p>
        React components implement a `render()` method that takes input data and
        returns what to display. This example uses an XML-like syntax called
        JSX. Input data that is passed into the component can be accessed by
        `render()` via `this.props`.
      </p>
      <p>
        <strong>JSX is optional and not required to use React.</strong> Try
        clicking on "Compiled JS" to see the raw JavaScript code produced by
        the JSX compiler.
      </p>
      <div id="helloExample"></div>
    </div>
    <div class="example">
      <h3>A Stateful Component</h3>
      <p>
        In addition to taking input data (accessed via `this.props`), a
        component can maintain internal state data (accessed via `this.state`).
        When a component's state data changes, the rendered markup will be
        updated by re-invoking `render()`.
      </p>
      <div id="timerExample"></div>
    </div>
    <div class="example">
      <h3>An Application</h3>
      <p>
        Using `props` and `state`, we can put together a small Todo application.
        This example uses `state` to track the current list of items as well as
        the text that the user has entered. Although event handlers appear to be
        rendered inline, they will be collected and implemented using event
        delegation.
      </p>
      <div id="todoExample"></div>
    </div>
    <div class="example">
      <h3>A Component Using External Plugins</h3>
      <p>
        React is flexible and provides hooks that allow you to interface with
        other libraries and frameworks. This example uses **remarkable**, an
        external Markdown library, to convert the textarea's value in real time.
      </p>
      <div id="markdownExample"></div>
    </div>
  </div>
  <script src="https://unpkg.com/remarkable@1.7.1/dist/remarkable.min.js"></script>
  <script src="/react/js/examples/hello.js"></script>
  <script src="/react/js/examples/timer.js"></script>
  <script src="/react/js/examples/todo.js"></script>
  <script src="/react/js/examples/markdown.js"></script>
</section>
<hr class="home-divider" />
<section class="home-bottom-section">
  <div class="buttons-unit">
    <a href="docs/hello-world.html" class="button">Get Started</a>
    <a href="tutorial/tutorial.html" class="button">Take the Tutorial</a>
  </div>
</section>

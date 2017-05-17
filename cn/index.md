[文档](docs/hello-world.md) | [教程](tutorial/tutorial.md) | [社区](community/support.md) | [博客](_posts/2017/04/07/react-v15.5.0.md) | [React Github](https://facebook.github.io/react/)


# React 

    一个构建用户交互界面的 JavaScript 库

[开始](docs/hello-world.md) | [获取教程](tutorial/tutorial.md)

| 声明式 | 组件化 | Learn Once, Write Anywhere |
| ---- | ---- | ---- |
| React 使得创建可交互用户界面变得轻松。在你的应用中为每个状态设计单个视图，并且当你的数据变化时 React 将高效的更新并渲染合适的组件。声明式视图使你的代码更加可预测且更容易调试。| 为管理他们自己的状态构建封装的组件，在 Javascript 中用复杂的可交互组件代替模板，通过你的 App 你可以很容易的传递丰富的数据，且在 DOM 上保持状态。| We don't make assumptions about the rest of your technology stack, so you can develop new features in React without rewriting existing code.React can also render on the server using Node and power mobile apps using <a href="https://facebook.github.io/react-native/">React Native</a>. |

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

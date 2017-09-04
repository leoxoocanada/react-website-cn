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
* [一致性比较（Reconciliation）](/cn/docs/reconciliation.md)
* [上下文（Context）](/cn/docs/context.md)
* [Web Components](/cn/docs/web-components.md)
* [高阶组件](/cn/docs/higher-order-components.md)
* [与其它类库集成](/cn/docs/integrating-with-other-libraries.md)

#### 参考

* [React API](/cn/docs/react-api.md)
* [React.Component](/cn/docs/react-component.md)
* [ReactDOM](/cn/docs/react-dom.md)
* [ReactDOMServer](/cn/docs/react-dom-server.md)
* [DOM 元素](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)


#### 贡献

* [**`如何贡献`**](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

# 如何贡献

React 是 Facebook 的第一个开源项目之一，它们都处于非常积极的发展之中，也被用于向 [facebook.com](https://www.facebook.com) 上的所有人发送代码。我们仍然在努力做贡献，尽可能使这个项目简单和透明，但是我们做的还不是很到位。希望这个文档可以让贡献的过程更清晰，并回答一些你可能需要的问题。

### [行为守则](https://code.facebook.com/codeofconduct)

Facebook 通过了“行为准则”，我们期望项目参与者都能遵守。请阅读[全文](https://code.facebook.com/codeofconduct) ，以便您了解将采取什么行动以及哪些行为不会被容忍。

### 开放发展

所有关于React的工作都直接在[GitHub](https://github.com/facebook/react)上进行。 核心团队成员和外部贡献者都会通过相同的审核流程发送 pull 请求。

### 分支机构

我们将竭尽全力保持[`master`分支](https://github.com/facebook/react/tree/master) 的良好状态，随时测试通过。 但为了快速迭代，我们将对您的应用程序可能不兼容的API进行更改。 我们建议您使用[最新版本的React](https://facebook.github.io/react/downloads.html)。

如果您发送 pull 请求，请在 `master` 分支上进行。 我们分别维护主要版本的稳定分支，但我们不接受直接向他们 pull 请求。 相反，我们从 master 到最新稳定的主要版本中挑选不间断的变化。

### 语义版本控制

React 遵循[语义版本](http://semver.org/)。 我们为错误发布修补程序版本，为新功能发布次要版本，为重大更改发布主要版本。 当我们进行重大更改时，我们还会在次要版本中引入废弃警告，以便我们的用户了解即将发生的更改并提前迁移其代码。

我们使用 tag 标记每个 pull 请求，标记变化是否应该在下一个 [补丁](https://github.com/facebook/react/pulls?q=is%3Aopen+is%3Apr+label%3Asemver-patch), [小改动](https://github.com/facebook/react/pulls?q=is%3Aopen+is%3Apr+label%3Asemver-minor), 或 [重大修改](https://github.com/facebook/react/pulls?q=is%3Aopen+is%3Apr+label%3Asemver-major) 版本。我们每隔几周发布新的补丁版本，每隔几月发布小改动版本，一年中1到2次发布重大修改版本。

每个重大变动都记录在 [changelog 文件](https://github.com/facebook/react/blob/master/CHANGELOG.md).

### Bugs

#### 在哪里查找已知问题

我们使用 [GitHub Issues](https://github.com/facebook/react/issues) 发布我们的公共 bug。我们会密切关注这些，并尝试在我们已经内部修复好之后清空这些问题。在提交新任务之前，请试着确保你的问题没有发布过。

#### 报告新问题

阐述你的 bug 的最佳方式是提供一个简化的测试用例。这个 [JSFiddle 模板](https://jsfiddle.net/84v837e9/) 是一个好起点。

#### 安全漏洞

Facebook 有一个 [赏金计划](https://www.facebook.com/whitehat/) 用于安全 bug 的安全披露。考虑到这一点，请不要提公开的 issues；查看页面了解披露流程。

### 如何联系

* IRC: [#reactjs on freenode](https://webchat.freenode.net/?channels=reactjs)
* 讨论区: [discuss.reactjs.org](https://discuss.reactjs.org/)

还有[Discord聊天平台上的React用户的活跃社区](http://www.reactiflux.com/) ，用于您需要React帮助的时候。

### 提出改变

如果你打算改变公共 API，或对实现做一些重大的改变，我们建议 [提出一个 issue](https://github.com/facebook/react/issues/new) 。这让我们在你付出巨大的努力之前在你的提议上可以达成共识。

如果你只是修复了一个 bug，只要提交一个 pull 请求就可以，但我们还是建议提一个 issue 来详述你修复了什么。如果我们不接受特定的修复，但是想要跟踪问题，这是有帮助的。

### 你的第一个 Pull Request

如何在你的第一个 Pull 请求上开始工作？你可以从这个免费的视频系列里学到这些：

**[如何在 GitHub 上贡献一个开源项目](https://egghead.io/series/how-to-contribute-to-an-open-source-project-on-github)**

为了帮助你适应并熟悉我们的贡献流程，我们有一个 **[初学者友好的问题](https://github.com/facebook/react/issues?q=is%3Aopen+is%3Aissue+label%3A%22Difficulty%3A+beginner%22)** 列表，其中包含相当容易修复的 bug。这是一个开始的好地方。

如果您决定解决问题，请确保检查评论，以防有人已经在修复问题。 如果目前没有人在工作，请发表评论，说明您打算在其上工作，以便其他人不会意外重复您的努力。

如果有人提出了一个问题，但没有跟进超过两个星期，接管它是很好的，但你应该留下评论。

### 发送 Pull Request

核心团队会监控 pull 请求。我们将检阅你的 pull 请求，有可能将它合并，改变请求，或关闭它并说明理由。对于 API 的改变我们可能需要修复我们在 Facebook.com 的内部使用，这可能会有一些延迟。我们将尽力在整个过程中提供更新和反馈。

**提交一个 pull request 前,** 请确保以下已经完成:

1. Fork [这个仓库](https://github.com/facebook/react) 并从 `master` 创建你的分支.
2. 如果你添加了应该要测试的代码，请添加测试用例！
3. 如果你更新了 API，请更新文档。
4. 确保测试用例通过 (`npm test`).
5. 确保你的代码通过 lint 检查 (`npm run lint`).
6. 通过 [prettier](https://github.com/prettier/prettier) 格式化你的代码 (`npm run prettier`).
7. 运行 [Flow](https://flowtype.org/) 类型检查 (`npm run flow`).
8. 如果你添加或移除了一些测试用例，请在提交 pull 请求 之前运行 `./scripts/fiber/record-tests` ，并提交改变的结果。
9. 如果还没有做什么，请先完成CLA。

### 贡献者许可协议（CLA）

In order to accept your pull request, we need you to submit a CLA. You only need to do this once, so if you've done this for another Facebook open source project, you're good to go. If you are submitting a pull request for the first time, just let us know that you have completed the CLA and we can cross-check with your GitHub username.

**[在这里完成你的CLA](https://code.facebook.com/cla)**

### 贡献前提条件

* You have `node` installed at v4.0.0+ and `npm` at v2.0.0+.
* You have `gcc` installed or are comfortable installing a compiler if needed. Some of our `npm` dependencies may require a compilation step. On OS X, the Xcode Command Line Tools will cover this. On Ubuntu, `apt-get install build-essential` will install the required packages. Similar commands should work on other Linux distros. Windows will require some additional steps, see the [`node-gyp` installation instructions](https://github.com/nodejs/node-gyp#installation) for details.
* You are familiar with `npm` and know whether or not you need to use `sudo` when installing packages globally.
* You are familiar with `git`.

### 开发工作流程

After cloning React, run `npm install` to fetch its dependencies.
Then, you can run several commands:

* `npm run lint` checks the code style.
* `npm test` runs the complete test suite.
* `npm test -- --watch` runs an interactive test watcher.
* `npm test <pattern>` runs tests with matching filenames.
* `npm run flow` runs the [Flow](https://flowtype.org/) typechecks.
* `npm run build` creates a `build` folder with all the packages.

We recommend running `npm test` (or its variations above) to make sure you don't introduce any regressions as you work on your change. However it can be handy to try your build of React in a real project.

First, run `npm run build`. This will produce pre-built bundles in `build` folder, as well as prepare npm packages inside `build/packages`.

The easiest way to try your changes is to run `npm run build` and then open `fixtures/packaging/babel-standalone/dev.html`. This file already uses `react.js` from the `build` folder so it will pick up your changes.

If you want to try your changes in your existing React project, you may copy `build/dist/react.development.js`, `build/dist/react-dom.development.js`, or any other build products into your app and use them instead of the stable version. If your project uses React from npm, you may delete `react` and `react-dom` in its dependencies and use `npm link` to point them to your local `build` folder:

```sh
cd your_project
npm link ~/path_to_your_react_clone/build/packages/react
npm link ~/path_to_your_react_clone/build/packages/react-dom
```

Every time you run `npm run build` in the React folder, the updated versions will appear in your project's `node_modules`. You can then rebuild your project to try your changes.

We still require that your pull request contains unit tests for any new functionality. This way we can ensure that we don't break your code in the future.

### 风格指南

Our linter will catch most styling issues that may exist in your code.
You can check the status of your code styling by simply running `npm run lint`.

However, there are still some styles that the linter cannot pick up. If you are unsure about something, looking at [Airbnb's Style Guide](https://github.com/airbnb/javascript) will guide you in the right direction.

### 代码约定

* Use semicolons `;`
* Commas last `,`
* 2 spaces for indentation (no tabs)
* Prefer `'` over `"`
* `'use strict';`
* 120 character line length (**except documentation**)
* Write "attractive" code
* Do not use the optional parameters of `setTimeout` and `setInterval`

### 介绍视频

You may be interested in watching [this short video](https://www.youtube.com/watch?v=wUpPsEcGsg8) (26 mins) which gives an introduction on how to contribute to React.

### 会议记录

React team meets once a week to discuss the development of React, future plans, and priorities. You can find the meeting notes in a [dedicated repository](https://github.com/reactjs/core-notes/).

### 许可

By contributing to React, you agree that your contributions will be licensed under its BSD license.

### 下一步是什么?

阅读 [下一节](/cn/contributing/codebase-overview.md) 了解代码库的结构。


---

* 上一篇：[合成事件（SyntheticEvent）](/cn/docs/events.md)
* 下一篇：[代码库概述](/cn/contributing/codebase-overview.md)

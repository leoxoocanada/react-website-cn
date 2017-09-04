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

为了接受你的 pull 请求，我们需要你提交一个 CLA。你只需要提交一次，因此如果你已经在其它的 Facebook 开源项目里提交过了，你将不需要再次提交。如果你是第一次提交 pull 请求，只要让我们知道你已经完成了 CLA，我们会再次确认你的 GitHub 用户名。

**[在这里完成你的CLA](https://code.facebook.com/cla)**

### 贡献前提条件

* 你已经安装了 `node` v4.0.0+ 和 `npm` v2.0.0+.
* 你已经安装了 `gcc` ，或者如果你需要，可以安装编译器。我们的一些 `npm` 依赖可能需要一个编辑环节。在 OS X, Xcode 命令行工具将处理这些。在 Ubuntu, 执行 `apt-get install build-essential` 将安装必要的包。类似的命令应该在其它 Linux 发行版上也能工作。Windows 将需要一些额外的步骤，查看 [`node-gyp` 安装说明](https://github.com/nodejs/node-gyp#installation) 了解详细信息.
* 你已经熟悉 `npm` 并知道在全局安装软件包时是否需要使用 `sudo` 
* 你已经熟悉 `git`.

### 开发工作流程

复制 React 之后，运行 `npm install` 摘取它的依赖。
然后，你可以运行这些命令：

* `npm run lint` 检查代码风格
* `npm test` 运行完整的测试用例
* `npm test -- --watch` 运行一个带交互的测试查看器
* `npm test <pattern>` 通过匹配的文件名运行测试用例
* `npm run flow` 运行 [Flow](https://flowtype.org/) 类型检查
* `npm run build` 创建 `build` 目录将将所有包打包进去

我们建议运行 `npm test` (或它上面的变体)来确保你的工作不会引入任何回归。然而在真实的项目中尝试构建 React 是非常方便的。

首先，运行 `npm run build`。这将在 `build` 目录生成预构建好的包，和在 `build/packages` 里预先准备好 npm 包一样。

最简单调试代的改变的方式是运行 `npm run build` ，并打开 `fixtures/packaging/babel-standalone/dev.html`。这个文件已经使用了 `build` 目录的 `react.js`，因此它将呈现你的改变。

如果你要在当前的 React 项目调试你的改变，你可以复制 `build/dist/react.development.js`, `build/dist/react-dom.development.js`, 或其它构建好的文件到你的应用，并使用它们来代替稳定的版本。如果你的产品从 npm 中使用 React，你可以在它的依赖里移除 `react` 和 `react-dom` ，并使用 `npm link` 连接到 `build` 目录:

```sh
cd your_project
npm link ~/path_to_your_react_clone/build/packages/react
npm link ~/path_to_your_react_clone/build/packages/react-dom
```

每次你在 React 目录里运行 `npm run build` ， 已经更新好的版本将出现在你的项目的 `node_modules` 里。你可以重新构建你的项目来调试你的改变。

我们仍然要求您的 pull 请求包含任何新功能的单元测试。 这样我们可以确保在将来不会破坏您的代码。

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

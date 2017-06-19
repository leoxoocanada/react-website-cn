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
* [用 React 思考](/cn/docs/thinking-in-react.md)

#### 高级教程

* [深入JSX](/cn/docs/jsx-in-depth.md)
* [使用 PropTypes 做类型检查](/cn/docs/typechecking-with-proptypes.md)
* [Refs 和 DOM](/cn/docs/refs-and-the-dom.md)
* [**`不可控组件`**](/cn/docs/uncontrolled-components.md)
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

# 不可控组件

在更多的案例里，我们建议使用 [受控组件](/cn/docs/forms.md) 来实现表单。在一个可控组件里，表单数据是 React 组件负责处理。另外一个选择是非受控组件，其表单数据由 DOM 自己负责处理。

要编写一个非受控组件，你能 [使用 ref](/cn/docs/refs-and-the-dom.md) 来从 DOM 获取值，而不是为每个状态更新写一个事件处理器。

例如，在一个非受控组件里，这段代码接受一个一个独立的名字：

```javascript{8,17}
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在CodePen中试试](https://codepen.io/gaearon/pen/WooRWa?editors=0010)

由于非受控组件的数据来源是 DOM，当使用非受控组件时它有时候更容易整合 React 和非 React 代码。如果你要求快速开发，不要求代码质量，非受控组件也可以一定程度上减少代码量。否则，通常你应该使用受控组件。

如果仍然不清楚在某个特定方案你应该使用哪种类型的组件，你可以在[这个文章中了解受控和不受控组件](http://goshakkk.name/controlled-vs-uncontrolled-inputs-react/) 寻找帮助。

### 默认值

在 React 渲染生命周期里，表单元素的 `value` 属性将覆盖 DOM 中的 value。在非受控组件中，你经常需要给 React 指定初始值，但保留后续的更新不受控制。要处理这个情况，你能指定一个 `defaultValue` 属性替代 `value`.

```javascript{7}
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={(input) => this.input = input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

同样, `<input type="checkbox">` 和 `<input type="radio">` 支持 `defaultChecked`,  `<select>` 和 `<textarea>` 支持 `defaultValue`.

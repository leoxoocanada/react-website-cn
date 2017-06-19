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
* [**`Refs 和 DOM`**](/cn/docs/refs-and-the-dom.md)
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

# Refs 和 DOM

在常规的 React 数据流里，[props](/cn/docs/components-and-props.md) 是父组件和他们的子元素交互的唯一方式。要修改子元素，你需要用一个新的属性值来重新渲染它。然而，在少数情况下，你需要在常规数据流外强制修改子元素。被修改的子元素可以是 React 组件实例，或是一个 DOM 元素。在这种情况下，React 提供了解决办法。

### 什么时候使用 Refs

这里有一些适合使用 refs 的场景：

* 处理聚焦、文本选择或者媒体播放
* 触发必要的动画
* 和第三方 DOM 库整合

如果可以通过声明式实现，就尽量避免使用 refs 。

例如，在一个 `Dialog` 组件上，通过传递一个 `isOpen` 属性给它来代替暴露 `open()` 和 `close()` 方法。

### 不要过度使用 Refs

的第一个想法可能是在你的应用中使用 refs 来做点事情。如果是这种情况，请花一点时间，更多的关注在组件层中使用 state。在组件层中，通常较高层次的 state 更为清晰。有关示例，请参考[状态提升](/cn/docs/lifting-state-up.md).

### 给 DOM 元素添加一个 Ref

React 支持一个附加在任何组件上的特殊属性。 `ref` 属性接受一个回调函数，并且回调将在组件 装载(mounted) 或者 卸载(unmounted) 之后立即执行。

当给一个 HTML 元素使用 `ref` 属性时， `ref` 接受底层的 DOM 元素作为参数。例如，这段代码使用 `ref` 回调来保存一个 DOM 节点的引用。

```javascript{8,9,19}
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focus = this.focus.bind(this);
  }

  focus() {
    // 使用原生 DOM API 来显式的聚焦文本输入框
    this.textInput.focus();
  }

  render() {
    // 使用 `ref` 回调在实例字段中保存一个文本输入框 DOM 元素的引用（例如，this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focus}
        />
      </div>
    );
  }
}
```

当组件 装载(mounted) 时 React 将用 DOM 元素作为参数调用 `ref` 回调，当它 卸载(unmounted) 时用 `null` 作为参数调用回调。

使用 `ref` 回调只是在类上设置一个属性，这是一种访问 DOM 元素的常用做法。首选的方式是像上面的示例一样在 `ref` 回调里设置一个属性。有一个更短的写法：`ref={input => this.textInput = input}`. 

### 给 类（Class） 组件 添加一个 Ref

当给一个类（Class）声明的自定义组件使用 `ref` 属性时， `ref` 回调接受 装载(mounted) 的组件实例作为它的参数。例如，如果我们想要包装 `CustomTextInput` ，实现组件在装载后立即触发模拟点击：

```javascript{3,9}
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focus();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

注意这仅在 `CustomTextInput` 是用类（class）定义的情况下才生效

```js{1}
class CustomTextInput extends React.Component {
  // ...
}
```

### Refs 和 函数式（Functional）组件

**你可能在函数式（Functional）组件上不需要使用 `ref` 属性**，因为它们没有实例：

```javascript{1,7}
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  render() {
    // 这里将 *不* 生效!
    return (
      <MyFunctionalComponent
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

如果需要使用 ref ，你应该将这个组件转换为类（class），就像你需要生命周期方法和状态时一样。

然而你可以**在函数式（Functional）组件内部使用 `ref` 属性**来引用一个 DOM 元素或者 类(class)组件

```javascript{2,3,6,13}
function CustomTextInput(props) {
  // textInput 必须在这里声明，以便 ref 可以引用到它
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}
```

### 暴露 DOM 引用给父组件

在极少数情况下，你可能要在父组件里使用子元素的 DOM 节点，通常不建议这么做，因为它破坏了组件的封装，但是它偶尔能用来触发聚焦或测量 DOM 子节点的大小或位置。

虽然你能[向子组件添加 ref](#adding-a-ref-to-a-class-component)，但这不是一个理想的解决方案，这样做你只能获取到一个组件实例而不是一个 DOM 节点。此外，在函数式（Functional）组件里这将不生效。

相反，在这种情况下，我们建议在子节点上暴露一个特殊的属性。子节点将会获得一个任意名字的函数属性(例如 `inputRef`)，并将其作为 ref 属性附加到 DOM 节点。这样使得父组件通过中间组件传递它的 ref 回调到子节点的 DOM 节点。

这样将在类(classes)和函数式（functional）组件中都能生效

```javascript{4,13}
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

在上面的示例中，`Parent` 通过 `inputRef` 属性传递它的 ref 回调到 `CustomTextInput`，并且 `CustomTextInput`传递同样的函数作为一个特殊的 `ref` 属性给 `<input>`。最终，在 `Parent` 的 `this.inputElement` 将被设置为跟 `CustomTextInput` 里的 `<input>` 对应的 DOM 节点。

注意在上面示例中 `inputRef` 属性的名字没有特殊的含义，它只是一般的组件属性。然而，使用 `<input>` 本身的 `ref` 很重要，它告诉 React 附加一个 ref 到它的 DOM 节点。

This works even though `CustomTextInput` is a functional component. Unlike the special `ref` attribute which can [only be specified for DOM elements and for class components](#refs-and-functional-components), there are no restrictions on regular component props like `inputRef`.

Another benefit of this pattern is that it works several components deep. For example, imagine `Parent` didn't need that DOM node, but a component that rendered `Parent` (let's call it `Grandparent`) needed access to it. Then we could let the `Grandparent` specify the `inputRef` prop to the `Parent`, and let `Parent` "forward" it to the `CustomTextInput`:

```javascript{4,12,22}
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

function Parent(props) {
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}


class Grandparent extends React.Component {
  render() {
    return (
      <Parent
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

Here, the ref callback is first specified by `Grandparent`. It is passed to the `Parent` as a regular prop called `inputRef`, and the `Parent` passes it to the `CustomTextInput` as a prop too. Finally, the `CustomTextInput` reads the `inputRef` prop and attaches the passed function as a `ref` attribute to the `<input>`. As a result, `this.inputElement` in `Grandparent` will be set to the DOM node corresponding to the `<input>` element in the `CustomTextInput`.

All things considered, we advise against exposing DOM nodes whenever possible, but this can be a useful escape hatch. Note that this approach requires you to add some code to the child component. If you have absolutely no control over the child component implementation, your last option is to use [`findDOMNode()`](/react/docs/react-dom.html#finddomnode), but it is discouraged.

### Legacy API: String Refs

If you worked with React before, you might be familiar with an older API where the `ref` attribute is a string, like `"textInput"`, and the DOM node is accessed as `this.refs.textInput`. We advise against it because string refs have [some issues](https://github.com/facebook/react/pull/8333#issuecomment-271648615), are considered legacy, and **are likely to be removed in one of the future releases**. If you're currently using `this.refs.textInput` to access refs, we recommend the callback pattern instead.

### Caveats

If the `ref` callback is defined as an inline function, it will get called twice during updates, first with `null` and then again with the DOM element. This is because a new instance of the function is created with each render, so React needs to clear the old ref and set up the new one. You can avoid this by defining the `ref` callback as a bound method on the class, but note that it shouldn't matter in most cases.

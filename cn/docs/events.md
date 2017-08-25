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
* [**`合成事件（SyntheticEvent）`**](/cn/docs/events.md)


#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

# 合成事件（SyntheticEvent）

这个参考指南记录了 `SyntheticEvent` 包装器，这个组成 React 事件系统的一部分。查阅 [事件处理指南](/cn/docs/handling-events.md) 了解更多。

## 概述

你的事件处理器将被传递 `SyntheticEvent` 的实例, 一个跨浏览器原生事件包装器。它具有原生浏览器相同的事件接口，包含 `stopPropagation()` 和 `preventDefault()`, 除了这些，事件在所有浏览器中的工作相同。

如果你发现为了某些原因你需要底层的浏览器事件，只需要使用 `nativeEvent` 属性来获取它。每个 `SyntheticEvent` 对象都有下面的属性：

```javascript
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
DOMEventTarget target
number timeStamp
string type
```

> 注意:
>
> 从 v0.14 起, 从一个事件处理器返回 `false` 将不再停止事件传播。相反，`e.stopPropagation()` 或 `e.preventDefault()` 应该视情况手动触发。

### 事件池

`SyntheticEvent` 是通过合并得到的，这意味着在事件回调被调用后，`SyntheticEvent` 对象将被重用并且所有属性都将被取消。
这是出于性能原因。 因此，您无法以异步方式访问该事件。

```javascript
function onClick(event) {
  console.log(event); // => 无效的对象.
  console.log(event.type); // => "click"
  const eventType = event.type; // => "click"

  setTimeout(function() {
    console.log(event.type); // => null
    console.log(eventType); // => "click"
  }, 0);

  // 不生效， this.state.clickEvent 将只包含 null 值
  this.setState({clickEvent: event});

  // 你仍然可以导出事件属性
  this.setState({eventType: event.type});
}
```

> 注意:
>
> 如果要以异步方式访问事件属性，应该对事件调用 `event.persist()` ，这将从池中删除合成事件，并允许用户代码保留对事件的引用。

## 支持的事件

React将事件规范化，以便它们在不同的浏览器中具有一致的属性。

下面的事件处理程序由冒泡阶段中的事件触发。 要为捕获阶段注册事件处理程序，请将 `Capture` 附加到事件名称中; 例如，您可以使用 `onClickCapture` 来处理捕获阶段中的点击事件，而不是使用 `onClick` 。

- [Clipboard Events](#clipboard-events)
- [Composition Events](#composition-events)
- [Keyboard Events](#keyboard-events)
- [Focus Events](#focus-events)
- [Form Events](#form-events)
- [Mouse Events](#mouse-events)
- [Selection Events](#selection-events)
- [Touch Events](#touch-events)
- [UI Events](#ui-events)
- [Wheel Events](#wheel-events)
- [Media Events](#media-events)
- [Image Events](#image-events)
- [Animation Events](#animation-events)
- [Transition Events](#transition-events)
- [Other Events](#other-events)

* * *

## 参考

### 剪贴板事件(Clipboard Events)

事件名称：

```
onCopy onCut onPaste
```

属性：

```javascript
DOMDataTransfer clipboardData
```

* * *

### 合成事件(Composition Events)

事件名称：

```
onCompositionEnd onCompositionStart onCompositionUpdate
```

属性：

```javascript
string data

```

* * *

### 键盘事件(Keyboard Events)

事件名称：

```
onKeyDown onKeyPress onKeyUp
```

属性：

```javascript
boolean altKey
number charCode
boolean ctrlKey
boolean getModifierState(key)
string key
number keyCode
string locale
number location
boolean metaKey
boolean repeat
boolean shiftKey
number which
```

* * *

### 焦点事件(Focus Events)

事件名称：

```
onFocus onBlur
```

这些焦点事件工作在 React DOM 中所有的元素上 ，不仅是表单元素。

属性：

```javascript
DOMEventTarget relatedTarget
```

* * *

### 表单事件(Form Events)

事件名称：

```
onChange onInput onSubmit
```

有关onChange事件的更多信息，请参阅 [Forms](/cn/docs/forms.md).

* * *

### 鼠标事件(Mouse Events)

事件名称：

```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp
```

 `onMouseEnter` 和 `onMouseLeave` 事件从离开的元素传播到正在进入的元素，而不是普通的冒泡，并且没有捕获阶段。

属性：

```javascript
boolean altKey
number button
number buttons
number clientX
number clientY
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
number pageX
number pageY
DOMEventTarget relatedTarget
number screenX
number screenY
boolean shiftKey
```

* * *

### 选择事件(Selection Events)

事件名称：

```
onSelect
```

* * *

### 触摸事件(Touch Events)

事件名称：

```
onTouchCancel onTouchEnd onTouchMove onTouchStart
```

属性：

```javascript
boolean altKey
DOMTouchList changedTouches
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
boolean shiftKey
DOMTouchList targetTouches
DOMTouchList touches
```

* * *

### UI事件(UI Events)

事件名称：

```
onScroll
```

属性：

```javascript
number detail
DOMAbstractView view
```

* * *

### 滚轮事件(Wheel Events)

事件名称：

```
onWheel
```

属性：

```javascript
number deltaMode
number deltaX
number deltaY
number deltaZ
```

* * *

### 媒体事件(Media Events)

事件名称：

```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted
onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay
onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend
onTimeUpdate onVolumeChange onWaiting
```

* * *

### 图像事件(Image Events)

事件名称：

```
onLoad onError
```

* * *

### 动画事件(Animation Events)

事件名称：

```
onAnimationStart onAnimationEnd onAnimationIteration
```

属性：

```javascript
string animationName
string pseudoElement
float elapsedTime
```

* * *

### 转换事件(Transition Events)

事件名称：

```
onTransitionEnd
```

属性：

```javascript
string propertyName
string pseudoElement
float elapsedTime
```

* * *

### 其他事件(Other Events)

事件名称：

```
onToggle
```

---

* 上一篇：[DOM 元素](/cn/docs/dom-elements.md)
* 下一篇：[如何贡献](/cn/contributing/how-to-contribute.md)

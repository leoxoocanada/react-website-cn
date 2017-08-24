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
* [**`DOM 元素`**](/cn/docs/dom-elements.md)
* [合成事件（SyntheticEvent）](/cn/docs/events.md)

#### 贡献

* [如何贡献](/cn/contributing/how-to-contribute.md)
* [代码库概述](/cn/contributing/codebase-overview.md)
* [实现说明](/cn/contributing/implementation-notes.md)
* [设计原则](/cn/contributing/design-principles.md)


</details>

# DOM 元素

React 实现了与浏览器无关的DOM系统，以实现性能和跨浏览器的兼容性。 我们借此机会在浏览器DOM实现中清理了一些粗糙的实现。

在React中，所有DOM属性和属性（包括事件处理程序）都应该是驼峰式命名。 例如，HTML属性 `tabindex` 对应于React中的 `tabIndex` 属性。 例外的情况是 `aria-*` 和 `data-*` 属性，它们应该是小写的。

## 属性差异

React和HTML之间有很多属性有所不同：

### checked

`checked` 属性由 `checkbox` 或 `radio` 类型的 `<input>` 组件支持。您可以使用它来设置组件是否选中。这对于构建受控组件很有用。`defaultChecked` 等价于不受控组件，它设置组件是否在首次挂载时检查。

### className

要指定一个CSS类，请使用`className`属性。 这适用于所有常规的DOM和SVG元素，如`<div>`，`<a>`等。

如果您使用 Web Components 的 React（这不常见），请改用`class`属性。

### dangerouslySetInnerHTML

`dangerouslySetInnerHTML` 是React在浏览器DOM中使用 `innerHTML` 的替代品。 一般来说，从代码设置HTML是有风险的，因为它很容易无意中暴露您的用户到[跨站点脚本(XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) 攻击。 因此，您可以直接从React设置HTML，但是您必须输入 `dangerouslySetInnerHTML` 并传递一个带有 `__html` 键的对象，以提醒自己这是危险的。 例如：

```js
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

### htmlFor

由于`for`是JavaScript中的保留字，React元素使用`htmlFor`代替。

### onChange

`onChange` 事件的行为与您期望的一样：每当一个表单字段被更改时，这个事件被触发。 我们故意不使用现有的浏览器行为，因为 `onChange` 对其行为的描述是不正确的，而 React 依赖此事件来实时处理用户输入。

### selected

`selected' 属性由 `<option>` 组件支持。 您可以使用它来设置是否选中组件。 这对于构建受控组件很有用。

### style

`style` 属性接受一个带有驼峰属性而不是一个CSS字符串的JavaScript对象。 这与DOM `style` 的JavaScript属性一致，但效率更高，并且可以防止XSS的安全漏洞。 例如：

```js
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

请注意，样式不是自动加前缀的。要支持旧版浏览器，您需要提供相应的样式属性：

```js
const divStyle = {
  WebkitTransition: 'all', // 注意这里大写的 'W' 
  msTransition: 'all' // 'ms' 是唯一的小写字母供应商前缀
};

function ComponentWithTransition() {
  return <div style={divStyle}>This should work cross-browser</div>;
}
```

Style key 是驼峰式命名，以便与从JS访问DOM节点上的属性（例如`node.style.backgroundImage`）保持一致。 供应商前缀 [`ms` 除外](http://www.andismith.com/blog/2012/02/modernizr-prefixed/) 应以大写字母开头。这就是为什么`WebkitTransition`有一个大写字母 "W"。

### suppressContentEditableWarning

Normally, there is a warning when an element with children is also marked as `contentEditable`, because it won't work. This attribute suppresses that warning. Don't use this unless you are building a library like [Draft.js](https://facebook.github.io/draft-js/) that manages `contentEditable` manually.

### value

The `value` attribute is supported by `<input>` and `<textarea>` components. You can use it to set the value of the component. This is useful for building controlled components. `defaultValue` is the uncontrolled equivalent, which sets the value of the component when it is first mounted.

## All Supported HTML Attributes

React supports all `data-*` and `aria-*` attributes as well as these attributes:

```
accept acceptCharset accessKey action allowFullScreen allowTransparency alt
async autoComplete autoFocus autoPlay capture cellPadding cellSpacing challenge
charSet checked cite classID className colSpan cols content contentEditable
contextMenu controls coords crossOrigin data dateTime default defer dir
disabled download draggable encType form formAction formEncType formMethod
formNoValidate formTarget frameBorder headers height hidden high href hrefLang
htmlFor httpEquiv icon id inputMode integrity is keyParams keyType kind label
lang list loop low manifest marginHeight marginWidth max maxLength media
mediaGroup method min minLength multiple muted name noValidate nonce open
optimum pattern placeholder poster preload profile radioGroup readOnly rel
required reversed role rowSpan rows sandbox scope scoped scrolling seamless
selected shape size sizes span spellCheck src srcDoc srcLang srcSet start step
style summary tabIndex target title type useMap value width wmode wrap
```

These RDFa attributes are supported (several RDFa attributes overlap with standard HTML attributes and thus are excluded from this list):

```
about datatype inlist prefix property resource typeof vocab
```

In addition, the following non-standard attributes are supported:

- `autoCapitalize autoCorrect` for Mobile Safari.
- `color` for `<link rel="mask-icon" />` in Safari.
- `itemProp itemScope itemType itemRef itemID` for [HTML5 microdata](http://schema.org/docs/gs.html).
- `security` for older versions of Internet Explorer.
- `unselectable` for Internet Explorer.
- `results autoSave` for WebKit/Blink input fields of type `search`.

## All Supported SVG Attributes

React supports these SVG attributes:

```
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan

```


---

* 上一篇：[ReactDOMServer](/cn/docs/react-dom-server.md)
* 下一篇：[合成事件（SyntheticEvent）](/cn/docs/events.md)

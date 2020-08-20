# 组件化

前端架构主要包含架构模式和组件化。

从编码角度来看，声明式方式更直观。组件化就是设计了一套方法，能让开发者用声明式方式（比如 HTML）写组件。

## 组件化实现方式

当前，组件化的实现方式主要有两种：

1. class 形式。本项目主要讲这种实现。
2. hooks 形式

## 组件组成

对象的组成包括：

- Properties
- Methods
- Inherit

组件一般用对象来抽象表示，而且额外增加了：

- Attributes
- Config
- State
- Event
- Lifecycle
- Children

![一个 Component 的组成图](./images/whole-component.png)

```javascript
class Component {
  // Config
  constructor(config) {
    // State
    this.state = {};
  }

  // Properties
  set props1() {}
  get props1() {}

  // Attributes
  setAttribute(key, value) {}
  getAttribute(key) {}

  // Children
  setChildren(){}
  getChildren(){}
}

// Attributes
<Component attr1="v">
  {/* Children*/}
  <div></div>
</Component>;
```

### Attributes 和 Properties

Attributes 强调描述性，一般使用标记语言（HTML 等）、模板语言（JSX 等）表示。我们可以把它当作 property 的默认值。当然，就像 JavaScript 可以操纵 DOM 一样， Attribute 也可以使用 JavaScript 来操纵。伪代码为：

```javascript
// set attribute
<myComponent a="v" />;
myComponent.setAttribute("a", "value");

// get attribute
myComponent.getAttribute("a");
```

Properties 强调从属关系，是对象的一部分。一般使用 JavaScript 来操纵。页面上展示的实际上是 property 的内容。伪代码为：

```javascript
// set property
myComponent.a = "value";
```

Attributes 和 Properties 可以保持一致，也可以保持不一致，由具体 API 层面决定。比如，在 React 的 API 采用了保持一致的设计思路；但是，DOM API 采取了另一种方式。

```html
<input value="cute" />

<script>
  let input = document.getElementsByTagName("input")[0];
  input.value; // cute 访问 property
  input.getAttribute("value"); // cute 访问 attribute

  input.value = "hello"; // 更改 property。注意，这里并不会去更改 attribute
  input.value; // hello 访问 property
  input.getAttribute("value"); // cute 访问 attribute
</script>
```

### Methods

Method 是对象拥有的方法。用户可以调用方法来改变组件。

### Inherit

表示组件的继承关系。但是很多时候，我们会采用组合的方式，而不是采用继承方式。

### Config

表示组件的配置。比如一些组件全局设置、环境变量等。

### State

表示组件内部的状态，只能由用户输入来改变。

### Event

表示组件的事件。

### Lifecycle

![lifecycle](./images/lifecycle.png)

### Children

children 分为 Content 型和 Template 型。

```javascript
// Content 型
<my-button><img src="{{icon}}"/>>{{title}}</my-button>

// Template 型。下图中，使用data-xxx 生成多个 li
<my-list data-xxx>
    <li><img src="{{icon}}"/>>{title}}
</my-list>
```

## 如何设计组件状态

|           | Markup Set | JavaScript Set | JavaScript Change | User Input Change |
| --------- | ---------- | -------------- | ----------------- | ----------------- |
| property  | ×          | √              | √                 | ?                 |
| attribute | √          | √              | √                 | ?                 |
| state     | ×          | ×              | ×                 | √                 |
| config    | ×          | √              | ×                 | ×                 |

## webpack 和 babel

webpack 用于打包 web 资源，使浏览器能调用 web 资源。babel 将新的 JavaScript 语法转化为浏览器可识别的语法。

## JSX

JSX 是 Facebook 发明的类 XML 的 ECMA Script 扩展。它不一定要结合 React 使用，可以单独拿出来给开发者使用。

## webpack-dev-server

webpack-dev-server 是一款实时热加载工具，减少了用户在开发过程中反复手动打命令编译的烦恼。

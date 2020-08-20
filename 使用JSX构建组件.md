# 使用 JSX 构建组件

## 大写组件的绘制

大写 Div 的时候，createElement 收到的第一个参数是 function。

```javascript
// 组件调用
let component = (
  <Div
    name="frank"
    style="width:200px; height: 100px; background-color: red;"
  ></Div>
);
component.mountTo(document.body);

// 组件定义
class Div {
  constructor() {
    this.root = document.createElement("div");
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  mountTo(parent) {
    parent.appendChild(this.root);
  }
}

// createElement 定义
function createElement(Cls, attributes) {
  let o = new Cls();

  for (let attr in attributes) {
    o.setAttribute(attr, attributes[attr]);
  }

  return o;
}
```

## 小写组件的绘制

由于小写 div 的时候，createElement 收到的第一个参数是 string，所以需要特别解决：引入一个 Wrapper 类。

```javascript
// 组件调用
let component = (
  <div
    name="frank"
    style="width:200px; height: 100px; background-color: red;"
  ></div>
);
component.mountTo(document.body);

// Wrapper 定义
class Wrapper {
  constructor(type) {
    this.root = document.createElement(type);
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  mountTo(parent) {
    parent.appendChild(this.root);
  }
}

// createElement 定义
function createElement(Cls, attributes) {
  let o;
  if (typeof Cls === "string") {
    o = new Wrapper(Cls);
  } else {
    o = new Cls();
  }

  for (let attr in attributes) {
    o.setAttribute(attr, attributes[attr]);
  }

  return o;
}
```

## 组件树的绘制

组件树按照先序、深度的方式渲染对应的 HTML Element，也就是说，渲染流程如下：

```text
let component = (
  <Large> // 5
    <Middle></Middle> // 1
    <Middle> // 3
      <Small></Small> // 2
    </Middle>
    <Middle></Middle> // 4
  </Large>
);
```

1. 渲染第一个 Middle。 createElement(f Middle, null)，生成 Middle {}
2. 渲染第一个 Small。 createElement(f Small, null)，生成 Small {}
3. 渲染第二个 Middle。 createElement(f Middle, null, Small {})，其中 Small {} 是上一步生成的，生成 Middle {}
4. 渲染第三个 Middle。 createElement(f Middle, null)，生成 Middle {}
5. 渲染第一个 Large。 createElement(f Large, null, Middle {}, Middle {}, Middle {})，其中三个 Middle {} 是前面生成的，生成 Large {}

基于上述步骤，在类中引入 children。

```javascript
// 组件调用
let component = (
  <Div
    id="large1"
    style="width:200px; height: 200px; background-color: purple;"
  >
    <Div
      id="middle1"
      style="width:20px; height: 20px; background-color: blue;"
    ></Div>
    <Div
      id="middle2"
      style="width:20px; height: 20px; background-color: yellow;"
    >
      <Div
        id="small1"
        style="width:10px; height: 10px; background-color: red;"
      ></Div>
    </Div>
    <Div
      id="middle3"
      style="width:20px; height: 20px; background-color: green;"
    ></Div>
  </Div>
);
component.mountTo(document.body);

// 组件定义
// 大写
class Div {
  constructor() {
    this.children = [];
    this.root = document.createElement("div");
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  appendChild(child) {
    this.children.push(child);
  }

  mountTo(parent) {
    // 本组件的 children 挂在到本组件
    for (let child of this.children) {
      child.mountTo(this.root);
    }

    // 将本组件挂在到它的父亲上
    parent.appendChild(this.root);
  }
}
// 小写
class Wrapper {
  constructor(type) {
    this.children = [];
    this.root = document.createElement(type);
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  appendChild(child) {
    this.children.push(child);
  }

  mountTo(parent) {
    // 本组件的 children 挂在到本组件
    for (let child of this.children) {
      child.mountTo(this.root);
    }

    // 将本组件挂在到它的父亲上
    parent.appendChild(this.root);
  }
}

// createElement 定义
function createElement(Cls, attributes, ...children) {
  let o;
  if (typeof Cls === "string") {
    o = new Wrapper(Cls);
  } else {
    o = new Cls();
  }

  for (let attr in attributes) {
    o.setAttribute(attr, attributes[attr]);
  }

  for (let child of children) {
    o.appendChild(child);
  }

  return o;
}
```

## text 的绘制

```javascript
// 组件调用
let component = (
  <Div name="frank" style="width:200px; height: 100px; background-color: red;">
    hello world
    <div>forza juventus</div>
  </Div>
); // JSX 解析到 forza juventus 时，forza juventus 是 div 的 child， 但是，它不会调用 createElement
component.mountTo(document.body);

// 组件定义
// 大写
class Div {
  constructor() {
    this.children = [];
    this.root = document.createElement("div");
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  appendChild(child) {
    this.children.push(child);
  }

  mountTo(parent) {
    // 本组件的 children 挂在到本组件
    for (let child of this.children) {
      if (typeof child === "string") {
        child = new Text(child);
      }
      child.mountTo(this.root);
    }

    // 将本组件挂在到它的父亲上
    parent.appendChild(this.root);
  }
}
// 小写
class Wrapper {
  constructor(type) {
    this.children = [];
    this.root = document.createElement(type);
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  appendChild(child) {
    this.children.push(child);
  }

  mountTo(parent) {
    // 本组件的 children 挂在到本组件
    for (let child of this.children) {
      if (typeof child === "string") {
        child = new Text(child);
      }
      child.mountTo(this.root);
    }

    // 将本组件挂在到它的父亲上
    parent.appendChild(this.root);
  }
}

// Text 定义
class Text {
  constructor(text) {
    this.root = document.createTextNode(text);
  }

  mountTo(parent) {
    parent.appendChild(this.root);
  }
}

// createElement 定义
function createElement(Cls, attributes, ...children) {
  let o;
  if (typeof Cls === "string") {
    o = new Wrapper(Cls);
  } else {
    o = new Cls();
  }

  for (let attr in attributes) {
    o.setAttribute(attr, attributes[attr]);
  }

  for (let child of children) {
    o.appendChild(child);
  }

  return o;
}
```

## 自定义模板的组件绘制

自定义模板，预留插槽，类似于 React

```javascript
// 组件调用
let component = (
  <div id="outer">
    <MyComponent>
      <div>text text text text</div>
    </MyComponent>
    <div
      id="a"
      class="b"
      style="width:200px; height: 100px; border: solid 1px red;"
    >
      <Div>
        <div>text1</div>
      </Div>
      <p></p>
      <div></div>
      <Div></Div>
    </div>
  </div>
);
component.mountTo(document.body);

// 组件定义
class MyComponent {
  constructor() {
    this.children = [];
  }

  setAttribute(key, value) {
    this.root.setAttribute(key, value);
  }

  appendChild(child) {
    this.children.push(child);
  }

  mountTo(parent) {
    this.slot = <div></div>;
    for (let child of this.children) {
      this.slot.appendChild(child);
    }

    this.render().mountTo(parent);
  }

  render() {
    return (
      <article>
        <header>***This is Header***</header>
        {this.slot}
        <footer>***This is Footer***</footer>
      </article>
    );
  }
}
```

## 总结

综上所述， JSX 自定义组件化的设计思路是：

1. 构建组件类
   组件类分为：

   - 自定义组件类（大写）

     通过 slot 构建

   - 原生组件类
     - Wrapper（小写）
     - Text（文本节点）

2. 组件类中与 DOM 关联
3. DOM 与 HTML 关联

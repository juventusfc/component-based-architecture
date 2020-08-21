# 使用 vanilla-JavaScript 构建 Carousel 组件

我们使用 vanilla-JavaScript 构建一个 Carousel 组件，实现该组件的自动播放以及滑动。

## 搭架子

新建 `index.css`、`index.js` 以及 `index.html`，不引用任何第三方包，当作项目架子。

架子搭好的标准就是运行 `index.html`，能看到一张猫图，但是还没有其他任何功能。

## 增加 slide 自动循环播放功能

为了表达这个过程，我画了下面这个图：

![slides](./images/slides.png)

从上图可知，slide 循环播放，我们需要定义两个关键帧：

1. 第一个关键帧是动画开始前，当前图片和下一张图片的位置需要移动到对的位置
2. 第二个关键帧是动画结束后，当前图片和下一张图片，都需要向左移动 -100%

那么问题已经很明确了，我们只需要找到动画开始前的关键帧：

1. 确定当前图片位置
2. 确定下一张图片位置

很容易的，我们就能得到：

```javascript
currentImg.style.transform = `translateX(${-100 * currentPosition}%)`;
nextImg.style.transform = `translateX(${100 - 100 * nextPosition}%)`;
```

找到每次的两个关键帧后，我们只需要让它们通过 `setTimeout` 循环触发就行了。

## 增加滑动功能

## 缺陷

该项目完成了上述的俩个功能，但是，俩个功能整合起来还有 bug。这个 bug 在这里不修复。会在 JSX 构建 Carousel 组件那里进行修复。

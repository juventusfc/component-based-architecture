# 使用 JSX 构建组件

## JSX 初探，构建项目基本框架

这一部分，会从 JSX 的角度出发，看看 JSX 到底做了什么以及怎么搭建一些简单的组件。具体可参考 [JSX 到底做了什么](./JSX到底做了什么.md)

## 构建 Carousel 组件 -- 不使用 animation 库和 gesture 库

我们可以将使用 vanilla 构建的 Carousel 组件放到 JSX 项目框架中。当然，我们还是需要改造点东西的。

1. 引用 CSS。在 JSX 中引入 CSS，安装 `style-loader` 和 `css-loader` 作为 webpack 的 loader。
2. 使用还未创建的 VanillaCarousel 组件

   ```javascript
   <VanillaCarousel
     data={[
       "https://static001.geekbang.org/resource/image/bb/21/bb38fb7c1073eaee1755f81131f11d21.jpg",
       "https://static001.geekbang.org/resource/image/1b/21/1b809d9a2bdf3ecc481322d7c9223c21.jpg",
       "https://static001.geekbang.org/resource/image/b6/4f/b6d65b2f12646a9fd6b8cb2b020d754f.jpg",
       "https://static001.geekbang.org/resource/image/73/e4/730ea9c393def7975deceb48b3eb6fe4.jpg",
     ]}
   />
   ```

3. 新建 VanillaCarousel 组件。TODO

## 构建 Carousel 组件 -- 使用 animation 库和 gesture 库

## 源码

[点击此处获取源码](https://github.com/juventusfc/jsx-component)

# CSS实现固定宽高比



## 一、可替换元素实现固定宽高比

可替换元素(如`<img />`、`<video>`)和其他元素不同，它们本身有像素宽度和高度的概念。所以如果想实现这一类元素固定宽高比，就比较简单。

我们可以指定其宽度或者高度值，另一边自动计算就可以了。

```css
.img-wrapper {
  width: 50vw;
  margin: 100px auto;
  padding: 10px;
  border: 5px solid lightsalmon;
  font-size: 0;
}
img {
  width: 100%;
  height: auto;
}
```

你可能没注意到，我们给img元素设定了height: auto;，这是为了避免开发者或者内容管理系统在 HTML 源码中给图片添加了height属性，通过这个方式可以覆盖掉，避免出现 bug。

此外，对于video元素也类似，大家可以试下。



## 二、普通元素实现固定宽高比

虽然我们上面实现了可替换元素的固定宽高比，但是这个比例主要是因为可替换元素本身有尺寸，而且这个比例还会受到原有尺寸比例的限制。显然，这并不灵活，那我们该如何针对普通元素实现固定宽高比呢。

首先我们来看看最经典的padding-bottom/padding-top的 hack 方式。

### 2.1 padding-bottom实现普通元素固定宽高比

在之前的陪读章节《精通 CSS》第 3 章 可见格式化模型中，我们提到垂直方向上的内外边距使用百分比做单位时，是基于包含块的宽度来计算的。

下面均以padding-bottom为例

通过借助padding-bottom我们就可以实现一个宽高比例固定的元素，以div为例。

```css
.wrapper {
  width: 40vw;
}
.intrinsic-aspect-ratio-container {
  width: 100%;
  height: 0;
  padding: 0;
  padding-bottom: 75%;
  margin: 50px;
  background-color: lightsalmon;
}
```

如上代码，我们将`div`元素的`height`设为了`0`，通过`padding-bottom`来撑开盒子的高度，实现了4/3的固定宽高比。

这样是实现了固定宽高比，但其只是一个徒有其表的空盒子，里面没有内容。如果想在里面放入内容，我们还需要将`div`内部的内容使用绝对定位来充满固定尺寸的容器元素。

如下：

```css
.intrinsic-aspect-ratio {
  position: relative;
  width: 100%;
  height: 0;
  padding: 0;
  padding-bottom: 75%;
  margin: 50px;
  background-color: lightsalmon;
}
.content {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
```

通过这种方式我们就可以实现一个可以填充内容的固定尺寸的容器了。

此外，尺寸比例，我们也可以通过`calc()`来计算，这样比较灵活。我们可以快速的写出任意比例，如`padding-bottom: calc(33 / 17 * 100%);`。

不知道，你有没有发现，上面的这种方式只能高度随着宽度动，并不能实现宽度随着高度动。

那有没有办法实现宽度随着高度动呢？答案是没有，至少现在没有。但将来就会有了。接下来我们一起看看更简单便捷的另一种方式。

### 2.2 aspect-ratio属性指定元素宽高比

由于固定宽高比的需求存在已久，通过`padding-bottom`来 `hack` 的方式也很不直观，并且需要元素的嵌套。

W3C 的 CSS 工作组为了避免这一问题，已经在致力于实现这样一个属性`aspect-ratio`。该方案已经提出，但是还没有浏览器实现，现在还处于设计节点，暂时还没有已发布的工作组草案，只有编辑草案。

但是这并不妨碍我们来提前了解一下这个新技术。

下面让我们一起来看看是如何的便利吧。

`aspect-ratio`的语法格式如下：`aspect-ratio: <width-ratio>/<height-ratio>`。

如下，我们可以将`width`或`height`设为`auto`，然后指定`aspect-ratio`。另一个值就会按照比例自动变化。

```css
/* 高度随动 */
.box1 {
  width: 100%;
  height: auto;
  aspect-ratio: 16/9;
}
/* 宽度随动 */
.box2 {
  height: 100%;
  width: auto;
  aspect-ratio: 16/9;
}
```

这一技术可以很灵活的实现元素的固定宽高比，并且指定宽高之一均可。只是现在还没有浏览器实现，让我们共同期待吧。



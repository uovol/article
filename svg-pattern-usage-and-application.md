# SVG \<pattern\> 标签的用法和应用场景

通过使用 `<pattern>` 标签，可以在 SVG 图像内部定义可重复使用的任意图案。这些图案可以通过 `fill` 属性或 `stroke` 属性进行引用。

### 使用场景

例如我们要在 `<svg>` 中绘制大量的圆点点，可以通过重复使用 `<circle>` 标签来实现。

```html
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
  <circle cx="10" cy="10" r="2" fill="black" />
  <circle cx="30" cy="10" r="2" fill="black" />
  <circle cx="50" cy="10" r="2" fill="black" />
  <circle cx="70" cy="10" r="2" fill="black" />
  <circle cx="90" cy="10" r="2" fill="black" />

  <circle cx="10" cy="30" r="2" fill="black" />
  <circle cx="30" cy="30" r="2" fill="black" />
  <circle cx="50" cy="30" r="2" fill="black" />
  <circle cx="70" cy="30" r="2" fill="black" />
  <circle cx="90" cy="30" r="2" fill="black" />

  <circle cx="10" cy="50" r="2" fill="black" />
  <circle cx="30" cy="50" r="2" fill="black" />
  <circle cx="50" cy="50" r="2" fill="black" />
  <circle cx="70" cy="50" r="2" fill="black" />
  <circle cx="90" cy="50" r="2" fill="black" />

  <circle cx="10" cy="70" r="2" fill="black" />
  <circle cx="30" cy="70" r="2" fill="black" />
  <circle cx="50" cy="70" r="2" fill="black" />
  <circle cx="70" cy="70" r="2" fill="black" />
  <circle cx="90" cy="70" r="2" fill="black" />

  <circle cx="10" cy="90" r="2" fill="black" />
  <circle cx="30" cy="90" r="2" fill="black" />
  <circle cx="50" cy="90" r="2" fill="black" />
  <circle cx="70" cy="90" r="2" fill="black" />
  <circle cx="90" cy="90" r="2" fill="black" />
</svg>
```

不好的地方在于，这种方法需要为每个点都创建一个 `<circle>` 标签，它们除了坐标不一致之外，其它属性都是相同的，**大量代码都是冗余的**。

这种情况正好就是 `<pattern>` 标签能够大显身手的地方。

### 使用方法

使用 `<pattern>` 标签的基本步骤如下：

1. 在 `<defs>` 标签内定义 `<pattern>`。
2. 通过元素的 `stroke` 或 `fill` 属性引用定义好的图案。

定义 `<pattern>` 最初看起来可能有些复杂，但实际上它仅仅是绘制一些形状或路径而已。你可以把它想象成一个可从外部重复引用的 `<svg>` 标签。

### \<pattern\> 可使用的一些属性

- **viewBox**: 用数值列表指定图案视口边界，默认为 `none`。
- **x**: 以长度或百分比指定图案的X坐标，默认为 `0`。
- **width**: 指定图案宽度，默认为 `0`。
- **height**: 指定图案高度，默认为 `0`。
- **href**: 要重用现有图案时，指定 `id`，默认为 `none`。
- **patternContentUnits**: 指定图案坐标系统，可选值为 `userSpaceOnUse`（SVG坐标）或`objectBoundingBox`（相对于形状），默认为 `userSpaceOnUse`。若设置了 `viewBox`，此属性无效。
- **patternTransform**: 如需对图案应用变换（如旋转 `rotate(45)` ），在此指定，默认为 `none`。
- **patternUnits**: 指定 `x`、`y`、`width`、`height` 值所使用的坐标单位，可选 `userSpaceOnUse` 或 `objectBoundingBox`，默认为 `objectBoundingBox`。
- **preserveAspectRatio**: 定义当图案应用于具有不同长宽比的图形时的处理方式，可选值包括 `none`、`xMinYMin`、`xMidYMin`、`xMaxYMin`、`xMinYMid`、`xMidYMid`、`xMaxYMid`、`xMinYMax`、`xMidYMax`、`xMaxYMax` 等，并可附加 `meet`（保持比例填充）或 `slice`（可截断），默认为 `xMidYMid meet`。

我们再以上面的点状图案为例，使用 `<pattern>` 标签重新实现一次，代码如下：

```html
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern id="dotPattern" width="20" height="20" patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="2" fill="black" />
    </pattern>
  </defs>
  <rect width="100" height="100" fill="url(#dotPattern)" />
</svg>
```

此时代码看起来比上面那一版要简洁多了，尽管坐标计算稍微复杂一些，但这种方式的可读性比上一版要好很多。

### 参考资料

> [Patterns - SVG：可缩放矢量图形 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial/Patterns)

> [<pattern> – SVG: Scalable Vector Graphics | MDN](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/pattern)

# 使用 JS 实现在浏览器控制台打印图片 console.image()

在前端开发过程中，调试的时候，我们会使用 console.log 等方式查看数据。但对于图片来说，**仅靠展示的数据与结构，是无法想象出图片最终呈现的样子的**。

虽然我们可以把图片数据通过 img 标签展示到页面上，或将图片下载下来进行预览。但这样的调试过程实在是复杂，何不实现一个 console.image() 呢？
## 先上演示案例：
[JS 实现浏览器控制台打印图片 console.image()
，在线预览HTML - 笔.COOL](https://bi.cool/bi/W1P1cyq)

（chrome 浏览器上演示效果）
![（chrome 浏览器上演示效果）](/console-image/1.png)


## 实现 console.image()：

参考 Github 上已实现的库 [https://github.com/adriancooney/console.image](https://github.com/adriancooney/console.image) Star 1.8k(本文发布前)。 实现代码如下：

```javascript
// 实现 console.image 函数【注意，url如果是网络图片必须开启了跨域访问才能打印】
console.image = function (url, scale) {
  const img = new Image()
  img.crossOrigin = "anonymous"
  img.onload = () => {
    const c = document.createElement('canvas')
    const ctx = c.getContext('2d')
    if (ctx) {
      c.width = img.width
      c.height = img.height
      ctx.fillStyle = "red";
      ctx.fillRect(0, 0, c.width, c.height);
      ctx.drawImage(img, 0, 0)
      const dataUri = c.toDataURL('image/png')

      console.log(`%c sup?` ,
        `
          font-size: 1px;
          padding: ${Math.floor((img.height * scale) / 2)}px ${Math.floor((img.width * scale) / 2)}px;
          background-image: url(${dataUri});
          background-repeat: no-repeat;
          background-size: ${img.width * scale}px ${img.height * scale}px;
          color: transparent;
        `
      )
    }
  }
  img.src = url
}

```

## 使用方式：

```javascript
// 支持 图片地址【注意，url如果是网络图片则必须开启了跨域访问才能打印图片】
console.image("替换为 图片 url", 0.5);
// 支持 base64
console.image("替换为 base64 字符出", 1);
```

上面仅展示 console.image() 的代码，因为原库还包含 console.meme() 的实现，用于在控制台生成表情包，感兴趣的同学可以去该库查看详情。

该库上一次更新已经将近10年了，由于近些年 Chrome 控制台中工作方式有变更，导致作者原版实现会使图片重复显示一次。 遇到相同问题的人提了 issues，本文章代码已根据 issues 里提供的解决方案进行了修复。

## 实现说明：

console.image() 借助于 console.log 能够**使用 %c 为打印内容定义样式** 的方式进行实现，例如：

```javascript
// 下面的代码将会在控制台打印出带样式的文本
console.log("这是 %c一条带样式的消息", `
    font-style: italic;
    color: cyan;
    background-color: red;
`);
```

下载本案例源码：[https://bi.cool/bi/W1P1cyq](https://bi.cool/bi/W1P1cyq)

> 参考资料 Reference :
> [https://developer.mozilla.org/zh-CN/docs/Web/API/console](https://developer.mozilla.org/zh-CN/docs/Web/API/console)

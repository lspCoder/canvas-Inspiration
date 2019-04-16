canvas实现类似css立体按钮效果，一般情况可能用不到，但是特殊场景也不失一种方案。

### 关键点

[`shadowOffsetX = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetX)

`shadowOffsetX` 和 `shadowOffsetY `用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 `0`。

[`shadowOffsetY = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetY)

shadowOffsetX 和 `shadowOffsetY `用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 `0`。

[`shadowBlur = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowBlur)

shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 `0`。

[`shadowColor = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowColor)

shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。

第一步我们先绘制矩形，绘制矩形我们使用moveTo和lineTo，因为按钮一般是带有圆角的，所以我们封装一个绘制圆角矩形的

```javascript
createPath: function (x, y, width, height, radius) {
    this.ctx.moveTo(x, y + radius);
    this.ctx.lineTo(x, y + height - radius);
    this.ctx.quadraticCurveTo(x, y + height, x + radius, y + height);
    this.ctx.lineTo(x + width - radius, y + height);
    this.ctx.quadraticCurveTo(x + width, y + height, x + width, y + height - radius);
    this.ctx.lineTo(x + width, y + radius);
    this.ctx.quadraticCurveTo(x + width, y, x + width - radius, y);
    this.ctx.lineTo(x + radius, y);
    this.ctx.quadraticCurveTo(x, y, x, y + radius);
},
```

圆角矩形绘制完成后我们在接着绘制阴影，

```javascript
setShadow: function (xoffset, yoffset) {
    var style = this.style;
    this.ctx.shadowOffsetX = xoffset || 0;
    this.ctx.shadowOffsetY = yoffset || 5;
    this.ctx.shadowBlur = 0;
    this.ctx.shadowColor = style.shadowColor;
},
```

阴影绘制完后我们还要绘制文本，毕竟是canvas，绘制文本不像css那么方便，我们需要计算文字宽度来定位，代码如下：

```javascript
 drawText: function () {
     var xoffset = this.ctx.measureText(this.text).width;
     var x = this.x,
         y = this.y;
     if (this.state === 'active') {
         y = y + 5;
     }
     this.ctx.save();
     this.ctx.beginPath();
     this.ctx.font = "30px Micosoft yahei";
     this.ctx.fillStyle = this.fontColor;
     this.ctx.textBaseline = 'middle';
     this.ctx.textAlign = 'center';
     this.ctx.fillText(this.text, x + (this.width - xoffset) / 2 + 10, y + (this.height - 22) / 2 + 5, this.width);
     this.ctx.closePath();
     this.ctx.restore();
 },

```

另附textAlign和textBaseLine的说明：

textAlign：

![](https://i.loli.net/2018/12/08/5c0b8fe927047.png)

textBaseLine：

![textBaseLine](https://i.loli.net/2018/12/08/5c0b90132f21e.png)

这样效果就基本大功告成了

![](https://i.loli.net/2018/12/08/5c0b925c935ee.gif)

canvas事件可以具体看我的代码和我以前的[博客](https://lspcoder.github.io/)


<iframe height="379" style="width: 100%;" scrolling="no" title="canvas绘制btn" src="//codepen.io/lspcoder/embed/moaBXV/?height=379&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lspcoder/pen/moaBXV/'>canvas绘制btn</a> by lspCoder
  (<a href='https://codepen.io/lspcoder'>@lspcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
### 关键点
多重阴影的叠加，这里我们可以类比css多层阴影效果


```javascript
    drawGlowText: function () {
        this.fromGlow();
        this.toGlow();
    },

    fromGlow: function () {
        this.drawText(0, 0, 10, '#FFF');
        this.drawText(0, 0, 20, '#FFF');
        this.drawText(0, 0, 30, '#FFF');
        this.drawText(0, 0, 40, '#00a67c');
        this.drawText(0, 0, 70, '#00a67c');
        this.drawText(0, 0, 80, '#00a67c');
        this.drawText(0, 0, 100, '#00a67c');
        this.drawText(0, 0, 150, '#00a67c');
    },

    toGlow: function () {
        this.drawText(0, 0, 5, '#FFF');
        this.drawText(0, 0, 10, '#FFF');
        this.drawText(0, 0, 15, '#FFF');
        this.drawText(0, 0, 20, '#00a67c');
        this.drawText(0, 0, 35, '#00a67c');
        this.drawText(0, 0, 40, '#00a67c');
        this.drawText(0, 0, 50, '#00a67c');
        this.drawText(0, 0, 75, '#00a67c');
    },

    drawText: function (shadowx, shadowy, blur, shadowColor) {
        var xoffset = this.ctx.measureText(this.text).width;
        var x = this.x,
            y = this.y;
        this.ctx.save();
        this.ctx.beginPath();
        this.setShadow(shadowx, shadowy, blur, shadowColor);
        this.ctx.font = "300px Micosoft yahei";
        this.ctx.fillStyle = this.fontColor;
        this.ctx.textBaseline = 'middle';
        this.ctx.textAlign = 'center';
        this.ctx.fillText(this.text, x + (this.width - xoffset) / 2 + 10, y + (this.height - 22) / 2 + 5, this.width);
        this.ctx.closePath();
        this.ctx.restore();
    }
```


<iframe height="344" style="width: 100%;" scrolling="no" title="GlowText-canvas" src="//codepen.io/lspcoder/embed/moaqyW/?height=344&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lspcoder/pen/moaqyW/'>GlowText-canvas</a> by lspCoder
  (<a href='https://codepen.io/lspcoder'>@lspcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
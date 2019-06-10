# HTML

## 你对语义化的理解

用正确的标签做正确的事情

- 利于开发：使代码**结构清晰**，**可读性高**，**方便维护**
- 利于SEO：方便爬虫根据语义标签确定**页面结构**和**关键字**的权重

### 你在平常工作中使用过哪些语义标签？

header, nav, aside, main, section, article, footer, canvas, video, audio

### 你说你用过canvas/video/audio，你是怎么用它们的？

- 画布：[canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)

   ```js
   <body>
    <canvas width="500" height="500"></canvas>
    <script>
        var canvas = document.querySelector("canvas")
        <!-- 获取上下文 -->
        var ctx = canvas.getContext("2d")
        ctx.beginPath()
        <!-- 设置图形轮廓的颜色 -->
        ctx.strokeStyle = "red"
        <!-- 绘制直线 -->
        ctx.moveTo(10, 20)
        ctx.lineTo(100, 200)
        ctx.stroke()
        </script>
    </body>
   ```

- 媒体元素：[video](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video) 和 [audio](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio)

   ```html
   <video src="视频资源路径" poster="海报资源路径" controls autoplay>
   <audio src="音频资源路径" controls autoplay loop>
   ```

## 解释下移动端为什么要添加 meta viewport ，怎么写？

作用：

防止页面在移动端可以缩放。

写法：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

解释：

- 使用 `viewport` 来控制浏览器视口的宽度和缩放比例
- 添加 `width=device-width` 以便让视口的宽度与设备一致
- 添加 `initial-scale=1` 以便让页面的默认缩放比例与PC端一致

参考阅读：

- **[在移动浏览器中使用viewport元标签控制布局](https://developer.mozilla.org/zh-CN/docs/Mozilla/Mobile/Viewport_meta_tag)**
- **[HTML 具有视口元标记](https://developers.google.cn/web/tools/lighthouse/audits/has-viewport-meta-tag?hl=zh-cn)**
- **[移动端web开发——视口](https://www.cnblogs.com/chunyangji/p/5795487.html)**

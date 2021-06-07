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
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

解释：

- 使用 `viewport` 来控制浏览器视口的宽度和缩放比例
- 添加 `width=device-width` 以便让视口的宽度与设备一致
- 添加 `initial-scale=1.0` 设置初始缩放比例为1.0
- 添加 `user-scalable=no` 使得用户不能放大或缩小网页
   - 添加 `minimum-scale=1.0, maximum-scale=1.0` 是为了兼容 iOS10 不支持 `user-scalable` 导致用户仍可缩放

参考阅读：

- **[在移动浏览器中使用viewport元标签控制布局](https://developer.mozilla.org/zh-CN/docs/Mozilla/Mobile/Viewport_meta_tag)**
- **[HTML 具有视口元标记](https://developers.google.cn/web/tools/lighthouse/audits/has-viewport-meta-tag?hl=zh-cn)**
- **[移动端web开发——视口](https://www.cnblogs.com/chunyangji/p/5795487.html)**

## 关于图片，你了解哪些形式，你觉得哪种场合用哪种？它们优劣如何？然后这些图片的应用场景能说说不？

| 格式     | 特点                                | 使用场景                                     |
| -------- | ----------------------------------- | -------------------------------------------- |
| JPG/JPEG | 有损压缩 体积小 不支持透明          | 1. 大的背景图； 2. 轮播图； 3. Banner 图     |
| PNG      | 无损压缩 质量高 体积大 支持透明     | 1. 小 Logo； 2. 透明背景                     |
| GIF      | 支持动画                            | 动态图片                                     |
| SVG      | 文本文件 体积小 不失真 兼容性好     | 能适应不同设备且画质不能损坏的图片           |
| Base64   | 文本文件 依赖编码 小图标解决方案    | 大小不超过 2KB，且更新率低的图片             |
| webp     | 细节丰富 支持透明 支持动画 兼容性差 | Chrome UC 等浏览器支持，局限性大，一般不考虑 |
| 雪碧图   | 一次加载多处使用                    | 小图太多的时候，集中成一张图片减少 HTTP 请求 |

更多阅读：[面试知识点 - 图片](https://github.com/LiangJunrong/document-library/blob/master/other-library/interview/personal-experience/other-%E5%9B%BE%E7%89%87.md)

## 如何实现图片的懒加载

链接：https://evestorm.github.io/posts/7617/ or [原文](https://segmentfault.com/a/1190000038413073)


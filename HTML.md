Table of Contents
=================

* [HTML](#html)
   * [HTML5 的新特性](#html5-的新特性)
   * [你对 HTML 语义化的理解](#你对-html-语义化的理解)

# HTML

## HTML5 的新特性

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

- 本地存储：[localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage) 长期存储数据，浏览器关闭后数据不丢失; [sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage) 的数据在浏览器关闭后自动删除;
- 语义元素：header, nav, main, section, article, footer; 表单控件：calendar, date, time, email, url, search
- 新技术：[Web Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API), [WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket), [Geolocation](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation)

## 你对 HTML 语义化的理解

- 使代码**结构清晰**，**方便阅读**
- 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以语义的方式来渲染网页
- 利于搜索引擎优化（SEO），使搜索引擎爬虫能更好的确定网页上下文和各个关键字的权重


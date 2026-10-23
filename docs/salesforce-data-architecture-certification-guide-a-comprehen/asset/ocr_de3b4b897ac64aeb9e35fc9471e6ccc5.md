# 基于视觉大模型的智能图文转换

输入一张图片，自动提取图片中的文字信息，识别文档结构和排版布局，以 Markdown 格式输出。

支持丰富的文档元素：

+   - 段落（paragraph）
+   - 标题（title）
+   - 列表（list）
+   - 表格（table）
+   - 引用（quote）
+   - 插图（image）
+   - 代码块（code）

输出 JSON 格式，包含元素类型、Markdown 内容和相对坐标（bbox）。

> > 注意：bbox 使用相对坐标，即图片宽度和高度的比例值。

```
{
  "direction": "horizontal|vertical",
  "contents": [
    {
      "type": "paragraph|title|list|...",
      "markdown": "in markdown format",
      "bbox": [0.xmin, 0.ymin, 0.xmax, 0.ymax]
    }
  ]
}
```

适用于扫描件、截图、照片等多种图像场景，可广泛用于文档电子化、信息提取与知识管理。
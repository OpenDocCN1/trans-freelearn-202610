

## 绘制椭圆

我们可以在图像上绘制线条或其他几何形状（例如，使用 PIL.ImageDraw 模块中的 ellipse() 函数绘制椭圆），如下面的 Python 代码片段所示：

```
im = Image.open("images/parrot.png")
draw = ImageDraw.Draw(im)
draw.ellipse((125, 125, 200, 250), fill=(255,255,255,128))
del draw
im.show()
```

## 绘制边界框

Image.getbbox() 方法计算包含二进制图像中所有非零（或非黑色）像素的最小边界框。边界框以 4 元组形式返回：(left, upper, right, lower)。下面的代码加载了一张灰度马图像并使用 getbbox() 来定位对象区域，然后使用 ImageDraw.Draw() 和 draw.rectangle() 在检测到的边界框周围叠加一条宽度为 3 的红色矩形轮廓。del draw 在绘制操作完成后删除绘图上下文对象以释放资源和内存。现在，请参考下面的代码块：

```
from PIL import ImageDraw
im = Image.open('images/horse.png').convert('L')
im = im.convert('RGB')
draw = ImageDraw.Draw(im)
draw.rectangle(im.getbbox(), fill = None, outline ="red", width = 3)
del draw
im
```

如果运行上述代码片段，你应该会得到如下图所示的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_124_0.png)

Figure 2.32 – Drawing a bounding box around an object with PIL

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_126_0.png)

Figure 2.34 – Iteratively pasting shrunk images with PIL

## 计算图像的基础统计信息

我们可以使用 stat 模块来计算图像的基础统计信息（不同通道像素值的均值、中位数、标准差等），如下面的代码片段所示：

```
s = stat.Stat(im)
print(s.extrema) # maximum and minimum pixel values for each channel R, G, B
print(s.count, s.mean, s.median, s.stddev)
```

## 绘制像素值的直方图

histogram() 函数可用于计算每个通道像素的直方图（像素值与频率的表格）并返回拼接后的输出（例如，对于 RGB 图像，输出包含 3 x 256 = 768 个值）。下图显示了通过运行代码片段绘制的 RGB 颜色直方图：

```
pl = im.histogram()
plt.bar(range(256), pl[:256], color='r', alpha=0.5)
plt.bar(range(256), pl[256:2*256], color='g', alpha=0.4)
plt.bar(range(256), pl[2*256:], color='b', alpha=0.3)
```

如果运行上述代码片段，你应该会得到如下图所示的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_126_1.png)

Figure 2.35 – Histogram of the RGB color channels of the parrot image

## 分离图像的 RGB 通道

我们可以使用 `split()` 函数来分离多通道图像的通道，如下为 RGB 图像的代码所示：

```
```python
ch_r, ch_g, ch_b = im.split() # split RGB image into 3 channels: R, G and B
plt.figure(figsize=(18,6))
plt.subplot(1,3,1); plt.imshow(ch_r, cmap=plt.cm.Reds); plt.axis('off')
plt.subplot(1,3,2); plt.imshow(ch_g, cmap=plt.cm.Greens); plt.axis('off')
plt.subplot(1,3,3); plt.imshow(ch_b, cmap=plt.cm.Blues); plt.axis('off')
# show the R, G, B channels separately
```
```

如果运行上述代码片段，你应该会得到如下图所示的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_127_0.png)

*Figure 2.36 – Extracting the red, green, and blue channels of the parrot image with PIL*

上图显示了通过运行前述代码片段为每个 RGB 通道创建的三个输出图像。

## 合并图像的多个通道

我们可以使用 `merge()` 函数来合并多通道图像的通道，如下代码所示，其中在交换红色和蓝色通道后，将通过拆分鹦鹉 RGB 图像获得的颜色通道合并：

```
```python
im = Image.merge('RGB', (ch_b, ch_g, ch_r)) # swap the red and blue channels
im.show()
```
```

下图显示了通过运行前述代码片段合并通道后创建的 RGB 输出图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_127_1.png)

*Figure 2.37 – Color-channel swapping with PIL*

## Alpha (α) 通道和图像混合

图像中的 alpha 通道是一个额外的通道，表示每个像素的透明度或不透明度。它常用于具有不同透明度级别的图像中，可实现图像与其背景或其他图像之间的平滑混合。

Alpha 通道是 RGBA 颜色模型不可分割的一部分，其中每个像素由四个分量表示：红色、绿色、蓝色和 alpha。PNG 图像格式支持 alpha 通道，使其成为需要透明度的图像的热门选择。另一方面，JPEG 不支持 alpha 通道。

Alpha 通道允许图像具有像素级透明度，从而可以在图像和背景之间创建平滑过渡。

Alpha 值为 0 表示完全透明，意味着该像素完全不可见。Alpha 值为 255（或浮点图像的 1.0）表示完全不透明，使像素完全可见。

## 将 JPG 转换为透明 PNG 图像

可以通过添加 alpha 通道使用 `convert("RGBA")`、从原始图像生成灰度遮罩并使用 `putalpha()` 分配透明度值，将 JPG 图像转换为透明 PNG。在示例中，`ImageChops.invert()` 反转灰度遮罩，使较暗区域变为透明而较亮区域保持可见，之后将图像保存为支持透明度的 PNG 文件。下面的代码片段读取一张黑白鸟的 JPG 图像并将其保存为透明 PNG 图像：

```
```python
from PIL import Image, ImageChops

img_trans = Image.new('RGBA', img.size)
alpha = Image.open('images/birds.jpg').convert('L')
img_trans.putalpha(ImageChops.invert(alpha))
img_trans.save('images/bird.png')
img_trans
```
```

下图显示了通过运行前述代码片段创建的输出图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_128_0.png)

*Figure 2.38 – Creating a transparent PNG image with PIL*

下面的代码片段读取鹦鹉 RGB 彩色 JPG 图像，提取对应于红色羽毛的区域并将其保存为透明 PNG，如下图所示：

```
img = Image.open('images/parrot.jpg') # n x m x 3
imga = img.convert("RGBA")  # n x m x 4

imga = np.asarray(imga)
r, g, b, a = np.rollaxis(imga, axis=-1) # split into 4 n x m arrays
r_m = r > 100 # binary mask for red channel, True for read feathers
g_m = g < 100 # binary mask for green channel, True for read feathers
b_m = b < 200 # binary mask for blue channel, True for read feathers

# combine the three masks using the binary "or" operation
# multiply the combined binary mask with the alpha channel
a = a * ((r_m == 1) & (g_m == 1) & (b_m == 1))

# stack the img back together
imga = Image.fromarray(np.dstack([r, g, b, a]), 'RGBA')
imga.save('images/parrot_transparent.png')

print(img.mode, imga.mode)
# RGB RGBA
```

如果运行上述代码片段，你应该会得到如下图所示的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_129_0.png)

Figure 2.39 – Creating transparent PNG image from a JPG image

## α-混合两幅图像

blend() 函数可用于通过使用常数 α 在两幅给定图像（大小相同）之间进行插值来创建新图像。两幅图像必须具有相同的大小和模式。输出图像由下式给出：

out = image₁.(1.0 − α) + image₂.α

如果 α 为 0.0，则返回第一幅图像的副本。如果 α 为 1.0，则返回第二幅图像的副本。

下面的代码片段显示了一个示例：

```
im1 = Image.open("images/parrot.png")
im2 = Image.open("images/hill.jpg")
# if two images have different modes, must be converted to the same mode
im2 = im2.resize((im1.width, im1.height), Image.BILINEAR)
# if two images have different sizes, must be converted to the same size
im = Image.blend(im1, im2, alpha=0.5).show()
```

下图显示了通过运行前述代码片段混合前两幅图像生成的输出图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_130_0.png)

Figure 2.40 – alpha-blending two images with PIL

## α-合成两幅图像

α-合成使用两幅图像的 alpha（透明度）值来混合两幅图像，决定每幅图像对最终像素颜色的贡献程度。给定一幅前景图像 F 和一幅背景图像 B，每幅图像均具有 RGBA 值，结果图像 C 使用 alpha 混合计算得出。以下是计算步骤的说明：

- 1. Alpha 归一化：首先，将前景 α_F 和背景 α_B 的 alpha 值归一化到 0 到 1 之间：α_F' = α_F / 255 和 α_B' = α_B / 255
- 2. 合成 alpha 计算：结果 alpha α_C（最终图像的透明度）计算为：α_C = α_F' + α_B'(1 − α_F')。
- 3. 混合颜色计算：对于每个颜色通道（R、G 和 B），最终合成颜色 C 计算如下：
- C = (F · α_F' + B · α_B'(1 − α_F')) / α_C

这确保前景根据透明度与背景混合。下面的 Python 代码片段通过使用 PIL.Image 中的方法，使用不同的透明度（alpha）值对两幅图像（鱼和海）进行 α-合成。以下是关键步骤：

- 1. 设置透明度：putalpha(100) 使鱼更透明，而 putalpha(200) 使海更不透明。


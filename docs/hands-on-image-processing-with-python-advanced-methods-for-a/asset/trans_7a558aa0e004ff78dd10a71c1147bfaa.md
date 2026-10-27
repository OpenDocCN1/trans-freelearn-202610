
我们将在*第 9 章*中回顾阈值处理。

## 随机半色调（随机抖动）

在阈值处理（二值量化）中，减少假轮廓伪影的一种方法是在量化之前向输入图像添加均匀分布的白噪声。具体来说，对于灰度图像 $f(x,y)$ 的每个输入像素，我们添加一个独立的均匀 $[-128,128]$ 随机数（可以表示为一个独立的均匀随机变量 $n(x,y)\sim U(-\Delta,\Delta)$），然后应用阈值处理。这可以表示为：

$$g(x,y)=1(f(x,y)+n(x,y)\geq T)$$

其中 $1(\cdot)$ 表示指示函数，若条件为真则等于 1，否则为 0；T 是阈值，$\Delta$ 控制噪声幅度（例如，对于 8 位图像，$\Delta=128$）。

这种技术通常被称为**随机抖动**，可视为**半色调**的一种简单形式，其中噪声用于打破大片均匀区域，并通过空间变化模拟中间灰度级。

与强制将所有像素变成非黑即白的严格区域（这会产生条带效应）不同，添加的噪声会使相邻像素随机跨越阈值翻转。在一定距离外观看时，这些随机图案会被人类视觉系统感知为平滑的灰度级，从而减少可见的轮廓伪影。

下面的代码块给出了一个实现：

```
im = Image.open('images/swans.jpg').convert('L')
im = Image.fromarray(np.clip(im + np.random.randint(-128, 128, \
    (im.height, im.width)), 0, 255).astype(np.uint8))
threshold = [0, 50, 100, 150, 200]
for i in range(2, 5):
    im_bin = im.point(lambda x: x > threshold[i])
    # plot binary image obtained with this threshold value here, refer
    github
```

如果运行上述代码片段并绘制所得到的二值图像，应得到下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_316_0.png)

*图 7.8 – 使用阈值处理得到的二值图像*

请注意，在上图中，尽管生成的二值图像有些噪声，但伪轮廓已显著减少；在足够的模糊下（例如，从远处观看时），它们会给我们一种具有多个灰度级的印象。

## 抖动

虽然随机噪声添加（如半色调中讨论的）能减少可见的条带效应，但其方式是非结构化的。更一般地，**抖动**是指在量化之前或量化过程中刻意修改像素值的技术，目的是使量化误差在空间上分布，而不是集中在大片均匀区域。这种重新分布有助于打破诸如伪轮廓之类的人为图案，并产生中间灰度级的视觉错觉。抖动可以通过不同方式实现，包括**随机抖动**、**有序抖动**和**误差扩散方法**，每种方法在视觉质量和计算复杂度之间提供了不同的权衡。

## 带误差扩散的 Floyd–Steinberg 抖动

Floyd–Steinberg 算法通过在扫描方向上向前扩散量化误差来执行抖动，确保局部误差由后续像素决策来补偿。它把一个像素的残余量化误差推（加）到其相邻像素上，留待后续处理。它按照下图所示的相邻像素分布，将量化误差扩散出去：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_316_1.png)

*图 7.9 – 使用 Floyd-Steinberg 误差扩散抖动传播量化误差*

在上图中，当前像素用星号（*）表示，空白像素表示先前已扫描的像素。算法从左到右、从上到下扫描图像。它逐一连续量化像素值；每次量化误差都会分配到相邻像素（尚未扫描）中，同时不影响已经量化的像素。如果多个像素被向下取整，那么算法就更有可能将接下来的像素向上取整，从而使平均量化误差接近于零。

令 $f(x,y) \in [0,1]$ 表示输入灰度图像，$g(x,y) \in \{0,1\}$ 表示二值化输出。

在每个像素 $(x,y)$ 处，我们定义 $\tilde{f}(x,y)=f(x,y)+e(x,y)$，其中 $e(x,y)$ 是从先前已处理像素传播过来的累积误差。

关键步骤如下：

- **量化步骤：** 将该像素量化为最近的调色板值（对于二值图像，阈值为 $T=0.5$ 时）：
  
  $$g(x,y)=1\bigl(\tilde{f}(x,y)\ge T\bigr)$$

- **量化误差：** 在 $(x,y)$ 处的量化误差为：
  
  $$\varepsilon(x,y)=\tilde{f}(x,y)-g(x,y)$$

- **误差扩散：** 将该误差按照固定权重分布到相邻的未处理像素：
  
  $$e(x+1,y)\mathrel{+}=\frac{7}{16}\varepsilon(x,y),\quad e(x-1,y+1)\mathrel{+}=\frac{3}{16}\varepsilon(x,y)$$
  $$e(x,y+1)\mathrel{+}=\frac{5}{16}\varepsilon(x,y),\quad e(x,y+1)\mathrel{+}=\frac{1}{16}\varepsilon(x,y)$$

- **守恒性质：** 这些权重满足 $\frac{7}{16}+\frac{3}{16}+\frac{5}{16}+\frac{1}{16}=1$，确保总量化误差被守恒并重新分布，而不是丢失。

与引入*随机噪声*的随机抖动不同，Floyd–Steinberg 使用确定性的、结构化的误差重新分布，从而：

- 更好地保留图像结构
- 降低噪声观感
- 获得更高的感知质量

接下来，Python 代码实现了 FS 抖动。下面列出了代码所执行的关键任务：

1. **颜色量化（find_closest_palette_color）：** 该函数将像素映射到一组精简灰度级中最接近的值。对于 num_colors = k，该函数创建一个调色板，其中的灰度级在 0 到 255 之间等步长分布：$step = \frac{255}{k-1}$。因此，像素强度会被取整到这 k 个灰度级中最接近的一个。
2. **抖动（get_dithered_image）：** 算法访问每个像素，并将其量化为最近的调色板灰度级。它计算量化误差：
   
   $error = original - quantized$
   
   然后根据固定权重将误差分布到相邻像素，如伪代码所示。这种误差扩散即使图像仅限于 k 个灰度级，也能产生中间强度的错觉。
3. **最终量化与裁剪：** 误差扩散后，图像再次被精确量化为 k 个灰度级。像素值被裁剪到 [0, 255] 并转换为 uint8。
4. **验证：** 代码使用 np.unique(dithered) 打印抖动图像中的唯一灰度级。这确认了输出符合预期的 k 个调色板灰度级。

现在，请参考下面的代码片段：

```
def find_closest_palette_color(old_pixel, num_colors=2):
    step = 255 / (num_colors - 1)
    return np.round(old_pixel / step) * step

def get_dithered_image(image, num_colors):
    dithered = np.copy(image)
    h, w = dithered.shape
    for x in range(h):  # Floyd-Steinberg Dithering
        for y in range(w):
            old_pixel = dithered[x, y]
            new_pixel = find_closest_palette_color(old_pixel, num_colors)
            dithered[x, y] = new_pixel
            quant_error = old_pixel - new_pixel
            if y + 1 < w:
                dithered[x, y + 1] += quant_error * 7 / 16
```

```python
if x + 1 < h and y > 0:
    dithered[x + 1, y - 1] += quant_error * 3 / 16
if x + 1 < h:
    dithered[x + 1, y] += quant_error * 5 / 16
if x + 1 < h and y + 1 < w:
    dithered[x + 1, y + 1] += quant_error * 1 / 16
return find_closest_palette_color(dithered, num_colors)

image = 255 * rgb2gray(imread('images/lena.jpg')[..., :3])
image = image.astype(np.float32)
dithered2 = get_dithered_image(image, 2)
dithered4 = get_dithered_image(image, 4)
# Display the original and dithered images side-by-side here
```

如果运行上述代码片段，应得到下图。该图显示，与之前的半色调方法相比，所获得的二值图像（使用 2 个灰度级）的质量有了极大改善。当在抖动中使用 4 个灰度级时，可以注意到质量进一步提高。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_319_0.png)

*图 7.10 – Floyd-Steinberg 抖动*

总结如下：

- 算法按**因果扫描顺序**（从左到右、从上到下）处理像素。
- 已经量化的像素保持不变。
- 后续像素会**受到先前误差的影响**，从而促使进行补偿。如果较早的像素被向下取整，后续像素往往会被向上取整。这保持了**局部平均强度**，从而产生没有条带效应的视觉平滑结果。


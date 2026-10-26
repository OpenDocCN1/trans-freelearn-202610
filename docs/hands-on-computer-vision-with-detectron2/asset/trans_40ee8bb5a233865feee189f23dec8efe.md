

以下代码片段创建了一些帮助程序，用于可视化 Transformation 和 Augmentation 类的输入和输出。以下是显示图像原始尺寸的帮助方法：

```python
# some import statements are removed for space efficiency
def imshow(image):
    dpi = plt.rcParams["figure.dpi"]
    im_data = image[:, :, ::-1]
    height, width, depth = im_data.shape
    figsize = width / float(dpi), height / float(dpi)
    fig = plt.figure(figsize=figsize)
    plt.imshow(im_data)
    plt.axis("off")
    plt.show()
```

以下方法（visualize_image_boxes）可在输入图像上可视化边界框：

```python
def visualize_image_boxes(img, boxes, colors=['k']):
    v = Visualizer(img, metadata={})
    v = v.overlay_instances(
        boxes           = boxes,
        assigned_colors = colors*len(boxes)
    )
    im_data = v.get_image()
    imshow(im_data)
```

最后，以下方法（visualize_sample）将字典（sample）作为输入，加载图像和边界框，并利用之前构建的方法对它们进行可视化：

```python
def visualize_sample(sample):
    img = sample['image'].to("cpu").numpy()
    img = np.moveaxis(img, 0, -1)
    boxes = sample['instances'].get('gt_boxes')
    visualize_image_boxes(img, boxes)
```

这些方法有助于以图像的实际尺寸可视化 Detectron2 数据集中提取的数据样本及其对应的边界框。

- **NoOpTransform 类**也是 `Transform` 类的子类。它的实例用于诸如在光度失真类别的变换类中返回 `inverse()` 方法等场景。它什么也不做。具体而言，`apply_image` 和 `apply_coords` 抽象方法的实现返回其对应的输入，而 `inverse` 方法的实现返回此类自身的当前对象（`self`）。

- **ResizeTransform 类**是最常用的变换之一，因为其他变换和深度学习模型通常要求图像大小相同。相比之下，输入图像可能具有不同的尺寸。此变换接受原始图像高度和宽度（`h`、`w`）、期望高度和宽度（`new_h`、`new_w`）以及插值方法（`interp`），并据此执行缩放。

- **ExtentTransform 类**：此类接收输入图像的一个子区域（`src_rect`，采用 `XYXY_ABS` 格式）、期望输出尺寸（`h`、`w`）以及插值方法（`interp`），并使用给定的插值方法将提取的区域调整为期望尺寸。此外，如果任何输入子区域尺寸大于输入图像，则使用填充颜色（`fill` 输入参数）进行填充。以下代码片段说明了此类的用法：

```python
# data
img = cv2.imread("Chapter08/1.jpg")
boxes = [[150, 140, 350, 420]]
visualize_image_boxes(img, boxes = boxes, colors=['k'])
# transform
e_tfm = T.ExtentTransform(
        src_rect = (100, 100, 400, 470),
        output_size = img.shape[:2]
    )
et_img = e_tfm.apply_image(img)
et_boxes = e_tfm.apply_box(boxes)
visualize_image_boxes(et_img, boxes = et_boxes)
```

此片段加载一个带有真实框的示例图像。然后，它创建一个 ExtentTransform 对象并调用 `apply_image` 和 `apply_box` 来相应地转换输入图像和真实框。*图 8.5* 显示了此代码片段的输出：

```python
Y_range = np.array(Y_range)
X, Y = np.meshgrid(X_range, Y_range)
grid = np.expand_dims(np.stack([X, Y], axis=2), axis=0)
grid = grid.astype('float32')
super().__init__(grid=grid, interp="nearest")
self.X_range = X_range
self.Y_range = Y_range
```

此方法（__init__）为从 pi 到 0 等距分布的角度生成余弦值范围，宽度为 441 点，高度为 543 点。然后将它们用于创建网格并初始化其超类的对象（GridSampleTransform）。此外，请注意，范围为 [np.pi, 0] 的余弦波返回 [-1, 1] 的范围，这正是 GridSampleTransform 类输入参数所期望的。

GridSampleTransform 有助于转换输入图像。然而，它不支持 apply_coords 方法。因此，我们需要编写 apply_box 方法来执行框变换。此 apply_box 方法利用 _apply_1_box 对其所有输入框执行变换。_apply_1_box 方法是一个内部帮助方法，用于对单个边界框执行变换。它包含以下代码：

```python
gs_box = np.array(box)
img_width = self.img_w
img_height = self.img_h
X_range = self.X_range
Y_range = self.Y_range
x0x1 = 2*gs_box[[0, 2]]/img_width - 1
y0y1 = 2*gs_box[[1, 3]]/img_height - 1
x0x1 = [X_range.searchsorted(x0x1[0]),
        X_range.searchsorted(x0x1[1])]
y0y1 = [Y_range.searchsorted(y0y1[0]),
        Y_range.searchsorted(y0y1[1])]
gs_box = [min(x0x1), min(y0y1), max(x0x1), max(y0y1)]
return gs_box
```

此方法（`_apply_1_box`）找到左上角（x0, y0）和右下角（x1, y1）的 X 和 Y 坐标，并按图像的宽度和高度进行归一化，坐标中心位于图像中心。归一化确保图像的左上角具有 [-1, -1] 坐标，图像的右下角具有 [1, 1] 坐标，并且原点 [0, 0] 位于图像中心。此归一化反映了图像坐标值在转换为余弦值后的情况。最后，它使用 searchsorted 方法根据余弦坐标找到像素坐标。

现在，我们可以像迄今为止学到的其他类一样使用此类：

```python
fc_tfm = PincushionTransform(*img.shape[:2])
fc_img = fc_tfm.apply_image(img)
fc_boxes = fc_tfm.apply_box(boxes)
visualize_image_boxes(fc_img, boxes = fc_boxes)
```

*图 8.8* 显示了此失真方法（右）的输出与其原始输入（左）的对比：

![](img/0e7994c850a84b7a53d34c7f943c7edd_192_0.png)

原始图像和真实框

枕形图像和真实框

图 8.8：原始图像和真实框及其枕形版本

![](img/0e7994c850a84b7a53d34c7f943c7edd_194_0.png)

图 8.9：源图像、目标图像及其混合版本

- **PadTransform 类**用 `pad_value`（默认为 0.0）和 `seg_pad_value`（默认为 0）分别在输入图像和分割掩码（如果有）的左侧、顶部、右侧和底部填充 x0、y0、x1 和 y1 像素。此外，如果你想使此变换可逆，可以指定原始宽度和高度（`orig_w` 和 `orig_h`）。

- **TransformList 类**扩展了 `Transform` 类，因此具有执行变换操作的类似方法。它接受输入参数作为变换列表（`transforms`）并将其作为属性维护。其方法按顺序将此列表中的所有变换应用于输入。例如，`TransformList` 对象的 `transforms` 属性中有一个 `CropTransform` 对象和一个 `PadTransform` 对象。调用此对象的 `apply_image` 方法首先裁剪图像，然后填充它。此场景有助于训练鲁棒模型以检测超出其特定内容的物体。例如，从哈士奇身上裁剪出雪景背景，迫使模型从哈士奇本身学习特征，而不是其背景上下文。

以下代码片段说明了如何使用 `CropTransform`、`PadTransform` 和 `TransformList` 类：

```python
x0, y0, w, h = (130, 130, 250, 310)
y1, x1 = img.shape[:2] - np.array([y0+h, x0+w])
cr_tfm = T.CropTransform(x0, y0, w, h)
pd_tfm = T.PadTransform(x0, y0, x1, y1)
lst_tfm = T.TransformList([cr_tfm, pd_tfm])
lst_img = lst_tfm.apply_image(img)
lst_boxes = lst_tfm.apply_box(boxes)
visualize_image_boxes(lst_img, boxes = lst_boxes)
```



```
X, Y = (x - cx) * z/fx, (y - cy) * z/fy
points.append([X, Y, z])
colors.append(cmap[seg[y, x]] / 255.0)  # color_val
pcd = o3d.geometry.PointCloud()
pcd.points = o3d.utility.Vector3dVector(np.array(points))
pcd.colors = o3d.utility.Vector3dVector(np.array(colors))
return pcd, points, colors

img_rgb = cv2.cvtColor(cv2.imread('cats.jpg'), cv2.COLOR_BGR2RGB)
depth, seg = get_depth(img_rgb), get_segmentation(img_rgb)
pcd_semantic, points, colors = depth_seg_to_pointcloud(img_rgb, depth, seg)
pcd_semantic = pcd_semantic.translate(-pcd_semantic.get_center())
o3d.visualization.draw_plotly([pcd_semantic])
```

如果你运行上述代码，你应该会得到类似下面的图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_562_0.png)

*图 11.7 – 基于语义融合的语义三维场景理解*

基于Transformer的深度估计使从单目RGB图像中获得密集的几何理解成为可能。通过结合深度预测、语义分割和几何投影，现代深度学习系统可以从普通照片中重建具有语义意义的三维场景。这将二维感知、几何、语义、空间推理等统一到一个可微分的单一流程中，用于下一代场景理解系统。

## 图像分割的实际应用

虽然现代分割模型在数学上很优雅，但它们真正的影响在于实际部署，从移动应用和视频通话到自动驾驶系统。在本节中，我们将探讨实用的、面向生产的流程，展示分割模型如何用于实时系统和大规模框架中。

## 使用 MediaPipe + DeepLabV3 (TFLite) 实现背景虚化

该应用演示了使用适用于移动和边缘设备的轻量级模型进行实时前景-背景分离。系统使用MediaPipe和DeepLabV3的TensorFlow Lite版本来高效执行语义分割。给定图像 I(x, y)，模型预测一个类别掩码：

S(x, y) ∈ {0,1}

其中 S(x, y) = 1 → 前景（人物/物体），S(x, y) = 0 → 背景。然后我们应用：

I_final(x, y) = { I(x, y), 如果 S(x, y) = 1; Blur(I(x, y)), 其他情况 }

这会产生类似视频会议工具的人像背景虚化效果。

下一个代码片段演示了使用MediaPipe的高级视觉任务API实现背景虚化。一个加载了DeepLabV3 TFLite模型的vision.ImageSegmenter执行逐像素语义分割，以生成区分前景和背景的类别掩码。该掩码被阈值化为二值掩码，背景使用高斯滤波进行模糊处理，最终图像通过将清晰前景与模糊背景混合而成。生成的流程随后通过一个简单的Gradio界面公开，允许用户上传图像并交互式查看原始图像、分割掩码和背景虚化结果。

```
# run in colab
from mediapipe.tasks.python import vision
base_options = python.BaseOptions(model_asset_path="deeplabv3.tflite")
options = vision.ImageSegmenterOptions(
    base_options=base_options, output_category_mask=True)
segmenter = vision.ImageSegmenter.create_from_options(options)

def segment_and_blur(img):
    mp_image = mp.Image(image_format=mp.ImageFormat.SRGB, data=img)
    result = segmenter.segment(mp_image)
    mask = result.category_mask.numpy_view().squeeze()
    binary_mask = (mask > 0.2).astype(np.uint8) * 255
    binary_mask_rgb = np.stack([binary_mask]*3, axis=-1)
    blurred = cv2.GaussianBlur(img, (55, 55), 0)
    condition = mask > 0.1
    final = np.where(condition[..., None], img, blurred)
    return img, binary_mask_rgb, final

with gr.Blocks() as demo:
    gr.Markdown("## DeepLabV3 Background Blur (TFLite + MediaPipe)")
    with gr.Row():
        input_img = gr.Image(type="numpy", label="Upload Image")
```

```
image = cv2.imread("dogs_cats.jpg")
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

# INSTANCE SEGMENTATION (Mask R-CNN)
cfg_ins = get_cfg()
cfg_ins.merge_from_file(model_zoo.get_config_file(
    "COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x.yaml"))
cfg_ins.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(
    "COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x.yaml")
cfg_ins.MODEL.DEVICE = device
predictor_ins = DefaultPredictor(cfg_ins)
outputs_ins = predictor_ins(image)
v_ins = Visualizer(
    image_rgb, MetadataCatalog.get(cfg_ins.DATASETS.TRAIN[0]), scale=1.2)
ins_result = v_ins.draw_instance_predictions(outputs_ins["instances"].to("cpu"))

# PANOPTIC SEGMENTATION (FPN)
cfg_pan = get_cfg()
cfg_pan.merge_from_file(model_zoo.get_config_file(
    "COCO-PanopticSegmentation/panoptic_fpn_R_101_3x.yaml"))
cfg_pan.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(
    "COCO-PanopticSegmentation/panoptic_fpn_R_101_3x.yaml")
cfg_pan.MODEL.DEVICE = device
predictor_pan = DefaultPredictor(cfg_pan)
panoptic_seg, segments_info = predictor_pan(image)["panoptic_seg"]
metadata = MetadataCatalog.get(cfg_pan.DATASETS.TRAIN[0])
v_pan = Visualizer(image_rgb, metadata, scale=1.2)
pan_result = v_pan.draw_panoptic_seg_predictions(
    panoptic_seg.to("cpu"), segments_info)

# SEMANTIC FROM PANOPTIC
panoptic_np = panoptic_seg.to("cpu").numpy()
semantic_map = np.zeros_like(panoptic_np)
for segment in segments_info: semantic_map[panoptic_np == segment["id"]] = segment["category_id"]
v_sem = Visualizer(image_rgb, metadata, scale=1.2, instance_mode=ColorMode.SEGMENTATION)
sem_result = v_sem.draw_sem_seg(torch.tensor(semantic_map))
# Plot image_rgb, sem_result, ins_result, pan_result side-by-side here
```

高效的数据加载通过tf.data.Dataset实现，支持批处理、缓存、预取和并行处理。预处理流程加载图像和掩码，将强度归一化到[0,1]，并使用最近邻插值调整大小，同时保持标签完整性。

U-Net编码器反复应用卷积、ReLU激活和最大池化。池化逐步降低空间分辨率，同时增加语义抽象。跳跃连接将编码器和解码器特征拼接起来：

D_l = Concat(U_l, E_l),

从而保留精细的边界信息。解码器使用Conv2DTranspose进行可学习的上采样，并重建详细的分割掩码。最后的1×1卷积充当逐像素分类器，将特征通道映射为类别logits：

Z ∈ ℝ^{H×W×C},

随后进行softmax概率估计：

P(c | x, y) = e^{z_c} / Σ_k e^{z_k}

下图展示了U-Net架构：

## U-Net 架构

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_567_0.png)

图 11.10 – U-Net 架构

模型使用`SparseCategoricalCrossentropy(from_logits=True)`和Adam优化器进行训练。分割质量使用平均IoU评估：

$$IoU = \frac{TP}{TP + FP + FN}$$

自定义的`MeanIoUWrapper`在指标计算之前使用`tf.argmax()`将logits转换为离散标签。在训练期间，前向传播计算预测，评估损失，通过反向传播获得梯度，并迭代更新参数。完整代码请参阅本书附带的GitHub仓库。

最后，训练好的模型使用`unet.to_json()`和`save_weights()`进行序列化，然后使用`model_from_json()`重建，以便将来部署和推理。函数`show_predictions()`并排可视化输入图像、真实掩码和预测分割，用于定性评估：

```
```python
# run in colab
path = kagglehub.dataset_download("kumaresanmanickavelu/lyft-udacity-challenge")
from tensorflow.keras.layers import Input, Conv2D, MaxPooling2D, \\
                                 Dropout, Conv2DTranspose, concatenate

train_image_list = sorted(glob('lyft-udacity-challenge/dataA/dataA/CameraRGB/*.png'))
train_mask_list = sorted(glob('lyft-udacity-challenge/dataA/dataA/CameraSeg/*.png'))
test_image_list = sorted(glob('lyft-udacity-challenge/dataA/dataA/CameraRGB/*.png'))
test_mask_list = sorted(glob('lyft-udacity-challenge/dataA/dataA/CameraSeg/*.png'))
train_dataset=tf.data.Dataset.from_tensor_slices((train_image_list,train_mask_list))
test_dataset = tf.data.Dataset.from_tensor_slices((test_images_list,test_masks_list))

def conv_block(inputs=None, n_filters=32, dropout_prob=0, max_pooling=True):
    conv = Conv2D(n_filters, kernel_size = 3, activation='relu',
                  padding='same',
                  kernel_initializer=tf.keras.initializers.HeNormal())(inputs)
    conv = Conv2D(n_filters, kernel_size = 3, activation='relu',
                  padding='same',
                  kernel_initializer=tf.keras.initializers.HeNormal())(conv)
    if dropout_prob > 0: conv = Dropout(dropout_prob)(conv)
    next_layer = MaxPooling2D(pool_size=(2,2))(conv) if max_pooling else conv
    skip_connection = conv
```
```



producing one 4096-dimensional representation per image.

**Applying t-SNE to CNN Features:** Once the deep features are obtained, t-SNE is applied.

```
tsne = TSNE(n_components=2)
embedding = tsne.fit_transform(features)
```

The resulting coordinates are visualized using color-coded animal categories. Unlike raw pixels, VGG16 features encode high-level semantic information learned from millions of images. Consequently, |f_i − f_j| becomes a meaningful measure of visual similarity. As explained earlier, images of dogs tend to occupy nearby regions of feature space, while elephants and butterflies appear farther apart. The resulting t-SNE plot often reveals well-separated clusters corresponding to animal categories despite the network never being trained explicitly on the Animal-10 dataset. This phenomenon demonstrates the remarkable transferability of deep convolutional representations.

**Thumbnail Visualization:** An even more informative visualization overlays actual image thumbnails on the t-SNE embedding with AnnotationBbox(). Instead of merely observing colored points, the user can directly inspect which images occupy neighboring locations. Such visualizations provide a powerful tool for dataset exploration, outlier detection, feature debugging, transfer-learning analysis, and embedding quality assessment.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_601_0.png)

Figure 12.2 – t-SNE 2D manifold visualization of samples from Animal-10 dataset

While t-SNE visualizes feature embeddings, **explainable AI** techniques such as *activation-map* visualization reveal what CNN layers learn, motivating the CNN Brain Viewer Dashboard developed next.

## CNN brain viewer dashboard

A common criticism of deep learning systems is their apparent lack of interpretability. Although convolutional neural networks achieve remarkable classification accuracy, understanding what information is captured by intermediate layers remains challenging. One useful strategy is to visualize activation maps generated throughout the network.

**From Edges to Semantics:** CNNs learn hierarchical representations:

| Layer Depth | Typical Features Learned |
|---|---|
| Early layers | edges, corners |
| Intermediate layers | textures, patterns |
| Deep layers | object parts |
| Final layers | semantic concepts |

*Table 12.1: Hierarchical representations with CNNs*

This hierarchy resembles processing in the human visual cortex, where increasingly abstract representations emerge in deeper regions.

**ResNet18 Feature Visualization:** The dashboard uses a pretrained ResNet18 model. Forward hooks are attached to selected layers.

```
def hook_fn(name):
    def hook(module, input, output):
        activations[name] = output.detach()
    return hook

model = models.resnet18(weights=models.ResNet18_Weights.DEFAULT).to(DEVICE)
model.eval()
activations = {} # hooks storage
model.conv1.register_forward_hook(...) # register layers
model.layer1.register_forward_hook(...)
model.layer2.register_forward_hook(...)
model.layer3.register_forward_hook(...)
model.layer4.register_forward_hook(...)
```

Forward hooks provide a non-invasive way to capture intermediate activations from pretrained networks without altering their architecture or retraining the model, making feature visualization simple and reliable. These hooks capture intermediate feature tensors during inference.

For a feature tensor $A \in \mathbb{R}^{C\times H\times W}$ a simple activation map is computed by averaging across channels:

$$M(x,y)=\frac{1}{C}\sum_{c=1}^{C}A_c(x,y),$$

yielding a coarse representation of the network’s attention.

**Heatmap Overlay:** The activation map is normalized:

$$M' = \frac{M-\min(M)}{\max(M)-\min(M)}$$

and superimposed onto the original image.

```
overlay_img = 0.5 * image + 0.5 * heatmap
```

The resulting visualization highlights image regions that strongly activate a particular CNN layer.

### Interactive exploration with Gradio

The dashboard exposes an interactive interface.

```
demo = gr.Interface(  # GRADIO UI
    fn=cnn_brain_view,
    inputs=[gr.Image(type="pil", label="Upload Image"),
            gr.Slider(0, 4, step=1, value=0,
                      label="CNN Layer (0=Early → 4=Deep)")],
    outputs=[gr.Image(label="Input"),
             gr.Image(label="Activation Heatmap"),
             gr.Image(label="Overlay (Model Attention)")],
    title="CNN Brain Viewer (ResNet18)",
    description="Visualize how CNNs transform images from edges → textures →
semantic object parts")
demo.launch()
```

allowing users to select different network depths and observe how representations evolve. The CNN-brain-viewer dashboard looks like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_604_0.png)

Figure 12.3 – CNN brain viewer Gradio app (with ResNet18)

A typical observation is: Conv1 focuses on edges and boundaries, Layer1 captures local textures, Layer2 captures repeated structures, Layer3 captures object components and Layer4 focuses on semantically meaningful regions.

This section demonstrated three complementary approaches to understanding image representations:

- PCA reveals major directions of variance
- t-SNE uncovers nonlinear cluster structures in high-dimensional spaces
- CNN activation visualization exposes hierarchical feature extraction inside deep neural networks

Together, these techniques provide valuable intuition about how modern image classification systems transform raw pixels into semantically meaningful feature embeddings, forming the foundation for transfer learning, vision transformers, self-supervised learning, image retrieval, and object detection techniques discussed throughout the remainder of this chapter.

## Image classification

Consequently, a network trained on ImageNet can often be reused for a completely different classification problem with only minor modifications.

**The VGG16 Architecture:** VGG16, proposed by Simonyan and Zisserman (2015), consists of 13 convolutional layers followed by 3 fully connected layers. The network accepts an RGB image **x** ∈ ℝ²²⁴×²²⁴×³ and produces posterior probabilities over 1000 ImageNet categories: ŷ = *f*(**x**; θ) where θ denotes the learned network parameters. The original VGG16 architecture contains approximately 138 × 10⁶ trainable parameters, making it one of the earliest deep convolutional networks capable of achieving state-of-the-art ImageNet performance.

**Experimental Dataset:** The *Cats vs Dogs* dataset contains two categories: *Cats* and *Dogs*. The dataset structure is

```
dataset/
├── training_set/
│   ├── cats/
│   └── dogs/
└── test_set/
    ├── cats/
    └── dogs/
```

The experiment uses 8,000 training images and 2,000 test images. Each image is resized to 224 × 224 to match the VGG16 input requirements.

**Data Loading and Preprocessing:** ImageDataGenerator creates efficient input pipelines that automatically load images from disk and apply VGG16-specific preprocessing (mean subtraction and channel normalization). The `flow_from_directory()` function then reads images from the training and test folders, resizes them to (224, 224), assigns binary labels based on subdirectory names (cats and dogs), and generates mini-batches for model training and evaluation.

```
train_datagen = ImageDataGenerator(preprocessing_function=preprocess_input)
train_data = train_datagen.flow_from_directory('dataset/training_set',
            target_size=(IMAGE_SIZE, IMAGE_SIZE), batch_size=BATCH_SIZE,
            class_mode='binary', shuffle=True)
```

### Approach 1: Using a pre-trained VGG16 network

The simplest strategy is to use VGG16 exactly as trained on ImageNet.

```
pretrained_vgg16_model = VGG16(weights='imagenet', include_top=True)
```

Since ImageNet does not contain a single “dog” or “cat” class but instead contains many breeds, the predicted ImageNet labels must be merged into two superclasses. For example,

```
"Persian cat" → cat
"Siamese cat" → cat
```

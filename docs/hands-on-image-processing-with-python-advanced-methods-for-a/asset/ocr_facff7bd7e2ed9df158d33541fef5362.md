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

# Approach 1: Using a pre-trained VGG16 network

The simplest strategy is to use VGG16 exactly as trained on ImageNet.

```
pretrained_vgg16_model = VGG16(weights='imagenet', include_top=True)
```

Since ImageNet does not contain a single “dog” or “cat” class but instead contains many breeds, the predicted ImageNet labels must be merged into two superclasses. For example,

```
"Persian cat" → cat
"Siamese cat" → cat
```
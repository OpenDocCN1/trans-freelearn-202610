In the context of image search, each image (now represented as a vector) is indexed into an Elasticsearch document. This vector can be one per document or multiple vectors per document. The vector representing the image is stored in a dense_vector field within the document. Additionally, other relevant information or metadata about the image can be stored in other fields within the same document.

The full example code can be found in the Jupyter Notebook available in the chapter 5 folder of this book's GitHub repository at https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter5, but we'll discuss the relevant parts here.

First, we will initialize a pre-trained model using the SentenceTransformer library.

The clip-ViT-B-32-multilingual-v1 model is discussed in detail later in this chapter:

```
model = SentenceTransformer('clip-ViT-B-32-multilingual-v1')
```

Next, we will prepare the image transformation function:

```
transform = transforms.Compose([
    transforms.Resize(224),
    transforms.CenterCrop(224),
    lambda image: image.convert("RGB"),
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5)),
])
```

Transforms.Compose() combines all the following transformations:

+   - transforms.Resize(224): Resizes the shorter side of the image to 224 pixels while maintaining the aspect ratio.
+   - transforms.CenterCrop(224): Crops the center of the image so that the resultant image has dimensions of 224x224 pixels.
+   - lambda image: image.convert("RGB"): This is a transformation that converts the image to the RGB format. This is useful for grayscale images or images with an alpha channel, as deep learning models typically expect RGB inputs.
+   - transforms.ToTensor(): Converts the image (in the PIL image format) into a PyTorch tensor. This will change the data from a range of [0, 255] in the PIL image format to a float in a range [0.0, 1.0].
+   - transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5)): Normalizes the tensor image with a given mean and standard deviation for each channel. In this case, the mean and standard deviation for all three channels (R, G, B) are 0.5. This normalization will transform the data range from [0.0, 1.0] to [-1.0, 1.0].
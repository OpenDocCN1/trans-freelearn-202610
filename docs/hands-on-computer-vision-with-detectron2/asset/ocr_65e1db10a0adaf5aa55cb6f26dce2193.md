The following code snippets create some helpers to visualize the inputs and outputs of the Transformation and Augmentation classes. The following is the helper method that shows images in their original size:

```
# some import statements are removed for space efficiency
def imshow(image):
    dpi = plt.rcParams["figure.dpi"]
    im_data = image[:, :, ::-1]
    height, width, depth = im_data.shape
    figsize = width / float(dpi), height / float(dpi)
    fig = plt.figure(figsize=figsize)
    plt.imshow(im_data)
    plt.imshow(im_data)
    plt.axis("off")
    plt.show()
```

The following method (visualize_image_boxes) visualizes bounding boxes on input images:

```
def visualize_image_boxes(img, boxes, colors=['k']):
    v = Visualizer(img, metadata={})
    v = v.overlay_instances(
        boxes           = boxes,
        assigned_colors = colors*len(boxes)
    )
    im_data = v.get_image()
    imshow(im_data)
```

Finally, the following method (visualize_sample) gets a dictionary (sample) as input, loads images and bounding boxes, and visualizes them while leveraging the previously built method:

```
def visualize_sample(sample):
    img = sample['image'].to("cpu").numpy()
    img = np.moveaxis(img, 0, -1)
    boxes = sample['instances'].get('gt_boxes')
    visualize_image_boxes(img, boxes)
```

These methods help visualize the data sample in the image's actual size, along with its corresponding bounding boxes, with data extracted from the Detectron2 dataset.
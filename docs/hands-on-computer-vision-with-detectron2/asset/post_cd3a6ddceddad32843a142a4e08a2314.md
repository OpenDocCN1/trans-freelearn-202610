
This code snippet randomly selects a value from the specified list of lengths and then resizes the shorter side of the input to this sampled length. If the longer side of the output is larger than 1,333, it continues to resize the image (while keeping the aspect ratio) to ensure that it does not exceed this number.

The RandomCrop and CategoryAreaConstraint classes take crop_type, crop_size, single_category_max_area, and ignored_category as input parameters. The first two parameters are similar to those in the RandomCrop class. However, this class searches for a cropping area in which no single category occupies a ratio larger than the value specified for single_category_max_area for semantic segmentation. This value is specified in 0 to 1 as ratios (set to 1.0 to disable this feature). The last parameter sets the category's id, which does not have to follow this constraint. Note that this function applies a best-effort strategy. It only tries, at most, 10 times to achieve the specified conditions. The following code snippet illustrates the usage pattern of this class:

```python
cac_aug = T.RandomCrop_CategoryAreaConstraint(
    crop_type                = "relative",
    crop_size                = (0.5, 0.5),
    single_category_max_area = 0.5,
    ignored_category         = None
)
sem_seg = np.zeros(img.shape[:2])
cac_tfm = cac_aug.get_transform(img, sem_seg=sem_seg)
cac_img = cac_tfm.apply_image(img)
cac_boxes = cac_tfm.apply_box(boxes)
visualize_image_boxes(cac_img, cac_boxes)
```

Note that besides the input image, the sem_seg parameter of the get_transform method is required because this class is used for semantic segmentation. Thus, this code snippet creates a fake semantic segmentation mask that only contains all zeros for illustration purposes.

The MinIoURandomCrop class takes min_ious, boxes, min_crop_size, mode_trials, and crop_trials as input parameters. This augmentation attempts to perform a random crop with output that meets the IoU requirement for the input image (controlled by min_crop_size) and bounding boxes (controlled by min_ious). The min_crop_size parameter is in the ratio (ranging from 0 to 1) to the input height and width (H, W). This class works as follows for the maximum mode_trials times, where the cropping region is (top, left, new_h, new_w):

- I. Sample for a min_iou value from [0, *min_ious, 1].
- II. For the maximum crop_trials times, do the following:
  - i. Sample two values from (min_crop_size*W, W) and (min_crop_size*H, H) to generate new_h and new_w. It keeps resampling if the crop size ratio is outside the range [0.5, 2.0] or the cropping region becomes a line or point so that it doesn’t distort the input too much.
  - ii. Once there’s a new size, it samples the top and left values from [H - new_h] and [W - new_w]. If the cropping region has an IoU with any ground-truth boxes smaller than min_iou, continue to Step II.

The following code snippet illustrates the usage pattern of this class:

```python
iou_aug = T.MinIoURandomCrop(
    min_ious     = [0.5, 0.6, 0.7],
    min_crop_size = 0.5,
    mode_trials   = 1000,
    crop_trials   = 50
)
iou_tfm = iou_aug.get_transform(img, boxes=boxes)
iou_img = iou_tfm.apply_image(img)
iou_boxes = iou_tfm.apply_box(boxes)
visualize_image_boxes(iou_img, iou_boxes)
```

Note that besides the input image, the get_transform method requires the ground-truth boxes since this augmentation works with bounding boxes. This augmentation is important because it keeps the ground-truth boxes while trying to remove parts of the external contexts. This technique enables training models that focus on learning features from the objects instead of their contexts.

Note that we may need to normalize the input image or choose a high value for `scale`. The default eigenvectors and values are for RGB images, while this snippet works with BGR images. Therefore, this snippet keeps reversing the color channels back and forth.

**The AugmentationList class** takes a list of transformations or augmentations (augs) as an input parameter. This class helps apply changes (augs) to the input sequentially. This class does not implement the `get_transform()` method; instead, it utilizes the `__call__` method to apply the augmentations or transformations to the input wrapped inside AugInput.

The following section provides further details about AugInput and its usage pattern.

## The AugInput class

This special class encapsulates data for inputs to augmentation objects. Specifically, an instance of this class includes attributes for the input image (`image`) and related annotations, including bounding boxes (`boxes`) and semantic segmentations (`sem_seg`). This class has a method called `transform(tfm: Transform)` that takes the input parameter as a Transform object (`tfm`) and performs the corresponding transformation on all of its attributes (`image`, `boxes`, and sem_seg) in place. If required, users can define a similar class with additional attributes to be transformed by providing attribute access (`getattr`) and a corresponding transform method to perform additional transformations on the attributes in place and return a Transform object.

Despite having the `transform` method, which can be used by itself, the main usage pattern of this class is to be used as the input to the `__call__` method of an Augmentation object. In this case, the `transform` operation of the Augmentation class changes the input in place and returns an object of a Transform class to perform augmentation on other inputs if needed. This method utilizes the attribute names from `aug_input` (an object of the AugInput class), passes the corresponding attribute values to the `get_transform` method, generates a transformation object, and then performs the corresponding transformation on the data in the `aug_input` object. One sample usage of the AugInput class can be seen in the following code snippet:

```python
# AugmentationList
rse_aug = T.ResizeShortestEdge(
    short_edge_length = (640, 672, 704, 736, 768, 800),
    max_size        = 800,
    sample_style    = 'choice')
rf_aug = T.RandomFlip(
    prob       = 0.5,
    horizontal = True,
    vertical   = False)
augs = [rse_aug, rf_aug]
augmentation = T.AugmentationList(augs)
```

```python
# AugInput
input = T.AugInput(img, boxes=boxes)
# Transformation
tfms = augmentation(input)
tfm_img = input.image
tfm_boxes = input.boxes
visualize_image_boxes(tfm_img, tfm_boxes)
```

This snippet shows the most commonly used augmentation in Detectron2. This snippet creates a list of two augmentations for resizing and randomly flipping the input horizontally. In this case, the augmentation is performed on the input in place. Thus, `input.image` and `input.boxes` return the transformed versions of these attributes. Furthermore, the returned object (`tfms`) is an object of the `Transform` object in the augmentation object that can be used to change other inputs. This snippet shows the exact default augmentations used in Detectron2. However, it helps to illustrate that it is relatively easy to plug in existing image augmentations and transformations using the declarative approach. If you need to create a custom image augmentation technique, then read on – the next chapter will show you how to create custom image augmentations, as well as how to apply image augmentations at test time.

Congratulations! At this point, you should have mastered important image augmentation techniques and have profound knowledge of the transformations and augmentations that Detectron2 offers.

## Summary

This chapter introduced image augmentations and why it is essential to perform them in computer vision. Then, we covered common and state-of-the-art image augmentation techniques. After understanding the theoretical foundation, we looked at Detectron2’s image augmentation system, which has three main components, and their related classes: `Transform`, `Augmentation`, and `AugInput`. Detectron2 provides a declarative approach for applying existing augmentations conveniently.

The existing system supports augmentations on a single input, while several modern image augmentations require data from different inputs. Therefore, the next chapter will show you how to modify several Detectron2 data loader components so that you can apply modern image augmentation techniques. The next chapter also describes how to apply test time augmentations.

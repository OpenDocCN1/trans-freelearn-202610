Note that we may need to normalize the input image or choose a high value for `scale`. The default eigenvectors and values are for RGB images, while this snippet works with BGR images. Therefore, this snippet keeps reversing the color channels back and forth.

**The AugmentationList class** takes a list of transformations or augmentations (augs) as an input parameter. This class helps apply changes (augs) to the input sequentially. This class does not implement the `get_transform()` method; instead, it utilizes the `__call__` method to apply the augmentations or transformations to the input wrapped inside AugInput.

The following section provides further details about AugInput and its usage pattern.

# The AugInput class

This special class encapsulates data for inputs to augmentation objects. Specifically, an instance of this class includes attributes for the input image (`image`) and related annotations, including bounding boxes (`boxes`) and semantic segmentations (`sem_seg`). This class has a method called `transform(tfm: Transform)` that takes the input parameter as a Transform object (`tfm`) and performs the corresponding transformation on all of its attributes (`image`, `boxes`, and sem_seg) in place. If required, users can define a similar class with additional attributes to be transformed by providing attribute access (`getattr`) and a corresponding transform method to perform additional transformations on the attributes in place and return a Transform object.

Despite having the `transform` method, which can be used by itself, the main usage pattern of this class is to be used as the input to the `__call__` method of an Augmentation object. In this case, the `transform` operation of the Augmentation class changes the input in place and returns an object of a Transform class to perform augmentation on other inputs if needed. This method utilizes the attribute names from `aug_input` (an object of the AugInput class), passes the corresponding attribute values to the `get_transform` method, generates a transformation object, and then performs the corresponding transformation on the data in the `aug_input` object. One sample usage of the AugInput class can be seen in the following code snippet:

```
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
```
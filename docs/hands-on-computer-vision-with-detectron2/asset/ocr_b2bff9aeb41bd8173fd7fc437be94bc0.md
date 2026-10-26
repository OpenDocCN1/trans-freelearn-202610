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
```

This snippet shows the most commonly used augmentation in Detectron2. This snippet creates a list of two augmentations for resizing and randomly flipping the input horizontally. In this case, the augmentation is performed on the input in place. Thus, `input.image` and `input.boxes` return the transformed versions of these attributes. Furthermore, the returned object (`tfms`) is an object of the `Transform` object in the augmentation object that can be used to change other inputs. This snippet shows the exact default augmentations used in Detectron2. However, it helps to illustrate that it is relatively easy to plug in existing image augmentations and transformations using the declarative approach. If you need to create a custom image augmentation technique, then read on – the next chapter will show you how to create custom image augmentations, as well as how to apply image augmentations at test time.

Congratulations! At this point, you should have mastered important image augmentation techniques and have profound knowledge of the transformations and augmentations that Detectron2 offers.

# Summary

This chapter introduced image augmentations and why it is essential to perform them in computer vision. Then, we covered common and state-of-the-art image augmentation techniques. After understanding the theoretical foundation, we looked at Detectron2’s image augmentation system, which has three main components, and their related classes: `Transform`, `Augmentation`, and `AugInput`. Detectron2 provides a declarative approach for applying existing augmentations conveniently.

The existing system supports augmentations on a single input, while several modern image augmentations require data from different inputs. Therefore, the next chapter will show you how to modify several Detectron2 data loader components so that you can apply modern image augmentation techniques. The next chapter also describes how to apply test time augmentations.
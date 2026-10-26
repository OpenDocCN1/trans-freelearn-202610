The RandomExtent class takes input parameters as tuples of size two for scale_range and shift_range. The former helps decide the size of the cropping subregion, while the latter helps determine the location to extract the subregion from. Specifically, the scale_range tuple specifies a range with low (l) and high (h) values, where a value (r) is uniformly sampled from this range; then, the output image's size is r*H and r*W (where H and W are the height and width of the input image, respectively). The second parameter, shift_range, is another tuple of size two, (x, y). Two random values (x_shift and y_shift) are uniformly sampled from the [-x, x] and [-y, y] ranges; then, the cropped region is also shifted by x_shift*W/2 and y_shift*H/2 around the center of the input image. The following code snippet illustrates the usage pattern of this class:

```
re_aug = T.RandomExtent(
    scale_range=(0.5, 0.9),
    shift_range=(0.3, 0.3))
re_tfm = re_aug.get_transform(img)
re_img = re_tfm.apply_image(img)
re_boxes = re_tfm.apply_box(boxes)
visualize_image_boxes(re_img, re_boxes)
```

Figure 8.12 shows some sample output for this code snippet (right) and the original input (left). As we can see, in the original input (left), a dashed box has been placed in the center of the input image. The shift_range parameter dictates this box size. A random point within this box is selected to place the center of the cropping region. The scale_range parameter decides the size of the cropping region (solid box):

![](bbox=[0.265, 0.535, 0.495, 0.745])

![](bbox=[0.565, 0.545, 0.705, 0.68])

Figure 8.12: Original image and the ground-truth box and their random-extent versions
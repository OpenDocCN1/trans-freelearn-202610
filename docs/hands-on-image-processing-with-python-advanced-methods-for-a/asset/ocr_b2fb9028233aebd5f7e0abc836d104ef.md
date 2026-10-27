+   - Finally, the destination image pixels are obtained by copying the valid pixel values (e.g., the ones corresponding to the coordinates that fall within the boundaries of the image dimensions) from the source image, using the source indices obtained. Effectively, it implements the `warp()` function.

Why rearrange `src_pts` and `dst_pts` as `(x, y) → (y, x)` before estimating `ProjectiveTransform`?

+   - In mathematical/geometry terms (e.g., when defining points in a plane), coordinates are usually given as `(x, y)`, where the following apply:
+     - `x` is the horizontal axis (column index)
+     - `y` is the vertical axis (row index)
+   - However, in NumPy and image arrays, pixels are accessed using row-major order.
+   - So, when dealing with image transformations, for example, when using the `estimate()` method from the `skimage.transform.ProjectiveTransform` class, it expects points in (row, column) format (i.e., `(y, x)`). So, it becomes necessary to rearrange the input coordinates.

The next code snippet demonstrates how the function works, with the following figure representing the input image warped to produce the output as shown:

```
def warp_projective(src_pts: np.ndarray, dst_pts: np.ndarray, \
                   im_src: np.ndarray
) -> np.ndarray:
    # Rearrange coordinates from (x, y) to (y, x) format
    # for processing
    src, dst = src_pts[:, [1, 0]], dst_pts[:, [1, 0]]
    # Create a ProjectiveTransform object, estimate transform from
    # src to dst
    pt = ProjectiveTransform()
    pt.estimate(src, dst)
    # Generate a meshgrid of pixel coordinates in dst image
    x, y = np.mgrid[:height, :width]
    dst_indices = np.hstack((x.reshape(-1, 1), y.reshape(-1,1)))
    # Apply inverse transform to get corresponding src coords
    src_indices = np.round(pt.inverse(dst_indices), 0) \
                    .astype(int)

    # Filter out indices that fall outside the valid image bounds
    valid_idx = np.where((src_indices[:,0] < height) & \
                         (src_indices[:,1] < width) & \
                         (src_indices[:,0] >= 0) & \
                         (src_indices[:,1] >= 0))
    # Keep only valid destination and corresponding source
    # coordinates
    dst_indices_valid = dst_indices[valid_idx]
```
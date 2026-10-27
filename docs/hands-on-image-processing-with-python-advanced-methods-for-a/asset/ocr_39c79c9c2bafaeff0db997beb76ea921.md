```
YCrCb_LOWER, YCrCb_UPPER = (0, 135, 85), (255, 180, 135)

def detect_skin_color(img: np.ndarray) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
    img_HSV = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    HSV_mask = cv2.inRange(img_HSV, HSV_LOWER, HSV_UPPER)
    HSV_mask = cv2.morphologyEx(HSV_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    img_YCrCb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)
    YCrCb_mask = cv2.inRange(img_YCrCb, YCrCb_LOWER, YCrCb_UPPER)
    YCrCb_mask = cv2.morphologyEx(YCrCb_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    global_mask = np.bitwise_and(YCrCb_mask,HSV_mask)
    global_mask = cv2.medianBlur(global_mask,3)
    global_mask = cv2.morphologyEx(global_mask, cv2.MORPH_OPEN, np.ones((4,4), np.uint8))
    img_RGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    out_HSV = cv2.cvtColor(img & cv2.merge([HSV_mask]*3), cv2.COLOR_BGR2RGB)
    out_YCrCb = cv2.cvtColor(img & cv2.merge([YCrCb_mask]*3), cv2.COLOR_BGR2RGB)
    out_global = cv2.cvtColor(img & cv2.merge([global_mask]*3), cv2.COLOR_BGR2RGB)
    return img_RGB, out_HSV, out_YCrCb, out_global

img = cv2.imread("images/cycling.png")
img_RGB, out_HSV, out_YCrCb, out_global = detect_skin_color(img)
plt.imshow(np.hstack((img_RGB, out_HSV, out_YCrCb, out_global)))
```

If you run the preceding code snippet, you will get the following figure as output:

![](bbox=[0.12, 0.75, 0.91, 0.85])

Figure 1.32 – Skin color detection with different color spaces
This strategy automatically adapts filter size to different face dimensions. Selected landmarks are converted into image coordinates and enclosed by a bounding rectangle. This provides an intuitive visualization of how MediaPipe partitions the face into semantically meaningful regions such as eyes, eyebrows, nose, lips, and entire face. Transparency-aware compositing is then performed using the PNG alpha channel, which produces visually realistic overlays without visible rectangular artifacts (using alpha blending).

```
pts = np.array([[int(face_landmarks[i].x*w), int(face_landmarks[i].y*h)]
    for i in indices])
x, y, bw, bh = cv2.boundingRect(pts)
```

The next figure illustrates three stages of the pipeline: the original image, detected facial regions, and the final Snapchat-style augmented image with sunglasses, mustache, and cartoon nose filters.

![](bbox=[0.22, 0.35, 0.82, 0.52])

Figure 12.45 – Snapchat filters with mediapipe

# Hand landmark detection and gesture visualization

MediaPipe Hand Landmarker estimates 21 anatomical keypoints for each detected hand:

$H = \{(x_i, y_i, z_i)\}_{i=1}^{21}$

These landmarks correspond to finger joints, fingertips, and palm locations, providing a compact skeletal representation of the hand. The detector learns $g(I) \rightarrow H$, where $H$ encodes hand pose and fine-grained finger articulation. The pretrained hand landmark detector is initialized using the MediaPipe Tasks API:

```
options = vision.HandLandmarkerOptions(base_options=base_options, num_hands=2)
self.detector = vision.HandLandmarker.create_from_options(options)
```

which enables simultaneous tracking of up to two hands. Unlike bounding-box-based detectors, landmark models capture fine-grained finger movements and are therefore suitable for gesture recognition and sign-language analysis. The hand skeleton is represented as a graph $G = (V, E)$, where the vertices $V$ correspond to landmarks and the edges (E) define anatomical connections between joints. For example, the thumb chain is represented by (0,1), (1,2), (2,3), (3,4). These skeletal connections are encoded as:

```
HAND_CONNECTIONS = [(0,1), (1,2), (2,3), (3,4), ...]
```
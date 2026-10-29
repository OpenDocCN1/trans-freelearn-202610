```
(Then #"^the state of the aggregator should be (\d+)$" [arg1]
;; state is 0 + 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9
(assert (= 45
(eem/state (eem/get-handler emitter :accumulator)))))
```

Where this is good to use is, for example, to retrieve some statistics such as averages, on the data passing by the features of the scenario.

# OpenCV

Alright, this is going to be somehow a bit of a bonus! This is a great way to get excited and try new things though. The coming feature writes in easy-to-understand English "features" you expect to find in media content, say, pictures.

For example, the following is simply just adapting the standard face recognition opencv lena example, but you do have to agree that this is easier to read than the usual OpenCV code.

Now here, again, the context of this is not about the OpenCV library itself, it is about the content of the image, and how to express it to other people in simple language:

```
Feature: OpenCV Face Recognition
    Playing around with face recognition and cucumber

Scenario: Looking for faces in an image
    When I load the image "fixtures/lena.png"
    And setup the classifier for frontal face recognition
    Then I can find 1 face in the loaded image
```

The whole code includes everything, from reading the image as an object, setting the face detector, starting the face recognition, down to drawing the rectangles:

```
(import '[org.opencv.core Core Mat MatOfRect Point Rect Scalar])
(import '[org.opencv.objdetect CascadeClassifier]
      '[org.opencv.highgui Highgui])
(def image
  (atom nil))
(def faceDetector
```
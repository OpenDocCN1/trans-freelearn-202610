We terminate the loop once we detect the Esc keypress, as usual:

```
if cv2.waitKey(5) == 27 :
    break
```

Once the loop is terminated, we release the camera and destroy the display window:

```
cap.release()
cv2.destroyAllWindows()
```

This will draw the contour roughly around the area where the movement is detected, as shown in the following screenshot:

![](bbox=[0.28, 0.32, 0.72, 0.58])

This code works very well for slow movements. You can make the output more interesting by drawing contours with different colors. Also, you can find out the centroid of the contours and draw crosshairs or circles corresponding to the centroids.

> If you wish to explore OpenCV with Raspberry Pi in more depth, go through Raspberry Pi Computer Vision Programming. Here is the link: https://www.packtpub.com/hardware-and-creative/raspberry-pi-computer-vision-programming.
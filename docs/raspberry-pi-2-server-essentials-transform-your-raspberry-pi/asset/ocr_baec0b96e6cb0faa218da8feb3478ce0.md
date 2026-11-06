# Text overlay

Text overlay is a feature that allows you to superimpose text lines onto video frames without any transcoding. The UV4L driver works extremely well with HD streams and framerates higher than 30, and supports mjpeg, H264, yuv420, and other video formats.

![](bbox=[0.245, 0.24, 0.295, 0.28])

> When enabled, the image width and height should be a multiple of 16. For 1080p you would need to use 1920 x 1088, 720p would be 1280 x 720, and so on.

At the most basic level, text is read from a JSON-formatted file, which contains properties for each line's position, color, thickness, and scale. The default configuration file will display the video frame rate as it is being transmitted. The configuration file gets loaded when the camera is started for the first time, but it can be updated with a specific command, making heads-up displays (HUDs) a practical reality.

To enable the text overlay, you need to start the driver with the following /etc/uv41/uv41-raspicam.conf configuration before loading the camera page for the first time. You cannot enable this feature if there is already an ongoing stream:

```
text-overlay = yes
text-filename = /usr/share/uv4l/raspicam/text.json
```

The text.json file contains a sample of all the options that are available to you. It is a good idea to copy this file to /home/pi.

![](bbox=[0.24, 0.54, 0.285, 0.58])

> If you plan on updating the file frequently, it is recommended to store the file in /tmp as the file will actually be stored in RAM.

After restarting the uv4l_raspicam service, you will now see some sample overlay text on your video stream.

You can edit the text.json file and execute the following command in the console for the overlay text to be updated:

```
v412-ctl --set-ctrl=text_overlay=1
```

This is a very basic approach to updating the overlay on your video stream, for example, running a separate program you wrote that will overwrite the file every so often and issue the refresh command.

Using this approach, you will not be able to update the overlay any faster than 1 second at a time. If you would like to achieve real-time overlay updates, refer to The HUD sample section.
Now we need to update the repository list, and then we can install the UV4L driver. We will do these with the following commands:

```
```bash
sudo apt-get update
sudo apt-get install uv41 uv41-raspicam
sudo apt-get install uv41-server uv41-raspicam-extras
```
```

![](bbox=[0.24, 0.26, 0.29, 0.31])

> The UV4L driver makes it really easy to stream video from the Raspberry Pi. Consider supporting Luca by visiting his website and donating to his cause or purchasing a serial key for the overlay module that is described later in the chapter.

# Configuring the UV4L-RaspiCAM

The configuration file can be found at /etc/uv41/uv41-raspicam.conf and is used to configure the initial video stream. There are default settings that are used, and the following bullet points are settings found within the file that you may be interested in changing manually for your own use.

![](bbox=[0.23, 0.47, 0.28, 0.52])

> These are some typical settings that you may want to use if capturing a stream using VLC. The driver is adaptive and will change configurations if you request MJPEG stream, or it will start a WebSockets stream from the browser, technically allowing you multiple streams. Multi-streaming is experimental, though.

Here are some typical settings:

+   - Encoding:
+     Streaming: H264, mjpeg, jpeg, yuv420
+     Specialist: nv21, yvu420, rgb565, rgb565x, rgb24, bgr24, rgba.
+   - Framerate:
+     0 sets auto
+     30 at 1080p
+     60 at 720p
+     90 at VGA.
+   - Bitrate: 17000000 (default value)
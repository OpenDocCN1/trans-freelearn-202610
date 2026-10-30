```
Input #0, video4linux2, from '/dev/video0':
    Duration: N/A, start: 629.030244, bitrate: 147456 kb/s
    Stream #0:0: Video: rawvideo, yuyv422, 1280x960, 147456 kb/s, 1000k tbn, 7.50 tbc
Output #0, avi, to '/home/pi/book/output/VideoStream.avi':
    Metadata:
        ISFT            : Lavf54.20.4
    Stream #0:0: Video: mpeg4, yuv420p, 1280x960, q=2-31, 200 kb/s, 25 tbn, 25 tbc
Stream mapping:
    Stream #0:0 -> #0:0 (rawvideo -> mpeg4)
Press ctrl-c to stop encoding
frame= 182 fps= 7 q=31.0 Lsize=     802kB time=7.28 bitrate=902.4kbits/s
video:792kB audio:0kB global headers:0kB muxing overhead 1.249878%
Received signal 2: terminating.
```

You can terminate the recording sequence by pressing Ctrl + C.

We can play the video using omxplayer. It comes with the latest raspbian, so there is no need to install it. To play a file with the name vid.mjpg, use the following command:

```
omxplayer ~/book/output/VideoStream.avi
```

It will play the video and display some output similar to the one here:

```
pi@raspberrypi ~ $ omxplayer ~/book/output/VideoStream.avi
Video codec omx-mpeg4 width 1280 height 960 profile 0 fps 25.000000
Subtitle count: 0, state: off, index: 1, delay: 0
V:PortSettingsChanged: 1280x960@25.00 interlace:0 deinterlace:0 anaglyph:0 par:1.00 layer:0
have a nice day ;)
```

Try playing timelapse and record videos using omxplayer.
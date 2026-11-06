+   - The Intra-period: This is used for H264 only; if you experience artifacts in the stream, try adjusting this value. Usually, artifacts are caused by lost frames and poor network performance. The lower this value, the more bandwidth is used. A good start is FPS / 2.
+   - Quality:
+     This applies to JPEG and MJEP streaming only
+     85 is the default value.
+   - * text overlay: Uncomment the two properties as this will be required later in the chapter.
+   - * no preview: If you have a monitor connected to HDMI, you will see the original stream preview. Uncomment this if you are experiencing Raspberry Pi reboot with a 1 A power supply and Wi-Fi.

Most of these configuration settings can be altered through the web interface by navigating to http://raspberrypi:8080. The options with the * mark must be configured in the configuration file, and UV4L must be restarted.

# Installing WebRTC

Web Real-Time Communication (WebRTC) is a protocol that allows modern web browsers to establish a peer-to-peer link to exchange data. UV4L uses WebRTC to create a socket to the Raspberry Pi and receive a live H264 video stream, eliminating the need for transcoding and allowing us to watch a high-quality, low-latency video directly in our browser.

To install the WebRTC module for uv4l-server, we just need to issue one command and restart UV4L:

```
```
sudo apt-get install uv4l-webrtc
sudo service uv4l_raspicam restart
```
```

# WebRTC streaming

Please be aware that UV4L WebRTC streams in an adaptive bitrate and resolution. The bitrate and resolution streamed to the client will adaptively adjust based on the connection quality. This means that any configuration that you have applied will not apply to WebRTC streams, but the overlay will still work. It may also disconnect other streams that do not use WebRTC.
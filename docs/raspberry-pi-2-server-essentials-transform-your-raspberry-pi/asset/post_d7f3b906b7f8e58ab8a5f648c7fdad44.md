
Now we need to update the repository list, and then we can install the UV4L driver. We will do these with the following commands:

```bash
sudo apt-get update
sudo apt-get install uv4l uv4l-raspicam
sudo apt-get install uv4l-server uv4l-raspicam-extras
```

![](img/a8753cacc9d454dfc09eda1ed5012bd3_104_0.png)

> The UV4L driver makes it really easy to stream video from the Raspberry Pi. Consider supporting Luca by visiting his website and donating to his cause or purchasing a serial key for the overlay module that is described later in the chapter.

## Configuring the UV4L-RaspiCAM

The configuration file can be found at /etc/uv4l/uv4l-raspicam.conf and is used to configure the initial video stream. There are default settings that are used, and the following bullet points are settings found within the file that you may be interested in changing manually for your own use.

![](img/a8753cacc9d454dfc09eda1ed5012bd3_104_1.png)

> These are some typical settings that you may want to use if capturing a stream using VLC. The driver is adaptive and will change configurations if you request MJPEG stream, or it will start a WebSockets stream from the browser, technically allowing you multiple streams. Multi-streaming is experimental, though.

Here are some typical settings:

- Encoding:
  - Streaming: H264, mjpeg, jpeg, yuv420
  - Specialist: nv21, yuv420, rgb565, rgb565x, rgb24, bgr24, rgba.
- Framerate:
  - 0 sets auto
  - 30 at 1080p
  - 60 at 720p
  - 90 at VGA.
- Bitrate: 17000000 (default value)
- The Intra-period: This is used for H264 only; if you experience artifacts in the stream, try adjusting this value. Usually, artifacts are caused by lost frames and poor network performance. The lower this value, the more bandwidth is used. A good start is FPS / 2.
- Quality:
  - This applies to JPEG and MJPEG streaming only
  - 85 is the default value.
- * text overlay: Uncomment the two properties as this will be required later in the chapter.
- * no preview: If you have a monitor connected to HDMI, you will see the original stream preview. Uncomment this if you are experiencing Raspberry Pi reboot with a 1 A power supply and Wi-Fi.

Most of these configuration settings can be altered through the web interface by navigating to http://raspberrypi:8080. The options with the * mark must be configured in the configuration file, and UV4L must be restarted.

## Installing WebRTC

Web Real-Time Communication (WebRTC) is a protocol that allows modern web browsers to establish a peer-to-peer link to exchange data. UV4L uses WebRTC to create a socket to the Raspberry Pi and receive a live H264 video stream, eliminating the need for transcoding and allowing us to watch a high-quality, low-latency video directly in our browser.

To install the WebRTC module for uv4l-server, we just need to issue one command and restart UV4L:

```bash
sudo apt-get install uv4l-webrtc
sudo service uv4l_raspicam restart
```

## WebRTC streaming

Please be aware that UV4L WebRTC streams in an adaptive bitrate and resolution. The bitrate and resolution streamed to the client will adaptively adjust based on the connection quality. This means that any configuration that you have applied will not apply to WebRTC streams, but the overlay will still work. It may also disconnect other streams that do not use WebRTC.

## Streaming Live HD Video

You can now navigate to http://raspberrypi:8080, and you will be presented with a screen similar to the following:

![](img/a8753cacc9d454dfc09eda1ed5012bd3_106_0.png)

You can click on audio/video stream via WebRTC. Then, on the next page, click on the start button. It may take a few seconds to establish a link, and if everything goes as planned, you will now see a live stream from the Raspberry Pi camera.

With the Raspberry Pi 1, it was extremely complicated to set up a stream similar to this one, leaving almost no resources left on the Pi at all. The WebRTC module is truly a time saver in terms of streaming from your Raspberry Pi.

## Real time HTTP streaming

To configure the HTTP stream, you can click on the Camera Control Panel link on the UV4L web page and set the format and other options you would like.

Alternatively, if you don't want to access the webpage, you can just configure the raspicam configuration file as you need it, and then you can use the corresponding URL in VLC or a third-party app to capture the stream:

- HTTP/MJPEG: http://raspberrypi:8080/stream/video.mjpeg
- HTTP/Raw H264: http://raspberrypi:8080/stream/video.h264
- HTTP/JPEG: http://raspberrypi:8080/stream/video.jpeg

To receive a H264 stream using VLC, I have found that tweaking some of the plugin settings worked best for me:

- 1. Click on **Tools** | **Preferences**.
- 2. Click on **All** in the **Show settings** panel in the bottom-left corner.
- 3. Click on **Input / Codecs** | **Demuxers**.
- 4. Change the demux module from the **Automatic** to **H264** video demuxer.
- 5. Click on **Demuxers** | **H264**.
- 6. You will notice that it is set to 25 frames per second. Unfortunately, changing this value does not work as expected. You should rather change your configuration on the Raspberry Pi to stream at 25 frames per second.
- 7. On the Raspberry Pi, choose a suitable bit rate for your stream. On the LAN, this may be the default, but on Wi-Fi, I suggest you try lower bitrates.

This configuration works best for higher quality streaming. It is necessary to change the VLC demuxer to H264 because it does not seem to be able to recognize the format on its own. You can also specify an additional option of `:demux=h264`, but I find that strictly setting the demuxer to H264 works better.

We also need to adjust the frame rate on the Raspberry Pi to 25 fps to match the default in the H264 module in VLC. Adjusting the frame rate in the module on VLC to 30fps did not help. The reason behind adjusting the frame rate is that I noticed strange artifacts on moving objects. This suggested bandwidth problems, but it even happened on the LAN.

I finally found this solution and realized that the Raspberry Pi was sending 30 fps while the demuxer was decoding at 25 fps, causing the key frames to be out of sync. As strange as this may sound, it really does produce a good quality stream, albeit with about 1+ seconds of buffering.

If you are experiencing no video, lots of artifacts, or long lags, refer to Chapter 2, *Preparing a Network* on how to run the `iperf` test to make sure you have good enough bandwidth. Bandwidth problems may also be caused by a poor power supply, especially if you are using a wireless adapter.

## Web conferencing

In the latest version of UV4L, there is an interesting new feature for web conferencing with audio and video. This feature is built into the `webrtc` module, and no extra configuration is necessary.

Any WebRTC compatible device, such as a smart phone, note book, or standard desktop, with a modern browser can join into the conference by going to http://raspberrypi:8080/conference. This will work inside your LAN, but if you would like to allow connections from the Internet, you need to provide the correct IP address in the Signaling Server Field and open the necessary ports.

This works purely as a server and does not use the Pi's camera, but you can join the conference from your Raspberry Pi from within the X client and a browser. If you want to use audio on the Raspberry Pi, make sure you have pulseaudio installed and a compatible USB sound card with a microphone.

## Streaming the X desktop

It is also possible to stream your actual X desktop using the UV4L driver. Consider it something like screen casting, similar to Miracast. If your smart TV's browser supports WebRTC, you can simply enter the URL and have a wireless video feed of the desktop on the TV.

To use this feature, we need to install an extra module.

```bash
sudo apt-get install uv4l-xscreen
```

You can then use raspi-config to enable boot to desktop/scratch to start up on the boot. As mentioned in the WebRTC section, the only caveat at the moment is that this will only work with a specific resolution. Other resolutions may be supported in the future. You will need to edit your /boot/config.txt file as follows:

```
framebuffer_width=640
framebuffer_height=480
framebuffer_depth=32
framebuffer_ignore_alpha=1
```

You can now log into your Raspberry Pi over SSH and issue the following command:

```
uv4l --driver xscreen --auto-video_nr --display :0 --framerate 10 --server-option '--port=9000'
```

Ignore all warnings. This command starts a new UV4L server and creates a device called /dev/video1 that can be used with the same commands as described in the real-time HTTP streaming section. It creates the webserver under port 9000. Port 8080 still works and can continue to stream your webcam while streaming the X video separately.

You can view the video transmission at: http://raspberrypi:9000/stream/webrtc.

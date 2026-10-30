Once you capture all the images, the next part is to encode them all in a fast playing video, preferably 20 to 30 frames per second. For this part, the mencoder utility is recommended. The following are the steps to create a timelapse video with mencoder on a Raspberry Pi or any Debian/Ubuntu machine:

+   1. Install mencoder using sudo apt-get install mencoder
+   2. Navigate to the output directory by using:
+      cd /home/pi/book/output/timelapse
+   3. Create a list of your timelapse sequence images using:
+      ls garden_*.jpg > timelapse.txt
+   4. Use the following command to create a video:
+      mencoder -nosound -ovc lavc -lavcopts vcodec=mpeg4:aspect=16/9:vbitrate=8000000 -vf scale=1280:960 -o timelapse.avi -mf type=jpeg:fps=30 mf://@timelapse.txt

This will create a video with name timelapse.avi in the current directory with all the images listed in timelapse.txt with a 30 fps frame rate. The statement contains the details of the video codec, aspect ratio, bit rate, and scale. For more information, you can run man mencoder on Command Prompt. We will cover how to play a video in the next section.

# Webcam video recording and playback

We can use a webcam to record live videos using avconv. Install avconv using sudo apt-get install libav-tools. Use the following command to record a video:

```
avconv -f video4linux2 -r 25 -s 1280x960 -i /dev/video0 ~/book/output/VideoStream.avi
```

It will show following output on the screen:

```
pi@raspberrypi ~ $ avconv -f video4linux2 -r 25 -s 1280x960 -i /dev/video0 ~/book/output/VideoStream.avi
avconv version 9.14-6:9.14-1rpilrpil, Copyright (c) 2000-2014 the Libav developers
  built on Jul 22 2014 15:08:12 with gcc 4.6 (Debian 4.6.3-14+rpil)
[video4linux2 @ 0x5d6720] The driver changed the time per frame from 1/25 to 2/15
[video4linux2 @ 0x5d6720] Estimating duration from bitrate, this may be inaccurate
```
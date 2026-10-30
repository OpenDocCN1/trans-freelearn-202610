
## Adding Cameras to Our Security System

As you can see in the following image, the ribbon connector is not that long, so the camera needs to be located close to the Raspberry Pi. By using a camera enclosure, you could actually mount the camera directly on top of the Raspberry Pi case itself, if that works for you.

![](img/631e8b200cce187082ee46b0eb1681b3_372_0.png)

The camera module, housed within an enclosure

## Setting up the camera module

Before we can use the camera module, we need to enable camera support on the Raspberry Pi. To do this, we use the `raspi-config` tool, as we did with the I2C bus earlier in our journey.

- 1. Connect to your Raspberry Pi the lazy way from your sofa using SSH, or directly using a keyboard and monitor.
- 2. Once you've logged in, launch the config tool with the following command:

```bash
$ sudo raspi-config
```

3. And then, select 5 Enable Camera.

![](img/631e8b200cce187082ee46b0eb1681b3_373_0.png)

4. You'll then be asked to confirm whether you want to enable camera support.

![](img/631e8b200cce187082ee46b0eb1681b3_373_1.png)

5. Select <Enable>.

6. Then, select Finish and reboot your Pi to enable the camera settings.

![](img/631e8b200cce187082ee46b0eb1681b3_375_0.png)

The test photo taken by the Raspberry Pi Camera Module

> If you find that raspistill outputs errors when you run it, ensure that it is connected properly at both ends of the ribbon cable. One other catch is that sometimes the ribbon that connects the actual camera lens component to the tiny connector on the camera board can come loose. Just ensure that this is securely connected too. I've had this issue a couple times after the camera modules have been taken out of my box of random test bits to be used.

The raspistill utility has loads of options for manipulating the images it captures, and we'll use some of them a bit later in our capture script. In the meantime, to see the available options, run `raspistill` without any options and they will be listed:

```bash
$ raspistill
```

## Be a video star

Now that we know our camera module is working, we can try and capture some video. To do this, we'll use the `raspivid` utility. The following command will take 5 seconds of high-definition video and save the file to your Raspberry Pi:

```bash
$ raspivid -o test.h264 -t 5000
```

You'll notice that file is called `test.h264`—this is because the video is captured as a raw **H.264** video stream. Unfortunately, not many media players will handle these files (although VLC player will—it rocks and handles practically anything you throw at it—get it on your PC at `www.videolan.org`).

## Adding Cameras to Our Security System

If you want to play the file on smartphones and conventional media players, then we will need to wrap it in a container format, such as MPEG-4, and give the file a .mp4 extension.

To do this, we'll use the GPAC package, which is an open source multimedia framework. It comes with a utility called MP4Box, which is a tool we'll use to create an MP4 container for our video file:

- 1. First, install the GPAC package:
  
  ```bash
  $ sudo apt-get install gpac
  ```
  
- 2. Once it's installed, run the command to convert the test video we created:
  
  ```bash
  $ MP4Box -fps 30 -add test.h264 test.mp4
  ```

You should now have the file, test.mp4, which you can download and play on your PC or smartphone.

> Another popular conversion tool is ffmpeg, which I use a lot on Windows to convert video files; however, it can be quite complex and although there is a package for the Raspberry Pi, I actually couldn't get it to convert properly on the Pi. MP4Box is much more straightforward and fitting for our needs.

## Caught on camera

So, we now have a method of capturing still images and video, which we can put to use in our security system. If we want to have this running constantly, we could write a script to take video constantly, but this would soon fill up our memory card and wouldn't be particularly efficient. So, we'll combine our camera system with the motion detectors we connected earlier.

In the last chapter, we created an alarm zone which had a couple of sensors and a motion detector connected to our system on the input GPA0. So, let's write a script that will take a video clip whenever the motion detector is triggered:

```bash
#!/bin/bash

# set up port expander
sudo i2cset -y 1 0x20 0x00 0xFF

# loop forever
while true
```

```sh
do
    # read the GPA inputs
    GPA=$(sudo i2cget -y 1 0x20 0x12)

    # detect the zone on input 0
    if [ $GPA == "0x01" ]
    then
        #circuit normally closed so zone is OK
        #short delay
        sleep 0.5

    else
        #zone is activated so take a 20 sec video clip

        #filename will be based on current timestamp
        sDate='date +%d%m%y'
        sTime='date +%T'
        echo "Zone 1 Activate at $sDate $sTime"

        #take video clip
        raspivid -o $sDate$sTime.h264 -t 20000

        #convert to MP4
        MP4Box -fps 30 -add $sDate$sTime.h264 $sDate$sTime.mp4
    fi
done
```

## You have new mail

Having the images stored on your Raspberry Pi is not really much use—ideally, you would want the images sent to you straightaway, as soon as they are captured, so that you can view them on your smartphone.

An easy, quick, and reliable way to do this is to simply have them e-mailed to you. Hence we're going to add an e-mailing functionality to our home security system so that image captures are attached to a message and sent to your e-mail address straightaway, which you can access from your smartphone. The images can then be removed from your Raspberry Pi to prevent the SD card space from being clogged up with these reasonably large files.

## Sending attachments

Now that we can send basic emails from our home security system, let's try sending the still image taken from our camera earlier. But first, we need to install yet another package to help us with this:

```
$ sudo apt-get install mpack
```

Once that's installed, you can send the test image file we took previously by using the following command:

```
$ sudo mpack -s "Security Photo" test.jpg me@mydomain.com
```

We now have all of the elements needed to send alerts and images from our home security system directly to our smartphone using email.

## Where was that taken?

Ordinarily, you could just annotate the email message with where and when the attached image was taken, but that wouldn't be as cool as actually overlaying the image with some text, would it? So let's do some magic with the help of imagemagick, which is a popular command-line image manipulation tool. Install it with the following line:

```
$ sudo apt-get install imagemagick
```

We'll now use the command line to take the test photo that we took earlier, overlay some text using one of the imagemagick utilities, and save it to another file:

```
$ convert test.jpg -fill red -pointsize 48 annotate +20+60 'Camera 1' annotated.jpg
```

## Adding Cameras to Our Security System

After a few seconds, this will have generated a file called `annotated.jpg` containing our image with **Camera 1** written in red in the top corner. When we put all of this together in our final system, we'll also overlay the image with a time stamp.

![](img/631e8b200cce187082ee46b0eb1681b3_380_0.png)

> At the moment, the images generated by the `raspistill` tool are pretty large, being high resolution photos. This makes manipulating and sending them a bit time-consuming as far as processing time is concerned, so when we build our final system, we'll be using the `raspistill` options, `-w`, `-h` and `-q`, to reduce the size and quality of the images to make the system more efficient.

To capture smaller image files, try using the following command:

```bash
$ raspistill -o test.img -h 768 -w 1024 -q 25
```

## Night vision

The standard Raspberry Pi camera is great for taking daytime snaps of people walking up the garden path, but when it comes to night time shots, it's not really suitable. There are two ways of dealing with this: the first is to illuminate the capture area with a bright light when the PIR detector is triggered, and the second is to use the Raspberry Pi **NoIR camera module** and an infra-red LED array to let the camera see in the dark. More about that in a minute.


You may want to play with the resolution to find the optimum settings for your application. Big images are great—they give you a more detailed view of the world—but they also take up significantly more processing power. You'll play with this resolution more as you actually ask your system to do some real image processing. Be careful if you are going to use vncserver to understand your system performance, as this will significantly slow down the update rate. An image that is twice the size (width/height) will involve four times more processing. You can now use this capability to do a number of impressive tasks.

## Gesture detection

OpenCV and your webcam can also track objects. This will be useful as you want your project to differentiate your hand from the background. OpenCV makes this amazingly simple by providing some high-level libraries that can help us with this task. To accomplish this, you'll edit a file to look something similar to what is shown in the following screenshot:

![](img/324d126a7e2da696837a3e066326bd61_201_0.png)

## Playing Rock, Paper, or Scissors with Raspberry Pi Zero

The following screen is the measure of your hand for paper:

![](img/324d126a7e2da696837a3e066326bd61_204_0.png)

You can add some simple code to determine if your hand is making a rock, paper, or scissors using the ratios of *x* and *y*. In the preceding picture, you can see that the ratio of width to height is roughly 140/80 for paper. For a rock, the ratio is going to be roughly 85/55. For scissors, it is going to be 140/40. Here is the code for the library that determines a rock/paper/or scissors:

```
import math

cap = cv2.VideoCapture(0)
cap.set(3, 320)
cap.set(4, 240)

def getHand():
    while(cap.isOpened()):
        ret, img = cap.read()
        cv2.rectangle(img, (200,200), (50,50), (0,255,0),0)
        crop_img = img[50:200, 50:200]
        grey = cv2.cvtColor(crop_img, cv2.COLOR_BGR2GRAY)
        value = (35, 35)
        blurred = cv2.GaussianBlur(grey, value, 0)
        _, thresh1 = cv2.threshold(blurred, 127, 255, cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)
        cv2.imshow('Thresholded', thresh1)
        contours, hierarchy = cv2.findContours(thresh1.copy(),cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
        max_area = -1
        for i in range(len(contours)):
            cnt=contours[i]
            area = cv2.contourArea(cnt)
            if(area>max_area):
                max_area=area
                ci=i
        cnt=contours[ci]
        x,y,w,h = cv2.boundingRect(cnt)
        print x, y, w, h
        if w != 148 and h !=148: # not starting rectangle
            if w/h >= 3:
                return 3  # scissors
            elif w/h < 3 and w/h > 1:
                return 2
            else:
                return 3
        cv2.rectangle(crop_img, (x,y), (x+w,y+h), (0,0,255),0)
        cv2.imshow('Gesture', img)
        k = cv2.waitKey(10)
        if k == 27:
            break

print getHand()
```

Now you know the state of the human opponent's hand. To complete the entire project, you'll need to add code to make a random selection for the computer hand's state, call the functions in the previous sections of this chapter to make the hand move to that state, and then compare the random selection of your robot hand and the user's hand to determine the winner. Playing rock, paper, or scissors has never been easier!

## Summary

You now have a hand that can play rock, paper, or scissors! By now you should have quite a few different capabilities that you can add to almost any project. In the last chapter, you'll add Raspberry Pi Zero to a quad copter to build a project that can fly.

## 9
### Adding Raspberry Pi Zero to a Quadcopter

You've had the opportunity to build lots of different types of robot, so now let's end with the one that can be truly amazing, a robot that can fly.

> Before we start, there are a number of safety warnings to be considered. Of course, never fly near people or buildings. Also, be very careful about flying outside your viewing range. In many countries, there are restrictions on where and when you can fly a quadcopter. Some countries, including the United States, require you to register your quadcopter.

It is also important to note that this chapter is not designed to be a complete step-by-step guide to the construction of a quadcopter for flying. That would take many chapters. This chapter is designed to get you started if you consider building a quadcopter that can be controlled using the Raspberry Pi Zero. In this chapter, you'll learn about the following topics:

- Building the basic quadcopter platform
- Interfacing Raspberry Pi Zero with the flight controller
- Discussing long-range communications
- Using GPS for location
- Adding autonomous flight

## Constructing the platform

Constructing the quadcopter hardware can be daunting; however, there are several excellent websites that can lead you through the process, from component selection to build details and programming and controlling your quadcopter with a radio. The http://www.arducopter.co.uk/ website is a great place to start for those who are new to quadcopter flight. Go to http://copter.ardupilot.com/, which is another excellent website with lots of information.

For this project, you'll want to choose a project that uses the **Pixhawk** flight controller. There are other flight controllers that are significantly less expensive, but this particular flight controller provides easy access for Raspberry Pi Zero. Here are some possible websites that can guide you through the construction process: http://learnrobotix.com/uavs/quadcopter-build/pixhawk/connecting-the-q-brain-esc.html, http://learnrobotix.com/uavs/quadcopter-build/pixhawk/connecting-components-pixhawk-flight-controller.html, and http://www.flying-drone.co.uk/how-to-build-a-quadcopter-with-a-pixhawk-flight-controller-step-11/.

At http://copter.ardupilot.com/wiki/advanced-pixhawk-quadcopter-wiring-chart/, you'll find an excellent wiring diagram of how to hook everything up. Let's go through the steps of constructing our own quadcopter.

First, you'll need a frame. You'll be building a quadcopter of size 450 mm, one of the least expensive frames, which is available from most online retailers, with fiberglass arms, as shown in the following image:

![](img/324d126a7e2da696837a3e066326bd61_208_0.png)

Now, perform the following steps to complete your quadcopter assembly:

- 1. The first step is to build the quadcopter as the instructions suggest.
- 2. The second step is to solder the four **Electronic Speed Controllers (ESCs)**, one to each motor, and the battery connection to the bottom plate. Here is an image of the bottom plate:

![](img/324d126a7e2da696837a3e066326bd61_209_0.png)

Note the + and - connections; each connection will be soldered to all the ESCs. The following is an image of the motor controller:

![](img/324d126a7e2da696837a3e066326bd61_209_1.png)

## Adding Raspberry Pi Zero to a Quadcopter

The red and black wire connectors are the connectors that are soldered to the bottom plate of the frame. The other three connectors will connect to the motor.

- 3. The third step is to install the motors on the frame. You'll need motors that are in the 1000 KV range, here is an image of such a motor:

![](img/324d126a7e2da696837a3e066326bd61_210_0.png)

Again, follow the instructions that came with your frame to attach the motor. Then attach the three connections that come from the ESC to the motor.

- 4. One optional step is to add a landing gear set to the unit. There are many of these available; the following is an image of one that is very sturdy:

![](img/324d126a7e2da696837a3e066326bd61_210_1.png)

- 5. Now you'll install Pixhawk on the frame and connect its associated electronics. The details are shown and described at http://copter.ardupilot.com/wiki/advanced-pixhawk-quadcopter-wiring-chart/. This will connect the Pixhawk to the ESCs, the battery, an RC transmitter, a telemetry radio, and a switch that will prevent the quadcopter from flying until you are ready.
- 6. Eventually, you will install four propellers on the quadcopter; however, you will have to wait until you have calibrated the ESCs, motors, and RC transmitter to install them. You'll need four propellers, two that are designed to spin clockwise and another two that are designed to spin counter-clockwise. For this quadcopter, you'll want propellers that are 10x4.7 pitches. Here is an image of one such propeller:

![](img/324d126a7e2da696837a3e066326bd61_211_0.png)

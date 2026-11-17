
![Figure 12.12 – The LEVEL HORIZON window](image/B19566_12_12.jpg)

Figure 12.12 – The LEVEL HORIZON window

## Pressure/Barometer Calibration

Next, we have pressure/barometer calibration. Calibrating the pressure sensor sets the drone’s current altitude to 0 at the current pressure:

*   Click on the **`Calibrate`** button; calibration will take place.
*   Once the calibration process has finished, a message stating **`Barometer calibration complete`** will appear on the screen.

![Figure 12.13 – The PRESSURE window](image/B19566_12_13.jpg)

Figure 12.13 – The PRESSURE window

## Safety Parameters

Next, we must set the safety parameters:

### Battery Failsafe Trigger

*   **`Low Action`**: `RTL (Return to Launch)`
*   **`Critical Action`**: `RTL (Return to Launch)`
*   **`Low Voltage Threshold`**: `21 V`
*   **`Critical voltage threshold`**: `20 V`

### Failsafe Trigger

*   **`Ground Station Failsafe`**: `Always enable RTL`
*   **`Throttle Failsafe`**: `Always RTL`

### PWM Threshold

*   Set **`PWM Threshold`** to `1000`

### RTL (Return to Launch)

Adjust these settings according to your requirements. However, for survey missions, the altitude should stay between 80 and 150m AGL. Hence, it is recommended to choose **`Return At Current Altitude`**. The rest of the settings can remain the same:

![Figure 12.14 – The RTL window](image/B19566_12_14.jpg)

Figure 12.14 – The RTL window

Keep the other settings as is or go through the AeroGCS manual and adjust the settings according to your needs.

## Flight Modes

Next, we will assign the flight mode. Keep **`Flight Mode Channel`** set to **`Channel 5`** since this is the flight stack standard. Note that this isn’t mandatory. Users can choose a different one as per their requirements.

Assign different flight modes to the channel so that it can be configured on RC/Herelink as well:

![Figure 12.15 – The FLIGHT MODES window](image/B19566_12_15.jpg)

Figure 12.15 – The FLIGHT MODES window

## RC Calibration

Finally, we have RC calibration. For RC configuration (**`RPA Configuration`** | **`RC Config`**), RC transmitters allow the pilot to set the flight mode, control the vehicle’s movement and orientation, and also turn on/off auxiliary functions (that is, raising and lowering the landing gear, and so on).

RC calibration involves capturing each RC input channel’s minimum, maximum, and “trim” values so that ArduPilot can correctly interpret the input.

Move both sticks in the largest circle possible so that they reach their complete range of motion. Move the **`Channel 5`** and **`Channel 6`** toggle switches through their range of positions.

Your transmitter should cause the following control changes:

*   **`Channel 1`**: Low = roll left, high = roll right
*   **`Channel 2`**: Low = pitch forward, high = pitch back
*   **`Channel 3`**: Low = throttle down (off), high = throttle up
*   **`Channel 4`**: Low = yaw left, high = yaw right

Herelink also has a built-in RC calibration tool that can be used for RC calibration:

![Figure 12.16 – The RC CALIBRATION window](image/B19566_12_16.jpg)

Figure 12.16 – The RC CALIBRATION window

# Motor and ESC Calibration

The **`MOTOR and ESC`** tab is used to test the motors and ESCs in different directions. Follow the instructions in the software to calibrate the ESCs and test the motors:

![Figure 12.17 – The MOTOR TEST window](image/B19566_12_17.jpg)

Figure 12.17 – The MOTOR TEST window

## ESC Calibration

ESCs regulate motor speed (and direction) based on the PWM input value from the flight controller. The range of inputs to which an ESC will respond is configurable, and the default range can differ even between ESCs of the same model.

This calibration process updates all the ESCs with the maximum and minimum PWM input values that will be supplied by the flight controller. Subsequently, all the ESCs/motors will respond to the flight controller’s input in the same way (across the whole input range):

![Figure 12.18 – The ESC CALIBRATION window](image/B19566_12_18.jpg)

Figure 12.18 – The ESC CALIBRATION window

The steps for calibrating ESCs are as follows:

1.  Click on the **`Calibrate`** button and proceed with the instructions that appear on the screen. Follow all the steps to calibrate all the ESCs.
2.  Disconnect the USB and battery so that the flight controller powers down.
3.  Connect the battery. The arming tone will be played if the vehicle has a buzzer attached.
4.  Press the safety button (if the flight controller has one) until it turns a solid red color.
5.  Listen out for two beeps that play a musical tone.
6.  After a few seconds, listen out for several beeps (one for each battery cell you are using).
7.  Listen out for a final single long beep. This indicates the endpoints and that the ESCs have been calibrated.
8.  Disconnect the battery and power up again normally.

By following these instructions, we can successfully configure a drone. This guide was just an introduction to different settings. The skills you gain depend on practice and reading. The more we practice, the more we understand things. You will encounter many hurdles on your first try.

The preceding configuration can also be done with Herelink and its internally built QGC app. Please follow the Herelink guide to learn how to configure a drone with it.

# Summary

In this chapter, we finished developing a survey drone. The important part of this process is selecting the correct components.

We have seen that a basic airframe can be chosen; however, we can design and improve the aesthetics of the drone as we want. We also studied drone configurations through AeroGCS. We used the Herelink system for telemetry and RC control, and its configuration can be learned through its datasheet. Then, we took a quick look at what we must do after assembling a drone. As mentioned previously, practice makes perfect; the more we study and practice, the more we’ll be close to perfection. Getting into the habit of studying datasheets and integration manuals will be highly beneficial in this regard.

# References

This book was written with the help and assistance of resources from authors of other books. I also give due credit to all the resources and images that helped to complete the book and allowed me to deliver it to you. The following are the references and resources used in each chapter.

## Chapter 1: Getting Started with UAV and Drone Engineering

*   *Introduction to Unmanned Aircraft **Systems**: [https://ftp.idu.ac.id/wp-content/uploads/ebook/tdg/MILITARY%20PLATFORM%20DESIGN/Unmanned%20Aircraft%20Systems.pdf](https://ftp.idu.ac.id/wp-content/uploads/ebook/tdg/MILITARY%20PLATFORM%20DESIGN/Unmanned%20Aircraft%20Systems.pdf)
*   *Applications of Unmanned Aerial Vehicles: A **Review**: [https://3ciencias.com/wp-content/uploads/2019/11/art-5_special-issue_3C-TECNO_november_2019-1.pdf](https://3ciencias.com/wp-content/uploads/2019/11/art-5_special-issue_3C-TECNO_november_2019-1.pdf)
*   *Different Types of Drones and Uses (2024 Full **Guide)**: [https://www.jouav.com/blog/drone-types.html](https://www.jouav.com/blog/drone-types.html)
*   *Drone Types: Multi-Rotor Vs Fixed-Wing Vs Single Rotor Vs Hybrid **VTOL**: [https://www.auav.com.au/articles/drone-types/](https://www.auav.com.au/articles/drone-types/)
*   *What are the parts of a Drone? Full **list**: [https://umilesgroup.com/en/what-are-the-parts-of-a-drone-full-list/](https://umilesgroup.com/en/what-are-the-parts-of-a-drone-full-list/)
*   *Anatomy of A Drone - What’s inside a DJI Phantom **Drone**: [https://www.dronefly.com/the-anatomy-of-a-drone](https://www.dronefly.com/the-anatomy-of-a-drone)
*   *Bicopter - The journey so **far**: [https://discuss.ardupilot.org/t/bicopter-the-journey-so-far/59328](https://discuss.ardupilot.org/t/bicopter-the-journey-so-far/59328)

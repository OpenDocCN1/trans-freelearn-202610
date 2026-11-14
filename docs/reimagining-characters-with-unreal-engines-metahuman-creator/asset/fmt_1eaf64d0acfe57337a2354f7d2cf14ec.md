
## Prerequisites

*   **System performance**: We need to streamline as much as possible so that our systems and Unreal Engine are only concerned with getting as much data at the highest possible frame rate as possible. Therefore, processor-intensive tasks such as photorealistic lighting, high levels of lighting, and camera effects need to be scaled down. Also, ensure that no shaders are prepared in the background and that no other applications are running.
*   **Take Recorder**: We'll be using the Take Recorder plugin to record the facial motion capture, so you need to ensure it is enabled.
*   **The Level Sequencer**: We'll be using the Level Sequencer to import and play back the facial mocap and for refining any animation with it if need be.

I'll guide you through the process of capturing the facial capture with your phone step by step:

## Capturing Facial Motion with iPhone

### Initial Setup
1.  Ensure your phone is in a fixed position with plenty of light and that the front camera is looking directly at the performer.
2.  Ensure that Live Link is receiving the signal (remember that you want to see a green dot next to the name of your phone).
3.  Then, make sure that all rendering settings are turned to `Low` and complex scene actors are either hidden or removed from the scene. See Figure 8.14 as an example of a scaled-down viewport setting using the `Unlit` and `Low` settings:

![Figure 8.14: Scalability set to Low and Unlit](image/B17878_01_14.jpg)

Figure 8.14: Scalability set to Low and Unlit

### Configuring Take Recorder
1.  Open `Take Recorder` by going to `Window` > `Cinematics` > `Take Recorder`.
2.  Then, add the character blueprint by clicking on the `+ Source` button. Run a search for your character's blueprint under `From Actor` and add your phone from the `From LiveLink` option, as per Figure 8.15.

![Figure 8.15: Adding sources to Take Recorder](image/B17878_01_15.jpg)

Figure 8.15: Adding sources to Take Recorder

### Recording the Session
1.  When you're ready, first run a simulation by hitting `Alt + P` and then hit the large red record button.

As soon as you run the simulation, the motion capture will begin, along with a 3-second countdown, similar to Figure 8.16:

![Figure 8.16: The 3-second countdown](image/B17878_01_16.jpg)

Figure 8.16: The 3-second countdown

Immediately upon recording, the Blueprint track and the iPhone track will be written as a take and a reference of this will temporarily occupy the Level Sequencer to show which tracks are actively recording:

![Figure 8.17: The tracks being recorded](image/B17878_01_17.jpg)

Figure 8.17: The tracks being recorded

When you are happy with your mocap session, hit the stop button on Take Recorder and stop your simulation by hitting `Esc`.

### Reviewing in Level Sequencer
1.  Going back to the Level Sequencer, you can now take a look at your facial mocap. If you don't have anything in your Level Sequencer, you will need to add your character Blueprint, click on `+ Track`, then `Actor to Sequence`, and search for your character as per Figure 8.18:

![Figure 8.18: Importing the character Blueprint into Level Sequencer](image/B17878_01_18.jpg)

Figure 8.18: Importing the character Blueprint into Level Sequencer

Note that in Figure 8.18, when you bring in your character, you also bring in a rig for both the face and another for the body. However, because we are driving the character with mocap, you need to delete these rigs.

1.  Once you've deleted the rigs from Level Sequencer, click on `Face`, then `+Track`, then `Animation`, and the most recent mocap session will be at the bottom of the list. In my case, I'm choosing the first take, `BP_Glenda_Scene_1_01_0`.

![Figure 8.19: Adding an actor to the Level Sequencer](image/B17878_01_19.jpg)

Figure 8.19: Adding an actor to the Level Sequencer

1.  Once you have selected a take, press the play button on the Level Sequencer to watch your motion capture play back in real time.

If you still have the character Blueprint from the previous chapter with the retargeted animation, you can import the body animation. In addition, if your performance requires speech, the Level Sequencer is capable of playing back audio if you wanted to test your performances inside Unreal Engine. This is useful when ensuring that the facial capture is in synchronization with the audio of the speech recorded.

> **Note**
> It is advised that you capture the body and face simultaneously because most performers lead with their heads and the head leads the body. Getting both the body and head in sync can only really be done properly when both elements are captured at the same time.

The iPhone solution can work simultaneously with DeepMotion Animate 3D. To do this, you need a facial capture rig to ensure that the iPhone's depth camera is facing you at all times and that your hands are free. Ideally, the camera needs to be at a consistent distance at all times. Any movement of the camera separate from the head will be understood as a head movement and will lead to inaccurate data.

Companies such as MOCAP Design specialize in creating helmets that work with iPhones. You can see some of their iPhone and GoPro camera solutions at [https://www.mocapdesign.com/m2-headcam](https://www.mocapdesign.com/m2-headcam).

This solution may be expensive to most so, alternatively, a DIY solution or 3D printed solution may be more cost-effective. Additionally, there are now solutions for live streamers to keep their phone cameras at a consistent distance. While not perfect, they are certainly better than holding a phone in one hand or on a tripod.

## Summary

In this chapter, we have explored the iPhone's facial motion capture solution for MetaHumans in Unreal Engine. We used the Live Link app, along with Live Link and ARKit plugins, to capture live data. Additionally, we got to use the Take Recorder plugin in conjunction with the Level Sequencer as a way to record and manage our mocap.

In the next chapter, we will continue our facial motion capture but on a much more professional level, using Faceware.

# Chapter 9: Using Faceware for Facial Motion Capture

In the previous chapter, we used an iPhone to capture facial data via the Live Link Face App. While that is a great solution, it does have its limitations. One such limitation is how little control we had with regard to calibration; we were only able to calibrate to a neutral pose.

In this chapter, we're going to look at a more professional solution that is used in both AAA games and film projects. Unlike the iPhone's depth camera, we are looking at a video camera-based solution that provides us with a lot more calibration control: the solution is Faceware Studio.

Not only will we be taking advantage of the improved calibration when it comes to facial motion capture for our MetaHuman, we will also be looking at the additive tool in the Level Sequencer too.

So, in this chapter, we will cover the following topics:

## Topics Covered

*   Installing Faceware Studio on a Windows PC
*   Installing Unreal's Faceware Plugin and the MetaHuman sample
*   Setting up a webcam and the streaming function
*   Enabling Live Link to receive Faceware data
*   Editing the MetaHuman Blueprint
*   Recording a session with a Take Recorder
*   Importing a take into the Level Sequencer
*   Baking and editing facial animation in Unreal

## Technical Requirements

In terms of computer power, you will need the technical requirements detailed in [Chapter 1](#B17878_01.xhtml#_idTextAnchor015) and the MetaHuman plus the Unreal Engine Mannequin that we imported into UE5 in [Chapter 2](#B17878_02.xhtml#_idTextAnchor027).

You will also need:

*   A webcam and a well-lit room
*   A stable internet connection as you will be downloading plugins
*   Faceware Studio

> **Note**
> A word of warning: Faceware Studio is not a free solution; however, it is cheaper than a second-hand iPhone X. For individual users, there is an Indie pricing plan, which is €15 per month (at the time of writing). Alternatively, you can request a free trial.

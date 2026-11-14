
## Baking Facial Capture to Control Rig

Building on the previous chapter, where we added facial motion capture using the iPhone, we will apply the process of baking animation keyframes to the Faceware Studio Capture.

Similar to how we baked motion capture data onto a body control rig, we are going to do the same with the face rig and then create an additive section to make animation adjustments:

1. With `Face` selected, right-click and choose `Bake to Control Rig`, followed by `Face_ControlBoard_CtrlRig`, as per `Figure 9.29`:

![Figure 9.29: Baking the facial capture Animation Asset to the control rig](image/B17878_09_29.jpg)

Figure 9.29: Baking the facial capture Animation Asset to the control rig

Once we’ve baked to the control rig, we should see the track looks similar to `Figure 9.30`:

![Figure 9.30: The mocap data baked into the control rig](image/B17878_09_30.jpg)

Figure 9.30: The mocap data baked into the control rig

1. For safety, we want to lock the baked track so that we don’t inadvertently edit it. Do this by right-clicking on the track and choosing `Locked`, as per `Figure 9.31`:

![Figure 9.31: Locking the baked track](image/B17878_09_31.jpg)

Figure 9.31: Locking the baked track

1. With the control rig track locked, we can create an additive track for adding additional animation. It is effectively a duplicate track without any keyframes. To do this, click on `+ Section` next to `Face_CcontrolBoard_CtrlRig` and choose `Additive`, as per `Figure 9.32`.

![Figure 9.32: Creating an additive section](image/B17878_09_32.jpg)

Figure 9.32: Creating an additive section

1. Once you’ve created the additive track, the next step is to add keyframes to it. However, before doing that, you might want to double-check to see that the original motion capture track is locked. It will have a red border around it, as shown in `Figure 9.33`, if it is locked.

![Figure 9.33: Locked track and new track](image/B17878_09_33.jpg)

Figure 9.33: Locked track and new track

Also, take notice of the fact that there are no keyframes next to all of the controllers in the new additive track. This is exactly what we want.

Now let’s look at editing the face. During the `Take Recorder` session, and after I played back the mocap in the `Level Sequencer`, I noticed that on occasion, the right eye would blink out of time with or randomly on its own. This isn’t a physical effect of me writing this book but more than likely due to my wearing glasses during the recording.

To fine-tune this issue, you can run a search in the Sequencer for something simple, such as `eye blink`. This will come up with a list of the parameters related to this movement:

![Figure 9.34: Picking a controller to edit](image/B17878_09_34.jpg)

Figure 9.34: Picking a controller to edit

Using the `Unreal MetaHuman controllers`, I was able to set just a few keyframes to fix the eye issue. You can see from `Figure 9.35` that I laid down the first keyframe at a point where the mocap was correct, the third keyframe where the mocap was correct again, and the second keyframe between them to correct the problem frames.

![Figure 9.35: Making slight adjustments to Animation Controllers](image/B17878_09_35.jpg)

Figure 9.35: Making slight adjustments to Animation Controllers

If we only created the second keyframe, it would impact all the animation on the relevant track. We create the first and third keyframes to protect the animation outside the animation range of the first and third keyframes.

We are only touching the surface of editing motion capture data with the `Level Sequencer`. From the example I have given, I just used an additive section to fix a problem. It is a great function to know how to use, particularly when it comes to motion capture sessions. The reason is that you will know when a mocap session is good enough and whether it is cost effective to tweak the animation later or keep on going for a better take. In addition to just tweaking, you can use the techniques described in this section to make radical changes to your animation.

## Summary

In this chapter, we have explored the `Faceware` Facial motion capture solution for `MetaHumans` in `Unreal`. Much of what we covered in this chapter was similar to the iPhone solution but with the bonus of being able to use a simple webcam, and we looked at how we can fine-tune our data before `Unreal` receives it.

On top of this, we had some time to revise what we have learned about the `Take Recorder`, and we learned how to bake our facial motion capture data into a control rig in a similar way to how we baked our body motion capture from `DeepMotion`. Finally, we uncovered some of the power of using the additive feature in conjunction with the `MetaHumans`’ many facial animation controllers to fix issues.

In the next chapter, we will build on this knowledge as we look at how to merge multiple takes inside the `Level Sequencer` and explore further ways to render videos.

# Chapter 10: Blending Animations and Advanced Rendering with the Level Sequencer

In the previous chapters, you’ve been equipped with a choice of facial motion capture methods, be it using an iPhone or a webcam with `Faceware`. You’ve also learned how to acquire custom body motion capture with `DeepMotion`. One common thread in all of this is the `Level Sequencer`.

In this chapter, we will come back to the `Level Sequencer` to manage our mocap animation data. You will learn how to manage both body motion capture and facial motion capture, fix timing issues, and merge takes.

In addition, we will look at some of the more advanced industry-standard rendering techniques and explore additional items that we can use with the `Level Sequencer`.

So, in this chapter, we will cover the following topics:

* Adding the `MetaHuman Blueprint` and body mocap data to the `Level Sequencer`
* Adding facial mocap data to the `Level Sequencer`
* Exploring advanced rendering features

## Technical Requirements

In terms of computer power, you will need the technical requirements detailed in [*Chapter 1*](#B17878_01.xhtml#_idTextAnchor015) and the `MetaHuman` plus the `Unreal Engine Mannequin` that we imported into `UE5` in [*Chapter 2*](#B17878_02.xhtml#_idTextAnchor027).

If you have followed along with all of the chapters, you will also have `Mixamo` and `DeepMotion` data from [*Chapter 5*](#B17878_05.xhtml#_idTextAnchor071) and [*Chapter 6*](#B17878_06.xhtml#_idTextAnchor084), respectively. We will be using that data here.

## Adding the MetaHuman Blueprint and Body Mocap Data to the Level Sequencer

In this section, you will once again add your `MetaHuman` to the `Level Sequencer` so that you can apply both body capture data and facial capture data to it.

### Adding the MetaHuman Blueprint

To start, first, create a new `Level Sequencer` by right-clicking anywhere within your content browser, finding `Animation` in the menu, and clicking `Level Sequencer`.

Once your new `Level Sequencer` has been created, click on `+Track` and add your `MetaHuman Blueprint` as per `Figure 10.1` (if you don’t see it, use the search function and type in `BP` for Blueprint):

![Figure 10.1: Adding the Blueprint to the Level Sequencer](image/B17878_10_01.jpg)

Figure 10.1: Adding the Blueprint to the Level Sequencer

As soon as you have your `MetaHuman Blueprint` in your Sequence, you’ll notice that both the `Body` and `Face` control rigs are present, which is standard when you add a `MetaHuman Blueprint` to a `Level Sequencer`:

![Figure 10.2: The Body and Face control rigs](image/B17878_10_02.jpg)

Figure 10.2: The Body and Face control rigs

For the moment, we don’t need the control rigs because we’re just learning how to manage our animation data, which we will add in the very next section. So, delete both rigs highlighted in the `Level Sequencer` in `Figure 10.2`.

With the `Level Sequencer` still open, we are now ready to add our body mocap data.

## Adding Previously Retargeted Body Mocap Data


In this chapter, we are going to look at another way to make changes to our character's pose to fix minor issues, such as an arm colliding with the torso of the MetaHuman, but will also look at how we can refine the motion of the character. For this, we will use the `Level Sequencer`, which is the tool we use for animating anything in Unreal Engine, and in many ways, works like a timeline in a video editing application.

So, in this chapter, we will cover the following topics:

*   Introducing the Level Sequencer
*   Creating a Level Sequencer and importing our character Blueprint
*   Adding the retargeted animation to the character Blueprint
*   Adding and editing the Control Rig
*   Adding a camera to the Level Sequencer
*   Rendering a test animation from the Level Sequencer

# Technical requirements

In terms of computer power, you will need the technical requirements detailed in [*Chapter 1*](#B17878_01.xhtml#_idTextAnchor015) and the MetaHuman plus the Unreal Engine Mannequin that we imported into UE5 in [*Chapter 2*](#B17878_02.xhtml#_idTextAnchor027). Both of these will need to be saved in the same project in UE and you will need to have UE running for this chapter.

For this chapter, you will also need to have access to the motion capture data that you just retargeted in [*Chapter 6*](#B17878_06.xhtml#_idTextAnchor084).

# Introducing the Level Sequencer

Until now, you've only been able to preview your animation either within the character Blueprint or within the viewport using the `Play` button. The `Play` button is effectively simulating gameplay, which is purely for an interactive experience.

The `Level Sequencer` can be used in gameplay, but for the purpose of this book, we want to focus on using the `Level Sequencer` to make the most of our motion capture animation. A `Level Sequencer` is a timeline within our level where we can control our animations. It is similar to any other timeline in an editing program, except that instead of adding video clips, we add elements to our scene, and then apply animatable attributes.

When using Unreal Engine for animation, anything you want animated needs to be added to a `Level Sequencer`, including lights, cameras, actors, vehicles, and even audio, and you can have more than one `Level Sequencer` in any given project.

The beauty of the `Level Sequencer` is that it allows you to only focus on the animated objects, and also allows us to effectively tweak animation by adding independent keyframes without being overwhelmed by all the keyframes that come with the motion capture data. Using it, we can make changes to the motion capture animation without having to depend on getting absolutely perfect takes when working with a motion capture solution such as DeepMotion or motion capture library files such as Mixamo.

In the next section, we'll go through the process of getting a character into a `Level Sequencer`.

# Creating a Level Sequencer and importing our character Blueprint

So, let's get started with creating a `Level Sequencer` (there isn't a lot to it).

First, you need to right-click anywhere within your `Content` folder; in my case, I did just that within my `MetaHumans` folder, which you can see in *Figure 7.1*. Then, go to `Animation`, and then click on `Level Sequencer`.

![Figure 7.1: Creating a Level Sequencer ](image/B17878_07_01.jpg)

Figure 7.1: Creating a Level Sequencer

With the `Level Sequencer` created, you'll see a `Level Sequencer` clapper icon in the `Content` folder. Double-click on the `Level Sequencer` icon to open it. You will see an empty `Sequencer` as per *Figure 7.2*:

![Figure 7.2: An empty Level Sequencer ](image/B17878_07_02.jpg)

Figure 7.2: An empty Level Sequencer

In order to animate using the `Level Sequencer`, you need to add an actor. An actor can be pretty much anything that you want to animate. Once an actor is added to a `Sequencer`, an animation track is created.

Because we want to animate our MetaHuman character, we will need to add the MetaHuman Blueprint as our animation track. To do this, simply click on `+ Track` and then `Actor To Sequencer`:

![Figure 7.3: Adding an Actor to the Sequencer ](image/B17878_07_03.jpg)

Figure 7.3: Adding an Actor to the Sequencer

In *Figure 7.3*, you can see a list of actors that are available to add as tracks. At the very top of the actor list is `BP_Glenda`, which is my MetaHuman character. Click on your MetaHuman Blueprint to import it into the `Level Sequencer` as an animation track.

Once you add the Blueprint to the `Sequencer`, you will get two tracks: one for the body and one for the face. As you can see from *Figure 7.4*, each of these tracks has a `Control Rig`. Initially, the `Face` Rig should be selected, which will reveal a yellow interface on the viewport with controls to animate the face (although we won't be looking at these).

![Figure 7.4: Adding the MetaHuman Blueprint ](image/B17878_07_04.jpg)

Figure 7.4: Adding the MetaHuman Blueprint

The other rig available to us is the `Body` Rig. If you select the `Body` track in the `Sequencer`, the rig will become visible in the viewport, now demonstrated in *Figure 7.5*:

![Figure 7.5: The MetaHuman Control Rig ](image/B17878_07_05.jpg)

Figure 7.5: The MetaHuman Control Rig

At this point, you have just gained a vast amount of control over your MetaHuman Character in terms of animation. Whether you are a seasoned animator or a novice, I recommend you experiment with these animation controls. In particular, experiment with rotating any of the colored components of the `MetaHuman Control Rig` as per *Figure 7.5*, before you move on to the next section.

With the character Blueprint added to the `Level Sequencer` as an actor, we are now ready to bring in some motion capture data, which we will do in the next section.

# Adding the retargeted animation to the character Blueprint

Before we continue to our DeepMotion animation into the `Sequencer`, I need to warn you that we are going to take a step that will seem somewhat counter-intuitive: we will delete the `Control Rigs` for both the body and the face only to end up adding a body `Control Rig` back in again. In this chapter, our aim is to add the motion capture data and then add an additional track of keyframe animation. However, to do that, we need to have the motion capture data baked into the `control rig` and then we need to have another `control rig` for additional animation.

So, let's delete the two current `Control Rigs`. You can do this by simply clicking on `Metahuman Control Rig` and `Face_ControlBoard_CtrlRig` in the `Sequencer` and hitting `Delete` on your keyboard.

Now, with `Body` selected in the `Sequencer`, click on the `+` icon, and then from `Animation`, search for `Retargeted`. This should call up the retargeted data created by the `IK Retargeter` from the previous chapter, which you can see in *Figure 7.6*:

![Figure 7.6: Adding the retargeted mocap data ](image/B17878_07_06.jpg)

Figure 7.6: Adding the retargeted mocap data

Once you have selected the retargeted mocap, it will appear on the timeline within the `Sequencer`.

Note

The animation will start at wherever your `play head` is on the timeline. The `play head` is the position on the `sequencer` timeline that it's currently paused at, and the frame the animation will start from. In *Figure 7.6*, my `play head` is at the frame numbered `0115` (indicated by the red arrow).

When you have imported the mocap animation as a track, you can reposition this track to start at any time by simply dragging the track left or right. The track itself includes the title of the animation displayed within it. In *Figure 7.7*, you can see the animation track under the horizontal blue line.

![Figure 7.7: The animation track ](image/B17878_07_07.jpg)

Figure 7.7: The animation track

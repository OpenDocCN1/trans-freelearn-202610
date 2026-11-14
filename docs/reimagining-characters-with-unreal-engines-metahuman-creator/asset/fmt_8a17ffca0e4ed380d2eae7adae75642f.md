
# What are IK Chains?

Put simply, an IK chain is like any other chain, such as a bicycle chain, where each link in the chain has a relationship with the next. This applies to limbs in a body where each bone makes up a link in a chain. So, each limb, such as the shoulder down to the hand, makes up one IK chain.

All an `IK Rig` does is simplify each rig into the following five IK chains:

*   `Spine` to `Head`
*   `Left shoulder/clavicle` to `Left hand`
*   `Right shoulder/clavicle` to `Right hand`
*   `Left thigh` to `Left foot`
*   `Right thigh` to `Right foot`

> **Note**
>
> Ensure that you create each chain in the order that I have written them here and that the order applies to both the source and target skeletons.

## The IK Retargeting Tab

To do this, let’s take a closer look at the `IK Retargeting` tab. In *Figure 4.15*, knowing that I just need five IK chains, I clicked on the `+Add New Chain` button five times:

![Figure 4.15: IK Retargeting tab for the source](image/B17878_04.15.jpg)

By default, the start and end bones for each chain are set to `None`. To change this, click on `None`, and type `Spine`. This will activate a search through all the available bones; from the results, we can see that this skeleton has three available bones titled `spine`. Select `spine_01`.

![Figure 4.16: Selecting the spine](image/B17878_04.16.jpg)

Selecting `spine_01` will make it the given `Start Bone`. Do the same for `End Bone` but this time, type `Head` to complete the chain.

With both `IK Rigs` displayed side by side, we get a visual guide for our first IK chain because when we select an IK chain, it turns bright green. To select a chain, click anywhere beside any one of the fields (that could be between the `Start Bone` and the `End Bone` fields or even just next to the trash can icon). You can see from *Figure 4.17* that I have highlighted the first IK chain by clicking next to the `End Bone` field on each. This, in turn, highlights the chain in both viewports, which works as a great visual aid.

![Figure 4.17: Visualizing the first IK chain](image/B17878_04.17.jpg)

Now, we have an IK solution for both skeletons even though the skeletons are different.

Referring back to the list at the start of the section, let’s continue with the second IK chain. For the source `IK Rig`, choose `Clavicle Left` for `Start Bone` and `Hand Left` for `End Bone`. Then, do exactly the same for the target `IK Rig`.

By following the table and the previous instructions, you will be able to create the third, fourth, and fifth IK chains yourself, so that your source and target `IK Rigs` look similar to *Figure 4.18*. As you can see, I have selected the last IK chain, which is `Left Leg`. You can see in both Rigs that `Start Bone` is `thigh_l` and `End Bone` is `foot_l` and they are selected, and that this is represented in both viewports, with the relevant chain in each character highlighted in green:

![Figure 4.18: Side-by-side visualization of IK chains from both the source and target Rigs](image/B17878_04.18.jpg)

This precise matching between the source and target Rig is vital for the animation to be translated correctly. With that done, in the next section, we are going to create an `IK Retargeter`.

# Creating an IK Retargeter

With the hard work out of the way, now it is time to create a tool that can make these two `IK Rigs` talk to each other. This is effectively what an `IK Retargeter` does, translating the animation from the IK chains of one character onto another. In this example, I have gone back to my `Glenda` folder to create the Retargeter. So, let’s go through the following steps:

1.  Just like when we were creating the `IK Rig`, right-click anywhere in your character’s `Blueprint` folder, choose `Animation`, and select `IK Retargeter`, just like in *Figure 4.19*:

    ![Figure 4.19: Creating the IK Retargeter](image/B17878_04.19.jpg)

2.  When creating the `IK Retargeter`, you are prompted to pick the `IK Rig` from which you want to copy the animation. To make things easier for ourselves, we already labeled the `IK Rig` `SOURCE_Mannequin` earlier so that we could easily identify it as a source. You can see this available in the list in *Figure 4.20*:

    ![Figure 4.20: Picking the source IK Rig](image/B17878_04.20.jpg)

3.  Once picked, we will see another dialog box that shows us the source skeleton. Find the `Target IKRig Asset` option, which I have highlighted in red in *Figure 4.21* (we will come to the option highlighted in green in a moment):

    ![Figure 4.21: The IK Retargeter dialog box](image/B17878_04.21.jpg)

    The drop-down list beside it will show any available `IK Rigs`. The target `IK Rig` I want is aptly labeled `TARGET_Glenda`:

    ![Figure 4.22: Selecting the target IK Rig within the IK Retargeter](image/B17878_04.22.jpg)

    When selecting `Target IKRig Asset`, you should see a `MetaHuman` mesh appear next to `Mannequin`. If you don’t see it, return to *Figure 4.21*, where I have highlighted the `Target Preview Mesh` selection box in green.

4.  Now, in *Figure 4.23*, you can see how the `IK Retargeter` gives you a viewport with both the `Mannequin` character and the `MetaHuman` character next to one another. In addition, note that you have two tabs at the bottom of the screen: `Chain Mapping` and `AssetBrowser`. Make sure you’re looking at the `Chain Mapping` tab and click on the `Auto-Map Chains` button.

    ![Figure 4.23: Chain Mapping and Auto-Map options in the IK Retargeter](image/B17878_04.23.jpg)

5.  Next, switch over to the `AssetBrowser` tab, where you will see the available animations. In my case, I have just one animation available titled `Jog_Fwd` (you may have more). Click on one and the viewport will update with the animation applied to your `MetaHuman`.

    ![Figure 4.24: Exporting selected animations](image/B17878_04.24.jpg)

6.  You can now export that animation so that it is retargeted to your `MetaHuman`. If you have more than one animation available, you can select them as well. To export, select the animation and click on `Export Selected Animations`. Here, you will see a dialog box asking you to select an export path. I suggest that you choose your `MetaHumans` folder to which to export the retargeted animations, as I have done in *Figure 4.25*:

    ![Figure 4.25: Choosing a directory in which to save your retargeted animations](image/B17878_04.25.jpg)

> **Note**
>
> When choosing the folder and pressing `OK`, the `IK Retargeter` will create a new file using the animation name. In my case, it was `Jog_Fwd`. The file created will have the `Retargeted` suffix, which is a convenient way of differentiating the original animation from the retargeted animation files.

At this stage, you have seen your animation on your `MetaHuman`, but only in the `IK Retargeter` viewport. To get the animation in the scene, one way of doing this is to open up your character Blueprint (in my case, it is `BP_Glenda`).

7.  With the Blueprint open, you can now choose the retargeted animation in the `Animation` section under the `Anim to Play` drop-down menu, or you can just drag and drop your retargeted animation into the box beside it as per *Figure 4.26*:

    ![Figure 4.26: Applying the retargeted animation to the character Blueprint](image/B17878_04.26.jpg)

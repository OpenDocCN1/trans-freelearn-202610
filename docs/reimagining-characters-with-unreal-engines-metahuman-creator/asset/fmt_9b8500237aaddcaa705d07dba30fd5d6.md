
## Mixamo Export Settings

### Skin
The default setting is `With Skin`; this means that when you download the file, it will contain your original mesh along with the Mixamo Rig and the animation all within the FBX file format. If you have multiple animations for your character, you only need to include the skin on the first animation you download. Choosing `Without Skin` will contain the animation data only.

> **Note**
>
> The first time that you download your animation, keep all of the default settings. However, once you’ve downloaded the first animation with `Skin` intact, you don’t need the skin for any subsequent animation downloads for the same character.

### Frames per Second
The options are `24`, `30`, and `60` fps. Higher frame rates are good if you wish to edit the timing of your animation within Unreal Engine, particularly if you want to slow it down. Most motion capture is captured at a higher frame rate, allowing for retiming of the motion. It is better to have that higher frame rate than to attempt to interpolate slow motion in Unreal synthetically in an advanced workflow. A lower frame rate would be good if you are taking in really long animations and running them on a substandard machine.

### Keyframe Reduction
This is best left alone. The data size of these files and Unreal’s ability to read them don’t require keyframe reduction or optimization. Without keyframe reduction, Mixamo bakes a keyframe for each joint rotation on every frame. With keyframe reduction, Mixamo adds a little math in order to reduce the data size and this can reduce the quality of the animation. It’s best for Unreal Engine to get that data to work. If there is any keyframe reduction required in Unreal, there are plenty of tools within Unreal to give you better and controllable results.

## Downloading the Animation
After clicking `DOWNLOAD`, Mixamo will ask you where you want to download your new FBX file; choose somewhere you will remember, ideally somewhere inside your project folder, as this will allow you to skip the following step.

If you save it directly to somewhere within your project `Content` folder and you have Unreal running, you will get a prompt as per Figure 5.18 (which you will see in a moment) to allow the import to take place.

However, let’s assume it’s saved somewhere else so we can now take a look at manually importing your animation in the next step.

# Importing the Mixamo Animation into Unreal

### Preparing the Import Folder
If you’ve downloaded your FBX to some arbitrary location, you will need to import it into Unreal from the same location. To do that, first, create a new folder inside your Unreal Project and call it `MIXAMO_ANIMATIONS`. Double-click the folder so that you are inside it and then right-click to see the following menu:

![Figure 5.17: Importing FBX animation](image/B17878_05_17.jpg)

Figure 5.17: Importing FBX animation

To import the animation and the Mixamo Rig, choose `Import to`, along with the folder you are about to import the asset into. You can see from the highlighted text in Figure 5.17 that I am importing the asset into `Game/MIXAMO_ANIMATIONS`.

### FBX Import Options
Once selected, it will bring up `FBX Import Options`, as shown in Figure 5.18:

![Figure 5.18: FBX Import Options](image/B17878_05_18.jpg)

Figure 5.18: FBX Import Options

In Figure 5.18, you can see that there is a wealth of options to edit. Ultimately, the `FBX Import Options` dialog box allows us to import the Mixamo Rig, the Mixamo animation, the geometry of our MetaHuman character, and the skin weights associated with it. Importing all these components into Engine is essential for us to retarget the animation onto our MetaHuman character correctly.

However, if you’ve followed all the previous steps mentioned in this chapter, you’ll find there’s very little to do here. The default settings will be enough, with the exception of the following two:

*   **Mesh Skeleton**: You need to choose what skeleton you want to use the Mixamo Rig on. Unfortunately, we can’t use the MetaHuman skeleton with the Mixamo Rig directly, as it’s far too complex; instead, we will use the next best thing, `SK_Mannequin`, which will be available once you click on the drop-down list, as shown in Figure 5.18.
*   **Import Animations**: It may seem a little obvious, but yes, we need to make sure that this `Import Animations` box is checked so that we can import the animation that has been embedded within the FBX file.

> **Note**
>
> If you have already gone through this process of downloading the Mixamo Rig, skin weights, and animation, you will see that your Mixamo Rig is now an option to choose from `FBX Import Options` instead of `SK_Mannequin`.

### Configuring Import Settings
Once you have ensured you’ve selected the correct `Mesh Skeleton` and checked `Import Animations`, click on `Import All`. This will take a few minutes to load into your project. Once imported, you will find that you now have newly imported assets, looking similar to Figure 5.19:

![Figure 5.19: Imported assets](image/B17878_05_19.jpg)

Figure 5.19: Imported assets

### Imported Assets
At this point, we’ve just successfully brought in the Mixamo animation, along with a rig that has all the skin weights enabled to properly manipulate the MetaHuman body mesh. From Figure 5.19, you can see those assets; from left to right, we have the following:

*   `Fbx_Default_Material_0`: This is the default material as exported from Mixamo. As we’re only interested in animation from Mixamo, we can ignore this.
*   `Macarena_Dance`: This is the Mixamo Rig that has the skin weights that we need.
*   `Macarena_Dance_Anim`: This is the actual keyframe data of each of the joints.
*   `Macarena_Dance_PhysicsAsset`: This was created automatically by Unreal during the import process, containing physics simulations that are beyond the scope of this book.

> **Note**
>
> To learn more about `PhysicsAsset` in UE, you can review the following link: [PhysicsAsset documentation](`https://docs.unrealengine.com/5.0/en-US/physics-asset-editor-in-unreal-engine/`).

### Setting Up IK Rigs for Retargeting
Currently, we have a Mixamo Rig but no skeleton, and because of this, we can’t apply the Mixamo animation to our MetaHuman Blueprint. So, just like in [Chapter 4](#B17878_04.xhtml#_idTextAnchor059), we need to use two IK Rigs so that we can repurpose the Unreal `SK_Mannequinn` and retarget animation from it to the MetaHuman. While it may be familiar to you, I’ve outlined the process again for our new Mixamo assets:

#### Creating IK Rigs
1.  In the new `Mixamo_Animations` folder, create a new IK Rig and call it `Source_RigMixamo`. Then, create another IK Rig and call it `Target_RigMeta`.

As a reminder on how to do this, look at Figure 5.20; you can see that when you right-click anywhere within the `Content` tab, you can choose `Animation` and then select `IK Rig` from the list:

![Figure 5.20: Creating an IK Rig](image/B17878_05_20.jpg)

Figure 5.20: Creating an IK Rig

#### Defining IK Chains
2.  With both IK Rigs created, the next step is to create five IK chains for each and ensure that the following chain order is completed:
    *   The spine to the head
    *   The left clavicle to the left hand
    *   The right clavicle to the right hand
    *   The left thigh to the left foot
    *   The right thigh to the right foot

#### Assigning Skeletons
3.  Using the IK Rigs, we need to use `SK_Mannequin` as the skeleton for the source and leave the `Glenda_Med_Ovw` skeleton as the target.

Just like in the previous chapter, I recommend opening up both the source and target IK Rigs and putting them side by side, as shown in Figure 5.21. This way, you can ensure that you are getting them to match as much as possible in terms of the IK chains and the order in which the IK chains are created.

![Figure 5.21: IK Rigs side by side](image/B17878_05_21.jpg)

Figure 5.21: IK Rigs side by side

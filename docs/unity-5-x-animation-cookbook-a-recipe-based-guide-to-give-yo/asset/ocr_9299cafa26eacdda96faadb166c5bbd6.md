# Importing skeletal animations

This first recipe shows how to import a skeletal animation from a 3D package. It assumes that you have an animation already prepared. We are going to bring the file to Unity and show where you can adjust the import settings. We will not jump into too much detail for now to make the import process as quick as possible. We will cover all the settings in further recipes.

# Getting ready

Most animations are created in external 3D packages such as Maya, 3ds Max, Motion Builder, or Blender 3D. Make sure you have prepared a 3D model with a skeleton, the model is skinned, and you have created at least one animation. You can also download the provided example; open the project in Unity and go to this folder: `Chapter 01 Working with animations\Recipe 01 Importing skeletal animations\Sheep Model`. You will find an imported FBX file called `Sheep.fbx` there.

# How to do it...

To import a skeletal animation, you need to follow these steps:

![](bbox=[0.1, 0.62, 0.2, 0.72])

+   1. Export a skinned model and its skeleton (called **rig** in Unity) to FBX format.
+   
+      If you are using Blender, you don't have to manually export the file to FBX format. Simply drag and drop the file to your `Assets` folder. Unity will use Blender FBX exporter in the background. Additionally, you will be able to open the file by double-clicking on it in Unity. After you edit and save it, Unity will reimport it again. It is a very convenient way of editing animations as you don't have to go through the export-import process each time you make a change.
+   
+   2. Drag and drop the exported file into any subfolder in the `Assets` folder in your project or go to **Assets | Import new asset** and choose your FBX file.
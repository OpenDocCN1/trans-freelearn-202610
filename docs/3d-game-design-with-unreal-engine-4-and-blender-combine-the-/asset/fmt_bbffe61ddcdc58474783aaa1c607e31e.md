
# Finalizing the Artifact Explosion

## Explosion Sequence Setup

![Using particle effects](OEBPS/Images/B05189_09_20.jpg)

Don't forget to tell the `Spawn Actor From Class` nodes to use our Top and Bottom chunk class `Blueprint`s.

Place the `Spawn Actor From Class` node next to the `Destroy Component` node and link them. The first thing we need to tell it is what class to spawn. Click the drop-down box and select the bottom `Artifact` chunk. The second thing it needs to know is where to spawn. This can be done by plugging a `Get Actor Transform` node (found using the search box in the `Palette`) into the `Spawn Transform` option. Now we are set to spawn! Repeat this step to set up a `Spawn Actor From Class` node for the top `Artifact` chunk.

Now fire that `Radial Impulse` and make this a true explosion!

![Using particle effects](OEBPS/Images/B05189_09_21.jpg)

Use these settings for the `Fire Impulse` node.

The node we need to use is `Fire Impulse`. The only thing this node needs is a `Target`. Grab our `Radial Impulse` component from the `Components` panel and plug it in.

To wrap things up we just need to do a little bit of housekeeping. At the end of this sequence, add a `Delay` and a `Destroy Actor` node. The `Delay` will be set to `4.0` seconds to give our sounds time to play, and the `Destroy Actor` will have `Self` as a `Target`. This will remove all of the class `Blueprint` from the level. However, it will leave the exploded chunks behind. Even though we used a class `Blueprint` to spawn them, they are separate Actors and will not be deleted when the rest of the class `Blueprint` deletes itself.

Make sure the sequence is connected up in order and test it. You should have a satisfying explosion that throws pieces of the `Artifact` around. If the player is too close, it will even throw them backward!

Explosions are so much fun to make and can be a great way to add a bit of action to any experience, but if you had fun building this sequence, you don't need to stop here. Try adding more explosion particle effects, or adding the tentacles as chunks for added effect.

![Using particle effects](OEBPS/Images/B05189_09_22.jpg)

The final result! I hope you all had as much fun as I did!

How oddly satisfying was that?! There is just something about building something amazing for a video game and then allowing our player to blow it up. With our game asset now properly set up, we can place it in our space station level and use it as a goal to drive our gameplay forward. In this chapter, we went through the process of bringing our fully textured and animated `Artifact` in to `Unreal` and setting it up to look great in the level. We used a class `Blueprint` setup to assemble the pieces and created a way for the player to interact with it, causing it open. We then added some additional components and programming that allowed the player to start a countdown and blow up our game asset in a fine display of fire, smoke, and flying chunks.

And with that, we are done! Congratulations on finishing the book! What you have created is a great skeleton of a game that can be expanded on to create something amazing and uniquely your own. You have two whitebox levels that can be filled with your own custom art created in Blender. You have two game assets that can be further customized to fit your game's specific needs. I would strongly encourage you to properly finish each of these levels and to keep practicing and polishing your skills. Become the game designer or game artist that you want to be and I truly look forward to playing your unique games in the future!

Stick with it and game on!

## Index

### A
- **animation**
  - handling: [How does Blender handle animation?](#ch08.xhtml)
- **animation tools**
  - using: [Using Blender's suite of animation tools](#ch08s03.xhtml)
- **asset**
  - designing: [Designing our asset – inspiration and concept art](#ch06.xhtml)
- **Average Island Scale**: [`Average Island Scale`](#ch07.xhtml)

### B
- **Blender**
  - installing: [Installing Blender](#ch01.xhtml)
  - URL: [Installing Blender](#ch01.xhtml)
  - object, exporting from: [Exporting our object from Blender](#ch04.xhtml)
  - multiple shapes, using within: [Using multiple shapes within Blender](#ch06s03.xhtml)
  - animation, handling: [How does Blender handle animation?](#ch08.xhtml)
  - suite of animation tools, using: [Using Blender's suite of animation tools](#ch08s03.xhtml)
- **blocking volumes**: [Whiteboxing a level for better asset creation](#ch05s02.xhtml)
- **Blueprints**
  - used, for adding interactive elements: [Adding interactive elements using Triggers and Blueprints](#ch02s03.xhtml)
- **Bridge Edge Loops tool**: [`Bridge Edge Loops tool`](#ch06s02.xhtml)

### C
- **class blueprint**
  - creating: [Creating a class blueprint to tie it all together](#ch09.xhtml)
  - creating, for artifact: [Creating a class blueprint to tie it all together](#ch09.xhtml)
- **complex objects**
  - unwrapping: [Unwrapping complex objects](#ch07.xhtml)
  - seams, creating: [Marking seams](#ch07.xhtml)
  - unwrap tool: [`Unwrap tool`](#ch07.xhtml)
  - stitch: [`Stitch`](#ch07.xhtml)
  - average island scale: [`Average Island Scale`](#ch07.xhtml)
  - pack islands: [`Pack Islands`](#ch07.xhtml)
  - Smart UV Project, using: [Using `Smart UV Project`](#ch07s02.xhtml)
- **Content Browser**
  - used, for start building level: [Using the Content Browser to start building the level](#ch02.xhtml)
- **cubes**
  - unwrapping: [Unwrapping Cubes](#ch07s03.xhtml)
- **Custom Marking Seams**
  - about: [Custom Marking Seams](#ch07s03.xhtml)
  - cubes, unwrapping: [Unwrapping Cubes](#ch07s03.xhtml)
  - cylinders, unwrapping: [Unwrapping Cylinders](#ch07s03.xhtml)
- **cylinders**
  - unwrapping: [Unwrapping Cylinders](#ch07s03.xhtml)

### D
- **3D asset**
  - setting up: [Setting up and using our new 3D asset](#ch04s04.xhtml)
  - using: [Setting up and using our new 3D asset](#ch04s04.xhtml)

### F
- **FBX**
  - about: [What is `FBX`?](#ch04s02.xhtml)

### G
- **game asset**
  - UV unwrapping: [UV unwrapping our game asset](#ch03s04.xhtml)
- **game assets**
  - making: [Getting started making game assets](#ch03.xhtml)

### I
- **interactive elements**
  - adding, Triggers used: [Adding interactive elements using Triggers and Blueprints](#ch02s03.xhtml)
  - adding, Blueprints used: [Adding interactive elements using Triggers and Blueprints](#ch02s03.xhtml)
- **interface**
  - exploring: [Exploring the interface](#ch01s02.xhtml)

### K
- **keyframes**
  - using: [Rigging and using keyframes](#ch08s02.xhtml)

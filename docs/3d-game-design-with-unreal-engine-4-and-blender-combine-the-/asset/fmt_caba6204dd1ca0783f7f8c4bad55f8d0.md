
# Advanced Scripting Techniques

## Brainstorming and Planning

Brainstorming using sticky notes is a great way to come up with ideas like this very quickly. The sticky notes make the ideas movable so that you can easily organize them into any order you choose.

Now there's a few great ideas! The next step is to arrange them into a logical order that can be programmed. Computers can't do anything we haven't specifically spelled out for them, so we have to be pretty clear about our instructions. Here is just one example of how I might order the instructions:

1.  Animate the doors and windows closed.
2.  Some kind of loud noise.
3.  Fire the smoke emitter.
4.  Fire the spark emitter.
5.  Play some clue to the player to let them know what to do to get out.

Seems like a pretty solid plan. The next step would be to add those additional elements to our level. Each entrance to the security checkpoint hallway needs a door, each window needs a shutter, and so on.

## Building Doors and Shutters

### Building the First Security Door

![Advanced scripting techniques](OEBPS/Images/BO5189_05_08.jpg)

1.  For the three security doors that slam down, I grabbed a `Wall_400x200` piece from the `Architecture` folder and adjusted the `Scale` of the `X` to `0.4` and the `Z` to `1.1`. I then changed the name for each to `Sec_Door1`, `Sec_Door2`, and `Sec_Door3`. Remember to also set the `Mobility` property to `Movable`.
2.  I positioned each door above each door frame so that it can slam down during the event:

![Building the security shutters](OEBPS/Images/BO5189_05_09.jpg)

### Building the Security Shutters

3.  For the two security window shutters, I used a `Wall_400x400` piece and adjusted the `Scale` property of the `x` to `1.5` and the `z` to `0.5`. I changed their names to `Sec_Win_Shutter1` and `Sec_Win_Shutter2` to make them easy to identify and set their `Mobility` to `Movable`.

## Adding Details

### Placing Particle Emitters

![Placing the particle emitters](OEBPS/Images/BO5189_05_10.jpg)

1.  Particle emitters are points in space that emit light, sparks, or even static meshes if we want them to. They are created using Unreal's Cascade Particle Editor, though the `Starter Content` folder has some great ones we can use in the `Particles` sub-folder. Here I used a `P_Smoke` emitter at each door and one `P_Sparks` emitter near the window on the left. For all of them, I turned off the `Auto Activate` option in the `Details` panel to give us control of when they fire.

A trap like this is great, but we still need some way for the player to escape:

![A view of the security office](OEBPS/Images/BO5189_05_11.jpg)

### Creating the Escape Route

2.  After a couple seconds, let's have the door to the security room malfunction and explode, allowing the player to reach the security override button! I have placed a couple of `Point Lights` above the door, one red and one green, that can indicate the door is opening (turn off the `Visible` property for the red light in the `Details` panel) and have built the override button using a `Box Trigger` and a `SM_MatPreviewMesh_02` from the `Props` folder of the `Content Browser`. To make sure that clue is clear to the player I have placed a `Text Render` object above the button reading `Disengage Security Lockdown`.

## Scripting the Event in the Level Blueprint

With all the pieces in place we are ready to script our event. Open the `Level Blueprint` by pressing the `Blueprints` button above the viewport and selecting `Open Level Blueprint`. For this level of scripting, we are also going to go up to the `Window` button in the toolbar and turn on the `Palette` window. This gives us access to a customizable `Favorites` list of nodes and the `Find a Node` search box, a great tool when constructing new Blueprint sequences:

![Beginning the Level Blueprint](OEBPS/Images/BO5189_05_12.jpg)

### Beginning the Level Blueprint

1.  Select both of the default nodes located in the Blueprint area and press `Delete`. We won't need them for this sequence.

### Setting Up the Trigger Event

2.  We need an event to start our sequence off. Back in the level, place a `Box Trigger` in the security checkpoint, about halfway down the hallway. Set its `Box Extents` property to be large enough to make missing it impossible. This will make the player trigger the security lockdown after they have completely entered the hallway. Inside the `Level Blueprint`, `Right-click` and select `Add Event for Trigger Box`, select `Collision`, and then select `Add OnActorBeginOverlap`. Remember that your trigger box number may vary from mine:

![The complete Blueprint for swapping our colored lights](OEBPS/Images/BO5189_05_13.jpg)

### Swapping Warning Lights

3.  Now, we need to change the warning light color. For this we are going to add a couple different nodes. In the search box of the `Find a Node` section of the `Palette` pane, type in `Do Once`. This node works like a gate and ensures that anything after it only runs once. Here, we use it to make sure that the trigger we use to start the sequence cannot be set off more than once. Next, search for the `Toggle Visibility` node. Drag two of them into the Blueprint and connect them as shown. The targets for these nodes will be our red and green lights. We want to make sure we are showing the light we want and hiding the light we don't want:

![Gathering our initial locations on our doors and windows](OEBPS/Images/BO5189_05_14.jpg)

### Gathering Initial Locations

4.  Time to slam the doors and windows shut! This part looks a bit complicated but is actually just like something we did in [Chapter 2](#ch02.xhtml "Chapter 2. Starting Our First Project"), *Starting Our First Project*. We are going to use a `Timeline` to shut all of the security doors we created at the same time. The first thing we need to do is save the initial locations of all the things we are going to animate. Without this step, the doors and windows would slam down then keep going right through the floor! To stop that from happening, we animate the door's position from its initial position. First, create some vector variables to hold the data for the three security doors and two windows. Next, we will build a special sequence to handle collecting that data. Start with an `Event BeginPlay` event and connect it to a `Set` node for each variable. Then connect a `Get Actor Location` node to each `Set`, with each door and window as a target. This will get the location of each piece and then set it as the variable so that we can animate it in the next step:

![The timeline that slides our doors and windows shut](OEBPS/Images/BO5189_05_15.jpg)

### Animating Doors and Windows with a Timeline

5.  Now add the `Timeline` node and double click it to open it up. We want to slide our doors and window shutters down into place using a `Vector Track`. Remember that in a `Vector Track`, we can animate all three axes of movement, but we only want to use the `Z` axis here, so it will take a bit of sorting. Hold the `Shift` key and click on the red line. This will add a keyframe to the `X`-axis. Now click on the new keyframe and drag it out of the way. Now do the same on the green line and drag the `Y`-axis out of the way. This leaves us with the blue line, the `Z` axis. Create a keyframe there and make sure its time and value are both set to `0`. Add a second keyframe and set the time to something fast. I used `0.25`. Set the value to `-210`. This will slide them down into place, blocking our player's escape. After creating your keyframes for the `Z`-axis, feel free to delete the keyframes on the other two axes by clicking them and pressing the `Delete` key. You can also lock and hide these axes by using the buttons located in the upper left portion of the graph. Be sure to select the `Last Keyframe` checkbox as well. This tells the `Timeline` to stop at the last keyframe in the sequence.

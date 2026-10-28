
Additive animations are used to move only specific parts of a character independently of the main skeletal animation. For instance, the skeletal animation contains only the walking or running part of the body, while the additive animation clip changes only the head or hands. Now the character can move the head to look around, without the need to create walking and running animations containing all possible head movements.

The combination of skeletal, morph, and additive animation enables us to build powerful and natural-looking characters for our virtual world, allowing the model to walk or run beside us, follow our movements with the head, and use facial morph animations to speak all at the same time.

Now let us look at the general workflow for creating character model animations. We can divide the animation workflow into two parts: preparation and updates. While the preparation part is needed only once while loading the model, updates are usually made for every frame drawn to the screen.

We will dive into the preparation process of the model first.

## Preparing the data for efficient usage

Game character models are stored in single files, or as a collection of files, each for a specific purpose. For instance, model and texture data could reside in separate files, allowing artists to change the images independently of the model vertices.

The following steps must be done in the application before the file in the model data can be used for animation and rendering:

- 1. As the first step in the preparation phase, these files must be loaded into the memory of the computer. Depending on the implementation in the game, partial loading is possible, adding only the elements of the character model that are needed at a specific level, or in a specific part of the virtual world.
- 2. Then, the data needs to be pre-processed. The representation of the data in the files on disk may be optimized in terms of saving space, but for efficient manipulation and rendering, a different kind of optimization is required.

For example, different rendering APIs, like OpenGL, Vulkan, and DirectX, may need slightly different representations of vertex or texture data, or shader code to be uploaded to the GPU. Instead of storing the different versions in the model file, a generic representation may be used. The required adjustments or conversions will be done after loading.

3. As the last step, static data like vertex data or textures will be uploaded to the GPU, and other static and variable data parts are stored in C++ classes and objects.

At this point, the model is ready to use. With the first appearance of that character on the screen, a continuously running task of data updates is needed. These per-frame tasks are required for states that change at runtime, such as positions or animation poses.

## Updating character data

Since the data of the character is split between main memory and GPU memory, the game or animation program must sample, extract, convert, and upload data for every single frame the character is drawn to the screen.

For instance, the key-frame data of the animations needs to be updated according to the animation clip to be shown and the time since the last frame.

The following steps must be done in every frame to create the pixels of a single model instance for a specific time point of a selected animation clip:

- 1. Blending between different animation clips could be requested by the program flow, and additional animation parts may be needed, like additive blending for the head or the hands, or facial animations to allow a facial expression for the character. So, we extract rotation, translation, and scaling for all nodes at the specified replay time from all animation clips and combine the per-clip node transformations into a final transformation matrix for every node.
- 2. After the animation data is sampled and combined, the skeleton of the character needs to be adjusted. According to the animation data, every virtual bone must be translated, rotated, and scaled to reach the desired destination.
- 3. Also, the world position of the character may need to be updated. World position changes can occur in different forms, like running around, jumping, or falling down. Knowing the exact position of all characters is an important part of the remaining steps.
- 4. Once bone positions and the world position of the character have been determined, collision detection can run. The collision detection checks if the character intersects with other characters or environmental objects like the floor and walls, or even if the character was hit by a projectile. As a reaction to the collision detection results, adjustments to the character properties, like position, or animation clip may be triggered.

## Working with Open Asset Import Library

- 5. Having the collision data at hand, inverse kinematics adjustments may run. Adjusting the skeleton data of the character could be needed to avoid character limbs intersecting with the wall or the floor, or to level the feet position on uneven ground.
- 6. Now, the pure CPU part of the character update is nearly over. As a final step from the CPU side, the updated animation data is uploaded to the GPU and the render commands are issued. By storing the dynamic character data in the GPU, the rendering itself can run without much additional workload from the CPU.
- 7. On the GPU, the vertex shader transforms the incoming vertex data according to the properties of the current view, like distances or perspective distortion. After other possible shader stages, the fragment shader receives the data from the rasterization stage of the GPU and draws the pixels to the output framebuffer.
- 8. After all pixels of the level data, characters, and the HUD, as well as maybe additional debug data, have been sent to the GPU, the visible and the drawing buffers are swapped – at this point, the character appears on the screen, at the position and in the animated pose we expect to see.

In between all these update steps, compute shaders may run on the GPU to calculate data. Running compute shaders allows the program to offload the calculations for multiple characters, freeing the CPU to work on other parts of the game.

As you can see, a lot of work is needed until you see a game character running around in the virtual world. Let’s now start the journey into character animation with an overview of Open Asset Import Library.

## What is Open Asset Import Library?

Open Asset Import Library, in short Assimp, is a cross-platform library to import and convert 3D model files. The different file formats are converted into a hierarchical data structure, enabling a programmer to support a wider range of model formats in a single, comprehensive way.

Figure 1.20 shows the key elements and their relationships:

![](img/3cfb9832c943f5f7366fd104b5255971_53_0.png)

Figure 1.20: A simplified version of Assimp's data structures

Let us take a closer look at these data structures and their functions:

- aiScene is the central element of Assimp's data structure. The root node entry, all information about the polygonal meshes, the materials, and the animations are stored in the aiScene element.
- The root node of aiScene points to a structure called aiNode. In every aiNode, possible child nodes are stored, eventually creating a node tree. Also, a transform matrix resides in the aiNode structure, defining the local transformation, relative to the parent node. This matrix is called the TRS matrix for the three possible transformations: translation, rotation, and scale, in this order. By combining the TRS transforms from the root node down to every node of the skeleton, the final world position of a node can be calculated only by using the world position of the root node and the local transformations of every node in the skeleton hierarchy.
- The node name is used in other structs, like the bones or the animations, to refer back to this specific node. For the vertices of the node, an index to the corresponding aiMesh structure in aiScene is stored here.

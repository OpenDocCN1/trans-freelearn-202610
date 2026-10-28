With the knowledge gained from this book, you will see the animations in games with a different pair of eyes, smiling a lot because you know how they’re made.

# Who this book is for

If you are already familiar with C++ and character animations but want to learn more about the implementation details and advanced character animation topics, similar to the animations found in 3D games, this book is for you.

# What this book covers

Chapter 1, Working with Open Asset Import Library, gives an overview of the data structures of Open Asset Import Library (or assimp) and explains how to load a character model from a file. The fileloading process will be enhanced by adding an ImGui-based Open File dialog. In addition, code to add and remove model and instances at runtime will be covered.

Chapter 2, Moving Animation Calculations from CPU to GPU, introduces compute shaders to offload calculations to the graphics processor and store lookup data in GPU memory. By using the massively parallel architecture of the GPU, computing the node positions of all instances will be accelerated, and the CPU will be free again for other tasks.

Chapter 3, Adding a Visual Selection, explains how to support the user of the application in the task of character instance selection. As well as highlighting the currently selected instance on the screen, the ability to select any instance by using the mouse will be added.

Chapter 4, Enhancing Application Handling, introduces a separation of the application into a view mode and an edit mode with partially different configurations. Also, undo and redo operations will be implemented.

Chapter 5, Saving and Loading the Configuration, covers storing the current configuration of the application plus the loaded models and created instances in a YAML file, as well as loading the configuration from a file back into the application.

Chapter 6, Extending Camera Handling, shows how different camera types and configurations can be added. The new camera settings include orthogonal projection and first-person and third-person views.
It means we will send a request to start 51,200 instances of the shader to the GPU driver:

```
16*32*1*10*10*1 = 51200
```

For more details about compute shaders, links to tutorials for OpenGL and Vulkan are available in the Additional resources section.

While there are some additional limitations, like the number of shader cores used together for the sake of simplified internal management (called a wave on AMD GPUs, and a warp on NVIDIA GPUs), the number of invocations shows the user-friendly usage of compute shaders.

You, the programmer, don’t need to care about spawning a massive number of threads in the code or joining them at the end of the program. Also, there is no need to create mutexes, or atomic variables, to control access to the data. All these steps are firmly hidden from your eyes in the depths of the graphics driver.

Though you are not free of obligations – you still have to make sure only a single shader invocation reads or writes a single buffer address. But, with the help of the control variables set by the GPU, like the global and local invocation IDs, this part is also easy – a lot easier compared to the efforts needed for manual multi-threading on the CPU.

So, how do we use the magic of the compute shaders in our program? The first step here is to analyze the hotspots in the code and to create a plan for how the same data could be computed on the GPU.

# Profiling animation performance

To test the performance of the application on your system, you can import the test model named Woman.gltf in the woman subfolder of the assets folder, move the slider next to the Create Multiple Instances button to 100, and click the button Create Multiple Instances several times. Every click will add another 100 instances of the model, distributed randomly across the virtual world.

Or, you can change the code for the instance slider in the createFrame() method of the UserInterface class in the opengl folder. Adjust the fourth parameter of the call, controlling the maximum value of the slider:

```
ImGui::SliderInt("##MassInstanceCreation",
    &manyInstanceCreateNum, 1, 100, "%d", flags);
```
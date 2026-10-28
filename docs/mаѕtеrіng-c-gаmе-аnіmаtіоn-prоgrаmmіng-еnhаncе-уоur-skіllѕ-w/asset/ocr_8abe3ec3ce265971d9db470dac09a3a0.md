Figure 14.10 shows a scene created by a ray tracing example from Sascha Willems Vulkan code (the code is available at https://github.com/SaschaWillems/Vulkan/tree/master/examples/pbr1bl):

![](bbox=[0.05, 0.18, 0.95, 0.68])

# Figure 14.10: Real-time ray tracing scene

The reflections on the floor, the spheres, and the teatpot in Figure 14.10 are calculated in real time by using ray tracing with the Vulkan API on an Nvidia RTX 4000 series GPU.

The FPS counter was included to show how fast the current GPU generation is able to create ray tracing images. Only a few decades ago, a single frame of the same picture required several days to render.

# Ray tracing should be optional

Note that the calculations for ray tracing require lots of computing power and creating complex scenes in real time needs both a GPU and a graphics API that support ray tracing. Right now, only Vulkan and DirectX 12 are able to use the ray tracing capabilities of modern GPUs. You need to check the availability of hardware and software support before switching to a graphics pipeline with ray tracing enabled.
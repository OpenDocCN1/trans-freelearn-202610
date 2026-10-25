

The main difference between these Lightmaps and other forms of Lighting is that Lightmaps are pregenerated (or baked) in the Editor and packaged into the game build. This ensures that we don’t need to keep regenerating this information at runtime, saving numerous Draw Calls and significant GPU activity. Since we can bake this data, we have the luxury of time to generate very high-quality Lightmaps (at the expense of larger generated texture files we need to work with, of course).

Since this information is baked ahead of time, it cannot respond to real-time activity during gameplay, and so by default, any Lightmapping information will only be applied to static objects that were present in the Scene when the Lightmap was generated and at the exact location they were placed. However, Light Probes can be added to the Scene to generate an additional set of Lightmap textures that can be applied to nearby dynamic objects that move, allowing such objects to benefit from pregenerated Lighting. This won’t have pixel-perfect accuracy and will cost disk space for the extra Light Probe maps and Memory Bandwidth at runtime to swap them around, but it does generate a more believable and pleasant Lighting profile.

There have been several techniques of generating Lightmaps developed throughout the years, and Unity has used a couple of different solutions since its initial release. Global Illumination is simply the latest generation of the mathematical techniques behind Lightmapping, which offers very realistic coloring by calculating not only how Lighting affects a given object, but also how light reflects off nearby surfaces, allowing an object to affect the Lighting profile of those around it. This effect is calculated by an internal system called Enlighten. This tool is used both to create static Lightmaps, as well as create something called Pre-computed Real-time GI, which is a hybrid of real-time and static Shading and allows us to simulate effects such as time-of-day (where the direction of light from the Sun changes over time) without relying on expensive real-time Lighting effects.

A typical issue with generating Lightmaps is the length of time it can take to generate them and get visual feedback on the current settings, because the Lightmapper is often trying to generate full-detail Lightmaps in a single pass. If the user attempts to modify its configuration, then the entire job must be canceled and started over. To solve this problem, Unity Technologies implemented the Progressive Lightmapper, which performs Lightmapping tasks more gradually over time, but also allows them to be modified while they are being calculated. This makes Lightmaps of the Scene appear to get progressively more detailed as it works in the background while also allowing us to change certain properties when it is still working and without having to restart the entire job. This provides almost immediate feedback and improves the workflow of generating Lightmaps immensely.

## Multithreaded Rendering

Multithreaded Rendering is enabled by default on most systems, such as desktop and console platforms whose CPUs provide multiple cores. Other platforms still support many low-end devices to enable this feature by default, so it is a toggleable option for them. For Android, it can be enabled via a checkbox under **Edit | Project Settings | Player | Other Settings | Multithreaded Rendering**, whereas for iOS, Multithreaded Rendering can be enabled by configuring the application to make use of Apple’s Metal API under **Edit | Project Settings | Player| Other Settings | Graphics API**. At the time of writing this book, WebGL does not support Multithreaded Rendering.

For each object in our Scene, there are three tasks to complete: determine whether the object needs to be rendered (through a technique known as _Frustum Culling_), and if so, generate commands to render the object (since rendering a single object can result in dozens of different commands), and then send the command to the GPU using the relevant Graphics API. Without Multithreaded Rendering, all of these tasks must happen on the main thread of the CPU, thus any activity on the main thread becomes part of the critical path for all rendering. When Multithreaded Rendering is enabled, the task of pushing commands into the GPU are handled by a _render thread_, whereas other tasks such as culling and generating commands get spread across multiple _worker threads_. This setup can save an enormous number of CPU cycles for the main thread, which is where the overwhelming majority of other CPU tasks take place, such as physics and script code.

Enabling this feature will affect what it means to be CPU bound. Without Multithreaded Rendering, the main thread is performing all of the work necessary to generate instructions for the Command Buffer, meaning that any performance we can save elsewhere will free up more time for the CPU to generate commands. However, when Multithreaded Rendering is taking place, a good portion of the workload is pushed onto separate threads, meaning that improvements to the main thread will have less of an impact on rendering performance via the CPU.

> Note that being GPU bound is the same regardless of whether Multithreaded Rendering is taking place. The GPU always performs its tasks in a multithreaded fashion.

## Low-level rendering APIs

Unity exposes a rendering API to us through their `CommandBuffer` class. This allows us to control the Rendering Pipeline directly through our C# code by issuing high-level rendering commands, such as `render this object, with this Material, using this Shader`, or `draw N instances of this piece of procedural geometry`. This customization is not as powerful as having direct Graphics API access, but it is a step in the right direction for Unity developers to customize unique graphical effects.

Check out the Unity documentation on `CommandBuffer` to make use of this feature at `http://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html`.

If an even more direct level of rendering control is needed, such that we wish to make direct Graphics API calls to OpenGL, DirectX, and Metal, then be aware that it is possible to create a Native Plugin (a small library written in C++ code that is compiled specifically for the architecture of the target platform) that hooks into the Unity's Rendering Pipeline, setting up callbacks for when particular rendering events happen, similar to how `MonoBehaviours` hook into various callbacks of the main Unity Engine. This is certainly an advanced topic for most Unity users, but useful to know for the future as our knowledge of rendering techniques and Graphics APIs matures.

Unity provides some good documentation on generating a rendering interface in a Native Plugin at `https://docs.unity3d.com/Manual/NativePluginInterface.html`.

## Detecting performance issues

It should be obvious that due to the number of complex processes involved, there are a lot of different ways in which the GPU can become bottlenecked. Now that we have a thorough understanding of the Rendering Pipeline and how bottlenecks may occur, let's explore how to detect these problems.

## Profiling rendering issues

The Profiler can be used to quickly narrow down which of the two devices used in the Rendering Pipeline we are bottlenecked within--whether it is the CPU or GPU. We must examine the problem using both the **CPU Usage** and **GPU Usage Areas** of the **Profiler** window, as this can tell us which device is working the hardest.

The following screenshot shows **Profiler** data for a CPU-bound application. The test involved creating thousands of simple cube objects, with no batching or Shadowing techniques taking place. This resulted in an extremely large Draw Call count (around 32,000) for the CPU to generate commands for, but giving the GPU relatively little work to do due to the simplicity of the objects being rendered:

![](img/da79484c9b30dc915216dbc5454a9bbf_224_0.png)

This example shows that the CPU's **Rendering** task is consuming a large amount of cycles (around 25 ms per-frame), whereas the GPU is processing for less than 4 milliseconds, indicating that the bottleneck resides in the CPU. Note that this Profiling test was performed against a standalone app, not within the Editor. We now know that our rendering is CPU bound and can begin to apply some CPU-saving performance improvements (being careful not to introduce rendering bottlenecks elsewhere by doing so).

Meanwhile, Profiling a GPU-bound application via the Profiler is a little trickier. This time, the test involves creating a simple object requiring minimal Draw Calls, but using a very expensive Shader that samples a texture thousands of times to create an absurd amount of activity in the Back End.


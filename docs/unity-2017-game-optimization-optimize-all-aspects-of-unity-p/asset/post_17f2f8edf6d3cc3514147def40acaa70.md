

## Dynamic Graphics Chapter 6

To perform fair GPU-bound Profiling tests, you should ensure that you disable Vertical Sync through Edit | Project Settings | Quality | Other | V Sync Count, otherwise it is likely to pollute our data.

The following screenshot shows Profiler data for this test when it is run in a standalone application:

![](img/da79484c9b30dc915216dbc5454a9bbf_225_0.png)

As we can see in the preceding screenshot, the rendering task of the CPU Usage Area matches closely with the total rendering costs of the GPU Usage Area. We can also see that the CPU and GPU time costs at the bottom of the image are relatively similar (about 29 milliseconds each). This is somewhat confusing as we seem to be bottlenecked equally in both devices, where we would expect the GPU to be working much harder than the CPU.

In actuality, if we drill down into the **Breakdown View** of the **CPU Usage Area** using the **Hierarchy Mode**, we will note that most of the CPU time is spent on the task labeled **Gfx.WaitForPresent**. This is the amount of time that the CPU is wasting while it waits for the GPU to finish the current frame. Hence, we are in fact bottlenecked by the GPU despite appearing as though we are bound by both. Even if Multithreaded Rendering is enabled, the CPU must still wait for the Rendering Pipeline to finish before it can begin the next frame.

![](img/da79484c9b30dc915216dbc5454a9bbf_226_0.png)

**Gfx.WaitForPresent** is also used to signal that the CPU is waiting on Vertical Sync to complete, hence the need to disable it for this test.

## Brute-force testing

If we're pouring over our Profiling data and still not sure where the source of the problem resides, or we're GPU-bound and need to determine where we're bottlenecked in the Rendering Pipeline, we should try the brute-force method, that is, cull a specific activity from the Scene and check whether it results in greatly improved performance. If a small change results in a big speed improvement, then we have a strong clue about where the bottleneck lies. There's no harm in this approach if we eliminate enough unknown variables to ensure that the data is leading us in the right direction.

The obvious brute-force test for CPU bounding will be to reduce Draw Calls to check whether performance suddenly improves. However, this is often not possible since, presumably, we've already been reducing our Draw Calls to a minimum through techniques such as Static Batching, Dynamic Batching, and Atlasing. This would mean that we have very limited scope for reducing them further.

What we can do, however, is intentionally increase our Draw Call count by a small number, either by introducing more objects or disabling Draw Call-saving features such as Static and Dynamic Batching and observe whether the situation gets significantly worse than before. If so, then we have evidence that we're either very close to being CPU bound or have already become so.

There are two good brute-force tests we can apply to a GPU-bound application to determine whether we’re bound by Fill Rate or by Memory Bandwidth: reducing screen resolution or reducing texture resolution, respectively.

By reducing screen resolution, we will ask the Rasterizer to generate significantly fewer fragments and transpose them over a smaller canvas of pixels for the Back End to process. This will reduce the Fill Rate consumption of the application, giving this key part of the Rendering Pipeline some additional breathing room. Ergo, if performance suddenly improves with a screen resolution reduction, then Fill Rate should be our primary concern.

![](img/da79484c9b30dc915216dbc5454a9bbf_227_0.png)

A reduction from a resolution of 2560 x 1440 to 800 x 600 is an improvement factor of about eight, which is often more than enough to reduce Fill Rate costs enough to make the application perform well again.

Similarly, if we’re bottlenecked on Memory Bandwidth, then reducing texture quality is likely to result in a significant performance improvement. By doing so, we have shrunk the size of our textures, greatly reducing the Memory Bandwidth costs of our Fragment Shaders, allowing the GPU to fetch the necessary textures much quicker. Globally reducing texture quality can be achieved by going to **Edit | Project Settings | Quality | Texture Quality** and setting the value to **Half Res**, **Quarter Res**, or **Eighth Res**.

An application bound by the CPU has ample opportunities for performance enhancements through practically every performance-enhancing tip in this book. If we free up CPU cycles from other activities, then we can afford to render more objects through more Draw Calls, keeping in mind, of course, that each will cost us more activity in the GPU. There are, however, additional opportunities to make some indirect improvements in Draw Call count while we try to improve other parts of the Rendering Pipeline. This includes Occlusion Culling, tweaking our Lighting and Shadowing behavior, and modifying our Shaders. These will be explained in the following sections as we investigate various performance enhancements.

Meanwhile, we will probably need to apply a little brute-force testing and guesswork to determine how a GPU-bound application is bottlenecked. Most applications are bottlenecked by Fill Rate or Memory Bandwidth, so we should start there. It is rare to find performance bottlenecks in the Front End, at least on desktop applications, so it is worth checking only after we’ve verified that the other sources are not the problem. Vertex Shaders are often trivial compared to Fragment Shaders, and so the only real opportunity to cause problems with Front End processing is either to push too much geometry or to have overly complex Geometry Shaders.

Ultimately, this investigation should help us determine whether we are CPU-bound or GPU-bound, and in the latter case, whether we are bound by the Front End or Back End, and again in the latter case, whether we are bound by Fill Rate or Memory Bandwidth. With this knowledge, there are a number of techniques we can apply to improve performance.

## Rendering performance enhancements

We should now have all of the information we need to make sense of performance bottlenecks so that we can start to apply fixes. For the remainder of this chapter, we will cover a series of techniques to improve Rendering Pipeline performance for CPU-bound and GPU-bound applications.

## Enable/Disable GPU Skinning

The first tip involves a setting that eases the burden on the CPU or GPU Front End at the expense of the other, that is, GPU Skinning. Skinning is the process where mesh vertices are transformed based on the current location of their animated bones. The animation system, working on the CPU, transforms the object’s bones that are used to determine its current pose, but the next important step in the animation process is wrapping the mesh vertices around those bones to place the mesh in the final pose. This is achieved by iterating over each vertex and performing a weighted average against the bones connected to those vertices.

This vertex processing task can either take place on the CPU or within the Front End of the GPU, depending on whether the **GPU Skinning** option is enabled. This feature can be toggled under **Edit | Project Settings | Player Settings | Other Settings | GPU Skinning**. Enabling this option pushes skinning activity to the GPU, although bear in mind that the CPU must still transfer the data to the GPU and will generate instructions on the Command Buffer for the task, so it doesn’t remove the CPU’s workload entirely. Disabling this option eases the burden on the GPU by making the CPU resolve the mesh’s pose before transferring mesh data across and simply asking the GPU to draw it as is. Obviously, this feature is useful if we have lots of animated meshes in our Scenes and can be used to help either bounding case by pushing the work onto the device that is least busy.


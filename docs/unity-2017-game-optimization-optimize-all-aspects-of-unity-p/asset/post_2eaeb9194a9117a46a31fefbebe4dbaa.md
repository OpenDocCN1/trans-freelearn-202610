

Next, we'll create a Scene that contains a single Directional Light with four cubes and four spheres, where each object has its own unique Material, position, rotation, and scale:

![](img/da79484c9b30dc915216dbc5454a9bbf_127_0.png)

In the preceding screenshot, in the **Batching** value in the **Game** window's **Stats** popup, we can see 9 total batches. This value closely represents the number of Draw Calls used to render the Scene. The current view will consume one of these batches rendering the background of the Scene, which could be set to **Skybox** or **Solid Color**. This is determined by the Camera object's **Clear Flags** settings.

The remaining 8 batches are used to draw our 8 objects. In each case, the Draw Call involves preparing the Rendering Pipeline using the Material's properties and asking the GPU to render the given mesh at its current transform. We have ensured that each Material is unique by giving them each a unique texture file to render. Ergo, each mesh requires a different Render State, and, therefore, each of our 8 meshes requires a unique Draw Call.

As previously mentioned, we can theoretically minimize the number of Draw Calls by reducing how often we cause the system to change Render State information. So, part of the goal is to reduce the amount of Materials we use. However, if we set all objects to use the same Material, we still won't see any benefit and the number of batches remains at 9:

![](img/da79484c9b30dc915216dbc5454a9bbf_128_0.png)

This is because we're not actually reducing the number of Render State changes nor efficiently grouping mesh information. Unfortunately, the Rendering Pipeline is not smart enough to realize we're overwriting the exact same Render State values, and then asking it to render the same meshes, over and over again.

## The Frame Debugger

Before we dive into how batching can save us Draw Calls, let's explore a useful tool, which can help us determine how batching is affecting our Scene--the Frame Debugger.

We can open the Frame Debugger by selecting **Window | Frame Debugger** from the main window or clicking on the **Frame Debugger** button in the **Breakdown View Options** in the Rendering Area of the Profiler. Either approach will open the **Frame Debug** window.

Clicking on the **Enable** button in the **Frame Debug** window will allow us to observe how our Scene is being constructed, one Draw Call at a time. The following screenshot shows the user interface of the Frame Debugger, with a list of GPU instructions in the left-hand panel, and more detailed information in the right-hand panel:

![](img/da79484c9b30dc915216dbc5454a9bbf_129_0.png)

There is a lot of information in this window, which can provide useful information if we want to debug the behavior of a single Draw Call, but the most useful area to look at is the **Drawing** section in the left-hand panel, which lists all of the Draw Calls in our Scene.

Each item under this section represents a unique Draw Call and what was rendered by it. An amazingly useful feature of this tool is the ability to click on any one of these items and immediately observe only the Draw Calls needed to render the Scene up to that point in the **Game** window. This lets us perform a quick, visual _diff_ of two sequential Draw Calls. This can make it easy to spot exactly which object(s) were rendered by a given Draw Call. This can help determine whether or not a set of objects were batched together by looking at how many of them appear during that Draw Call.

> A weird bug with the Frame Debugger (which still exists in early builds of Unity 2017) is that if we are observing a Scene making use of a Skybox and click on various items under the **Drawing** section, only the final Scene presentation can be observed in the **Game** window. We would need to temporarily disable the Skybox via the Camera’s **Clear Flags** setting to take a look at how Draw Call progression appears in the **Game** window by setting it to **Solid Color** instead.

As we can see in the preceding Frame Debugger screenshot, one Draw Call is being consumed to clear the screen (the item labelled **Clear**), and then our 8 meshes are being rendered in 8 separate Draw Calls (the item labelled `RenderForward.RenderLoopJob`).

> Note that the number next to each item in the left-hand panel actually represents a Graphics API call, of which a Draw Call is but one type of API call. These can be seen in the `Camera.Render`, `Camera.ImageEffects`, and `RenderTexture.ResolveAA` items. Any API call can be just as costly as a Draw Call, but the overwhelming majority of API calls we will make in a complex Scene is in the form of Draw Calls, so it is often best to focus on minimizing Draw Calls before worrying about the API communication overhead of things such as post-processing effects.

## Dynamic Batching

Dynamic Batching has the following three important qualities:

- Batches are generated at runtime (batches are dynamically generated)
- The objects that are contained within a batch can vary from one frame to the next, depending on what meshes are currently visible to the **Main Camera** view (batch contents are dynamic)
- Even objects that can move around the Scene can be batched (it works on dynamic objects)

Hence, these attributes lead us to the name Dynamic Batching.

If we return to the **Player Settings** page and enable **Dynamic Batching**, we should see that the number of batches drops from 9 down to 6. Dynamic Batching automatically recognizes that our objects share Material and mesh information and is, therefore, combining some of them into a larger batch for processing. We should also see a different list of items in the Frame Debugger, demonstrating that meshes are now being dynamically batched.

![](img/da79484c9b30dc915216dbc5454a9bbf_131_0.png)

As we can see from the Frame Debugger, our four boxes have been combined into a single Draw Call named **Dynamic Batch**, but our four spheres are still being rendered with four separate Draw Calls. This is because the four spheres do not fit the requirements of Dynamic Batching. Despite the fact that they all use the same Material, there are many more requirements we must fulfill.

The list of the requirements needed to successfully dynamically batch a mesh can be found in the Unity documentation at http://docs.unity3d.com/Manual/DrawCallBatching.html.

> At the time of publication of this book, the previous page is a little out of date, and a better breakdown can be found in this blog post: https://blogs.unity3d.com/2017/04/03/how-to-see-why-your-draw-calls-are-not-batched-in-5-6/.

The following list covers the requirements to enable Dynamic Batching for a given mesh:

- All mesh instances must use the same Material reference
- Only `ParticleSystem` and `MeshRenderer` Components are dynamically batched. `SkinnedMeshRenderer` Components (for animated characters) and all other renderable Component types cannot be batched
- There is a limit of 300 vertices per mesh
- The total number of vertex attributes used by the Shader must be no greater than 900
- Either all mesh instances should use a uniform scale or all meshes should use a nonuniform scale, but not a mixture of the two
- Mesh instances should refer to the same Lightmap file
- The Material’s Shader should not depend on multiple passes
- Mesh instances must not receive real-time shadows
- There is an upper limit on the total number of mesh indices in the entire batch, which varies per-Graphics API and platform used, which is around 32k-64k indices (check out the documentation/aforementioned blog post for specifics)

It is important to note the term _Material references_, because if we happen to use two different Materials with identical settings, the Rendering Pipeline is not smart enough to realize that, and they will be treated as different Materials and, therefore, will be disqualified from Dynamic Batching. Most of the rest of these requirements have either already been explained; however, a couple of these requirements are not completely intuitive or clear from the description, which merits some additional explanation.


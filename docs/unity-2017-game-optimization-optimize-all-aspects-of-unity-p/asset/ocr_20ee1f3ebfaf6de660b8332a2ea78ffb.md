There are two different rendering formats, which can greatly affect our Lighting performance, known as **Forward Rendering and Deferred Rendering**. The setting for these Rendering options can be found under **Edit | Project Settings | Player | Other Settings | Rendering** and configured on a per-platform basis.

# # Forward Rendering

Forward Rendering is the traditional form of rendering Lights in our Scene, as explored previously. During Forward Rendering, each object will be rendered in multiple passes through the same Shader. How many passes are required will be based on the number, distance, and brightness of Light sources. Unity will try to prioritize the `DirectionalLight` Component that is affecting the object most and render the object in a _base pass_ as a starting point. It will then take several of the most powerful `PointLight` Components nearby and re-render the same object multiple times through the same Fragment Shader. Each of these Point Lights will be processed on a per-vertex basis and all remaining Lights will be condensed into an _average_ color by means of a technique called Spherical Harmonics.

Some of this behavior can be simplified by setting a Light's **Render Mode** to values such as **Not Important** and changing the value of **Edit | Project Settings | Quality | Pixel Light Count**. This value limits the number of Lights that will be gathered for Forward Rendering, but is overridden by any Lights with a **Render Mode** set to **Important**. It is, therefore, up to us to use this combination of settings responsibly.

As we might imagine, using Forward Rendering can utterly explode our Draw Call count very quickly in Scenes with a lot of Point Lights present due to the number of Render States being configured and Shader passes required.

> More information on Forward Rendering can be found in the Unity documentation at http://docs.unity3d.com/Manual/RenderTech-ForwardRendering.html.

# # Deferred Rendering

Deferred Rendering, or _Deferred Shading_ as it is sometimes known, is a technique that has been available on GPUs for about a decade or so, but it has not resulted in a complete replacement of the Forward Rendering method due to the caveats involved and somewhat limited support on mobile devices.
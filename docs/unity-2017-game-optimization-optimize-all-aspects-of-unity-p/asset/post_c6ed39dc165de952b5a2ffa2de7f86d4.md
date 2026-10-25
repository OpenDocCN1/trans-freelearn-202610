

It is possible to use acceleration for the motion of the player character, but it should be incredibly short and rapid before the user starts to self-adjust too quickly. It would be wisest to stick to motion that relies on constant velocities and/or teleportation.

![](img/da79484c9b30dc915216dbc5454a9bbf_266_0.png)

Placing banked turns in racing games seems to improve user comfort a great deal since the user naturally tilts their head and adjusts their balance to match the turn.

All of the above rules apply just as well to 360 Video content as they do to VR games. Frankly, there have been an embarrassing number of 360 Videos released to market, which are not taking the above into account--they feature too many jerking movements, lack of camera stabilization, manual viewport rotation, and so on. These hacks are often used to ensure the user is facing in the direction we intend. However, more effort must be spent to do so without hacking in such nausea-inducing behaviour. Humans are naturally very curious about things that move. If they notice something moving in the corner of their eye, then they will most likely turn to face it. This can be used to great effect to keep the user facing in the direction we intend as they watch the video.

![](img/da79484c9b30dc915216dbc5454a9bbf_266_1.png)

Laziness is not the way to go when generating VR content. Don’t just slap a 360 Camera on top of a dirt rally car and hack an unexpected camera rotation into the video to keep the action in the center. Motion needs to be smooth and predictable. During production, we need to constantly keep in mind where we expect the user to be looking so that we capture action shots correctly.

Fortunately, for the 360 Video format it seems as though industry standard frame rates such as 24FPS or 29.97FPS do not have a disastrous effect on user comfort, but note that this frame rate applies to video playback only. Our rendering FPS is a separate FPS value, and dictates how smooth positional head tracking will be. The rendering FPS must always be very high to avoid discomfort (ideally 90 FPS).

Other problems arise with building VR apps--different HMDs and controllers support different inputs and behavior, making feature-parity across VR platforms difficult. A problem called **Stereo-Fighting** can occur if we try to merge 2D and 3D content together, where 2D objects appear to be rendering deep inside 3D objects since the eyes can’t distinguish the distance correctly; this is typically a big problem for the User Interface of VR applications and 360 Video playback, which tends to be a series of flat panels superimposed over a 3D background. Stereo-Fighting does not usually lead to nausea, but it can cause additional eye strain.

Although the effects of discomfort are not quite as pronounced in the AR platform, it’s still important not to ignore it. Since AR apps tend to consume a lot of resources, low frame rate applications can cause some discomfort. This is especially true if an AR app makes use of superimposing objects onto a camera image (which is the majority of them), where there will probably be a disconnect in the frame rate between the background camera image and the objects we’re superimposing over it. We should try to synchronize these frame rates to limit that disconnect.

## Performance enhancements

That’s enough talk about the industry and XR development. Let’s cover some performance enhancements that can be applied to XR projects.

## The kitchen sink

Since AR and VR apps are built using the same Engine, the same subsystems, assets, tools, and utilities as any other Unity game, literally every other performance enhancement mentioned in this book can help VR and AR apps in some fashion, and we should try them all before getting too in-depth with XR-specific enhancements. This is reassuring, as there are a lot of potential performance enhancements we could apply. The downside is that we may need to apply many of them to reach the level of performance we need for our app.

The biggest threat to a VR app’s performance is GPU Fill Rate, which is already one of the more likely bottlenecks in any other game, but significantly more so for VR since we will always be trying to render a high resolution image to a much larger Frame Buffer (since we’re effectively rendering the Scene twice--once for each eye). AR apps are typically going to find extreme consumption in both the CPU and the GPU since AR platforms make heavy use of the GPU’s parallel pipeline to resolve spatial locality of objects and perform tasks such as image recognition, as well as needing a lot of Draw Calls to support those activities.

Of course, certain performance enhancing techniques are not going to be particularly effective in XR. Occlusion Culling in a VR app may be difficult to set up since the user can look under, around, and sometimes through objects in the Scene (although it can still be enormously beneficial). Meanwhile, AR apps normally render objects at reachable distances, LOD enhancements may be fairly pointless to setup. We must use our better judgement to determine whether a performance optimization technique is worth implementing _before_ we start implementing it since many of them take a lot of time to implement and support.

## Single-Pass versus Multi-Pass Stereo Rendering

For VR apps, Unity provides two rendering modes; Multi-Pass and Single-Pass. This can be configured under Edit | Project Settings | Player | XR Settings | Stereo Rendering Method. Multi-Pass rendering will render the Scene to two different images, which are displayed separately for each eye. Single-Pass Stereo Rendering combines both images into a single double-width Render Texture, where only the relevant half is displayed to each eye.

Multi-Pass Stereo Rendering is the default case. The advantage of Single-Pass rendering is that it provides significant savings in the CPU work in the main thread by reducing Draw Call setup and in the GPU since less texture swapping needs to occur. Of course, the GPU will need to work just as hard to render the objects since each object is still rendered twice from two different perspectives (there are no freebies here). The disadvantage is that this effect is currently only usable when using OpenGL ES3.0 or higher and hence is not available on all platforms. In addition, its effects on the Rendering Pipeline require extra care and effort, particularly surrounding any Shaders making use of screen-space effects (effects which only use data already drawn to the Frame Buffer). With Single-Pass Stereo Rendering enabled, Shader code can no longer make the same assumptions about the incoming screen space information. The following image shows how screen space coordinates vary between Multi-Pass and Single-Pass Stereo Rendering

![](img/da79484c9b30dc915216dbc5454a9bbf_268_0.png)

The Shader is always informed of the screen space coordinates relative to the entire output Render Texture rather than just the portion it is interested in. For example, we would normally expect an x value of 0.5 to correspond to the horizontal half-way point of the screen, which would be the case when we use Multi-Pass Stereo Rendering, however, if we use Single-Pass Stereo Rendering, then an x value of 0.5 would correspond to the half-way point between the rendering of both eyes (the right-edge of the left eye, or the left-edge of the right eye).

Unity provides some useful helper methods for screen space conversion for Shaders, which can be found at `https://docs.unity3d.com/Manual/ SinglePassStereoRendering.html`.

Another problem to worry about is Post-Processing effects. We essentially always pay double the cost for any Post-Processing effect applied to the Scene in VR since it needs to be evaluated once for each eye. Single-Pass Stereo Rendering can reduce the Draw Calls needed to set up our effect, but we can’t blindly apply a Post-Processing effect to both images simultaneously. Consequently, Post-Processing effect Shaders must also be adjusted to ensure that they render to the correct half of the output Render Texture. Without doing this, a Post-Processing effect will be stretched over both eyes, twice, which might look incredibly bizarre for effects such as lens flares.

The Single-Pass Stereo Rendering feature has been around since Unity 5.4, but is still currently labeled **Preview** in the Editor due to the current lack of support for all platforms. We can expect it to be rolled out to more platforms eventually, but for platforms supporting it we will need to perform some profiling and sensible sanity checks on our screen space Shaders to ensure that we are making positive gains from enabling this option.

## Apply anti-aliasing

This tip is less of a performance enhancement and more of a requirement. Anti-aliasing significantly improves the fidelity of XR projects since objects will blend together better and appear less pixelated, improving immersion, which can cost a lot of Fill Rate. We should enable this feature early and try to reach our performance goals with the assumption that it is simply always there, only disabling it as an absolute last resort.


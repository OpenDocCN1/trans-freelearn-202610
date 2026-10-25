![](bbox=[0.1318, 0.1113, 0.2012, 0.1758])

Did you ever notice how clouds and bushes looked exactly the same in Super Mario Bros but with different colours? This is the same concept.

There could also be ways to combine textures into Atlases to reduce the number of swaps needed. If there are a group of textures that are always used together at similar times, then they could potentially be merged together. This could save the GPU from having to pull in separate texture files over and over again during the same frame.

Finally, removing textures from the application entirely is always a last resort option we could employ.

# # VRAM limits

One last consideration related to textures is how much VRAM we have available. Most texture transfer from CPU to GPU occurs during initialization, but can also occur when a nonexistent texture is first required by the current view. This process is normally asynchronous and will result in a blank texture being used until the full texture is ready for rendering (refer to Chapter 4, Kickstart Your Art, to note that this assumes read/write access is disabled for the texture). As such, we should avoid introducing new textures at runtime too frequently.

# # Preload textures with hidden GameObjects

The blank texture that is used during asynchronous texture loading can be jarring when it comes to game quality. We would like a way to control and force the texture to be loaded from disk to RAM and then to VRAM before it is actually needed.

A common workaround is to create a hidden GameObject that uses the texture and place it somewhere in the Scene on the route that the player will take toward the area where it is actually needed. As soon as the player looks at that object, the texture is needed by the Rendering Pipeline (even if it's technically hidden), it will begin the process of copying the data from RAM to VRAM. This is a little clunky, but easy to implement and works sufficiently well in most cases.
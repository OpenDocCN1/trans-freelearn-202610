# # Minimize Material count

This concern goes hand in hand with the concepts of Batching and Atlasing. The fewer Materials we use, the fewer Draw Calls required. This strategy will also help with concerns relating to VRAM and Memory Bandwidth, which tend to be very limited on mobile devices.

# # Minimize texture size

Most mobile devices feature a very small Texture Cache relative to desktop GPUs. There are very few devices on the market still supporting OpenGL ES 1.1 or lower, such as the iPhone 3G, but these devices could only support a maximum texture size of 1024 x 1024. Devices supporting OpenGLES 2.0, such as everything from the iPhone 3GS to the iPhone 6S, can support textures up to 2048 x 2048. Finally, devices supporting OpenGLES 3.0 or greater, such as devices running iOS 7, can support textures up to 4096 x 4096.

> > There are way too many Android devices to list here, but the Android developer portal gives a handy breakdown of OpenGLES device support. This information is updated regularly to help developers determine supported APIs in the Android market at `https://developer.android.com/about/dashboards/index.html`

Double-check the device hardware we are targeting to be sure that it supports the texture file sizes we wish to use. However, later-generation devices are never the most common devices in the mobile marketplace. If we wish our game to reach a wide audience (increasing its chances of success), then we must be willing to support weaker hardware.

Note that textures that are too large for the GPU will be downscaled by the CPU during initialization. This wastes valuable loading time and is going to leave us with unintended loss of quality due to an uncontrolled reduction in resolution. This makes texture reuse of paramount importance for mobile devices due to the limited VRAM and Texture Cache sizes available.
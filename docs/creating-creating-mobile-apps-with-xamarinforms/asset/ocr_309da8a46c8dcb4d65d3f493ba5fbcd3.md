In the Microsoft Windows world, a similar technique was developed called *device-independent pixels* (DIPs) or *device-independent units* (DIUs). To the Microsoft programmer, desktop video displays are assumed to have a resolution of 96 DIUs, which is exactly 1/3 higher than 72 DPI.

Phones, however, have some different rules: The pixel densities achieved on modern phones are typically much higher than desktop displays. This higher pixel density allows text and other visual objects to shrink much more in size before becoming illegible.

Phones are also typically held much closer to the user’s face than a desktop or laptop screen. This difference also implies that visual objects on the phone can be much smaller than comparable objects on desktop or laptop screens. Because the physical dimensions of the phone are much smaller than desktop displays, shrinking down visual objects is very desirable because it allows much more to fit on the screen.

Apple continues to refer to the device-independent units on the iPhone as *points*, but for all Apple’s high-density displays—which Apple refers to by the brand name *Retina*—there are two pixels to the point. This is true for the iPhone, iPad, and MacBook Pro.

For example, the 640 × 960 pixel dimension of the 3.5" screen of the iPhone 4 has an actual pixel density of about 320 DPI. To the programmer, the screen appears to have a dimension of 320 × 480 points. The iPhone 3 actually did have a pixel dimension of 320 × 480, and points equaled pixels, so to the programmer the displays of the iPhone3 and iPhone4 appear to be the same size. But this size implies a conversion factor of 160 points to the inch rather than 72.

The iPhone 5 has a 4" screen, but the pixel dimension is 640 × 1136, and the pixel density is about the same as the iPhone 4. To the programmer, this screen has a size of 320 × 768 points.

Android does something quite similar: Android devices have a wide variety of sizes and pixel dimensions, but the Android programmer always works in units of *density-independent pixels (dps)*. The relationship between pixels and dps is set assuming 160 dps to the inch, which means that Apple and Android device-independent units are very similar.

Microsoft took a different approach with the Windows Phone, however. Windows Phone 7 devices have a uniform pixel dimension of 480 × 800, which is often referred to as WVGA (Wide Video Graphics Array). Windows Phone 7 programmers work with this display in units of pixels, which means that the Windows Phone is implicitly assuming a pixel density of about 240 DPI, which is 1.5 times the assumed pixel density of iPhone and Android devices.

With Windows Phone 8, several larger screen sizes are allowed: 768 × 1280 (WXGA or Wide Extended Graphics Array), 720 × 1280 (referred to using high-definition television lingo as 720p), and 1080 × 1920 (called 1080p).

For these additional display sizes, programmers work in device-independent units. An internal scaling factor translates between pixels and device-independent units so that the width of the screen in portrait mode always appears to be 480 pixels wide. The scaling factors are 1.6 (for WXGA), 1.5 (720p), and 2.25 (1080p).
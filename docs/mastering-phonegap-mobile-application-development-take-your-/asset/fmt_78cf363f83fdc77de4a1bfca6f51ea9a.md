
Technically, your launch screen could be a simple black background and this would work. If your app's background color is black, you could indeed get away with this. If your app has a white background, a black launch screen is a little distracting, so you should endeavor to match your launch screen with your app's interface.

Apple has the idea that your launch screen should appear as if your app has been loaded into memory, but has yet to be populated with data. This gives the impression that your app loads quickly, but this is really a trick; your app is still unresponsive at this point. Personally, I find this a little frustrating from the end user perspective, because it seems to make the app's load time feel slower than it would otherwise. I have the tendency to start interacting with the apps that do this earlier than the app expects, so I have to repeat my actions when the app finally initializes.

### Tip

Most devices have a **watchdog** that ensures your app starts up within a certain amount of time. For example, on iOS, if your app doesn't complete the startup within ten seconds, your app is automatically terminated. The operating system decides that your app has crashed if it remains unresponsive for this time period, and to prevent the user from having to manually terminate the app, the operating system does it for them.

Most app developers will instead create something a little more abstract that still represents their app. The colors that the app uses will be present on the launch screen, but the rest of the launch screen may not accurately reflect the app's actual appearance. Although Apple frowns on this kind of launch screen, most apps fall into this category, so clearly Apple isn't terribly concerned that you follow their guidelines on this issue.

Other apps will use the launch screen as an opportunity to get really artsy. This is most common with games, but the launch screen really isn't about *delaying* the user's interaction with the app, it's solely about covering up the loading time. So, if your app loads quickly, your user may never get to notice the intricate details of your art. In short: keep it simple, not complex. Furthermore, games usually render *several* splash screens in quick succession. For example, the publisher might have a screen all to themselves, while the development company might get another. Some actual game art might be displayed just prior to launching the menu. This particular use of splash screens should be handled by your app, *not* the operating system. As such, you would be better off using a very simple launch screen (perhaps, simply with the *Your Publisher presents* message, as seen in many Nintendo games).

This translates into several different styles of launch screens. Let's look at a few:

![Launch screen requirements](graphics/9781OS_11_04.jpg)

As you can see, the first launch screen (upper left) is very bland. It's a solid color, and that's it. In this case, it would match the background color of our demonstration app if it didn't have any data. This isn't so much to present the unpopulated user interface we've been talking about; it's just trying to prevent the jarring transition from one color to another. If I had used a black color, the transition from the launch screen to the app would be extremely obvious.

The second launch screen (upper right) takes things a step farther. In this case, I added the name of the app and a tag line. These never appear in the app in this particular form, but it tells the user that *yes, the app is loading and, by the way, you did start the right app*.

The third launch screen (lower left) starts to work off the unpopulated user interface metaphor. There's an orange bar at the top exactly where the navigation bar would be in the app. The transition to the app would be virtually seamless, except for the disappearance of the app title and tag line.

The final launch screen (lower right) starts to be a bit more abstract. The colors are still the same, but I've given up on the navigation bar concept. Instead there are two different kinds of color here dividing the screen (in a way, like a book), and the app title is imposed over the separation between these two colors creating an inverse effect. This is definitely *not* what our app looks like with no data, but the launch screen still works.

Like your app's icon, your launch screen needs to be rendered in various resolutions. For each platform, this means creating a launch screen that matches the various screen sizes and orientations that your app targets. On iOS, this adds up to quite a few versions. You would follow the same example on Android, but you can also use nine-patch images instead.

### Tip

Nine-patch images are *resizable* raster images. You can recognize them by their extension: `.9.png`. They are PNG files, so you can technically create them in any editor. Around the border is a series of black and white pixels. These pixels tell Android what portions of the image can be scaled and what portions should always remain the same size. This allows you to create a launch screen that completely fills the screen of any device while not stretching the important content (such as a logo or other user interface elements). We'll cover creating these images later on in this chapter.

It should be pretty obvious at this point that creating lots of versions of the launch screen for each different resolution can be quite time-consuming. If you keep your important content near the center of the canvas, you may be able to use some utilities to create these images for you (and I listed a few at the end of the chapter). But if you have content that's near the edges (like a navigation bar), you will have to do the hard work yourself. In the preceding example, this makes the third launch screen the hardest to export.

Let's quickly go over the required resolutions for iOS:

| Device || Portrait || Landscape |
| --- | --- | --- |
| iPhone (@1x) || 320 × 480 || N/A |
| iPhone 4, 4s (@2x) || 640 × 960 || N/A |
| iPhone 5, 5s (@2x) || 640 × 1136 || N/A |
| iPhone 6, 6s (@2x) || 750 × 1334 || 1334 × 750 |
| iPhone 6+, 6s+ (@3x) || 1242 × 2208 || 2208 × 1242 |
| iPad (@1x) || 768 × 1024 || 1024 × 768 |
| iPad (@2x) || 1536 × 2048 || 2048 × 1536 |
| iPad Pro (@2x) || 2048 × 2732 || 2732 × 2048 |

Android also has several different suggested resolutions for the launch screen, as follows:

| DPI || Portrait || Landscape |
| --- | --- | --- |
| ldpi || 200 × 320 || 320 × 200 |
| mdpi || 320 × 480 || 480 × 320 |
| hdpi || 480 × 800 || 800 × 480 |
| xhdpi || 720 × 1280 || 1280 × 720 |

These sizes aren't a hard and fast rule. Android devices have many different screen sizes and pixel densities. This is why we'll use nine-patch images: they can scale to fit any device size without distorting the content.

As we've mentioned, both iOS and Android have differing opinions as to what a launch screen should be, and whether or not it is even required. What's really important to know is that neither Apple, nor Google, particularly *requires* you to adhere to their guidelines when it comes to launch screens. For Apple especially, it's just important that the launch screen exists. They *won't* check whether it matches what your user interface would look like with no data.

For our app, we'll take the slightly abstract launch screen approach (the fourth launch screen in the prior section). This lets us simplify the export of the launch screens in all the various resolutions by using automation utilities rather than doing it all by hand.

We can't cover *all* the steps involved in creating the initial launch screen, since they would vary based upon your use cases, your app's user interface, and your graphic editor, but we can cover what you'll have to do at a high level. You should be able to look at my source and output images in order to see how it worked out for me.

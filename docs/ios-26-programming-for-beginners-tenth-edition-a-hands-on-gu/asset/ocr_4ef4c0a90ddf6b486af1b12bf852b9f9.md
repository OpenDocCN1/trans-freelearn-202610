# Technical requirements

You will continue working on the JRNL project that you modified in the previous chapter.

The resource files and completed Xcode project for this chapter can be found in the Chapter20 folder of the code bundle for this book, which can be downloaded here:

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

Check out the following video to see the code in action:

https://youtu.be/UY9AB8TLjOU

Let’s start by modifying the Add New Journal Entry screen to display an image picker controller, which allows you to use the device camera or select a photo from the user’s photo library.

# Create a new UIImagePickerController instance

To make it easy for a user to use the camera or photo library, Apple implemented the UIImagePickerController class. This class manages the system interfaces for taking photos and choosing items from the user’s photo library. An instance of this class is called an image picker controller, and it can display an image picker on the screen.

If you have ever added a photo to a social media post, you will have seen what the image picker looks like. It typically displays either the view from your camera or a grid of photos from your photo library, and you can then choose a photo to be added to your post:

![](bbox=[0.35, 0.59, 0.71, 0.88])

Figure 20.1: Simulator showing the image picker
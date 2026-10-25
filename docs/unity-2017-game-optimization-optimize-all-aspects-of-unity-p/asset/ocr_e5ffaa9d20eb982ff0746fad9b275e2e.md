# Manage resolution downscaling externally

Unity puts a lot of effort into making things as easy to use as possible and provides us with
the ability to place the project files from external tools to our project workspace, such as
.PSD and .TIFF files, which are often large and split into multiple layered images. Unity
automatically generates a texture file from the file's contents for the rest of the Engine to
make use of, which can be very convenient, as we only need to maintain a single copy of the
file through Source Control, and the Unity copy is automatically updated when an artist
makes changes.

The problem is that the aliasing introduced by Unity's auto-texture generation and
compression techniques from these files may not be as good as what the texture-editing
tools we use could generate for us. Unity is very feature-rich and, first and foremost,
focuses on being a game development platform, which means that it can have difficulty
competing in areas that other software developers work on full time. Unity may be
introducing artifacts through aliasing as a result of downscaling the image for us, and so we
might find ourselves working around it by importing image files with a higher resolution
than necessary just to keep the intended quality level. However, had we downscaled the
image through the external application first, we might have suffered much less aliasing. In
these cases, we may achieve an acceptable level of quality with a lower resolution, while
consuming less overall disk and memory space.

We can either avoid using .PSD and .TIFF files within our Unity project as a matter of
habit (storing them elsewhere and importing the downscaled version into Unity) or just
perform some occasional testing to ensure that we're not wasting file size, memory, and
GPU Memory Bandwidth using larger resolution files than necessary. This costs us some
convenience in project file management, but can provide some significant savings for some
textures, if we're willing to spend the time comparing the different downscaled versions.

# Adjust Anisotropic Filtering levels

Anisotropic Filtering is a feature that improves the image quality of textures when they are
viewed at very oblique (shallow) angles. The following screenshot shows the classic
example of painted lines on a road with and without Anisotropic Filtering applied:
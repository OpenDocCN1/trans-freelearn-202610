# # Make textures square and power-of-two

We have already covered this topic in Chapter 4, Kickstart Your Art, but it is worth revisiting the subject of GPU-level Texture Compression. The GPU will find it difficult, or simply be unable to compress the texture if it is not in a square format, so make sure that you stick to the common development convention and keep things square and sized to a power-of-two.

# # Use the lowest possible precision formats in Shaders

Mobile GPUs are particularly sensitive to precision formats in its Shaders, so the smallest formats should be used such as half. On a related note, precision format conversion should be avoided at all costs for the same reason.

# # Summary

If you've made it this far without skipping ahead, then congratulations are in order. That was a lot of information to absorb for just one subsystem of the Unity Engine, but then it is clearly the most complicated of them all, requiring a matching depth of explanation. Hopefully, you've learned a lot of approaches to help you improve your rendering performance and enough about the Rendering Pipeline to know how to use them responsibly.

By now, we should be used to the idea that, with the exception of algorithm improvements, every performance enhancement we implement will come with some related cost that we must be willing to bear for the sake of removing one bottleneck. We should always be ready to implement multiple techniques until we've squashed them all, and potentially spend a lot of additional development time to implement and test some performance enhancing features.

In the next chapter, let's bring performance optimization into the modern era by exploring some performance improvements we can apply to Virtual Reality and Augmented Reality projects.
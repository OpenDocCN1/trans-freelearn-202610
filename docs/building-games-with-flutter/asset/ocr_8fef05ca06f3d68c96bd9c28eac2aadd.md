# Chapter 9

+   1. `lifespan` and `count` are common properties to set when using particles. They represent how long the particle should be shown for and how many should be shown, respectively.

+   2. Particles created a lot of objects very quickly and can use up a lot of memory, so we need to free up the memory once the particle is no longer in use.

+   3. `MovingParticle`, `CircleParticle`, and `ComputedParticle` are examples of particles that Flame supports.

+   4. Flame supports the `PreRenderedLayer` for static images and the `DynamicLayer` for animated images.

+   5. We need to delegate the rendering of the super class to the layer class so that the layer processor can do its work of generating the shadow image.
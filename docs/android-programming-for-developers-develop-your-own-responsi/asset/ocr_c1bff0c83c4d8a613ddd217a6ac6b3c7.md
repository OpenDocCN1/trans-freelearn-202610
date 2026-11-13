It is worth noticing that we are keeping a reference to the list of modifiers. This list is shared among all the particles in the system and it is managed by the ParticleSystem class.

During onUpdate, we add the elapsed milliseconds to the total time and check whether the Particle has passed its time to live. If this is the case, we remove it from the GameEngine. Otherwise, we update the position and rotation based on the speed (linear and angular) and then apply all the modifiers.

You may have realized that we are using linear and angular speed values but, so far, we have not set them. This is done by the ParticleSystem through the initializers. This is why these variables are public.

Let's look at the system that handles the particles.
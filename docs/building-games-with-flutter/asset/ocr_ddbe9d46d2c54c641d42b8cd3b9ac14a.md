+   - How long a particle lives
+   - How often a new particle is created
+   - The position where the particle is created
+   - The angle, distance, and speed of travel
+   - What colors the particles should be
+   - How physics affects the particles

A good example of a particle effect is fireworks. Fireworks explode in a variety of colors and travel at different speeds and angles as they vanish into nothing in the sky after a short time.

Flame supports many types of particle effects, which you can see examples of at https://examples.flame-engine.org/#/Rendering_Particles. These are discussed in more detail in the Flame documentation at https://docs.flame-engine.org/1.0.0/particles.html, but let's summarize some of the different types of particles here for your reference:

+   - `MovingParticle` – Moves the child particle between two points during its lifetime
+   - `AcceleratedParticle` – Applies basic physics-based effects to the particle, such as gravity or speed dampening
+   - `CircleParticle` – Draws circles in different sizes
+   - `SpriteParticle` – Uses sprite images in your particle
+   - `ComputedParticle` – For more advanced control of the particle, which may need computed values to affect the particle

In our game, we want to make the coins explode outward while fading out the alpha value. We will make these particles yellow to match the coin color. For our enemies, we will reuse the exploding effect but instead make the particles red to look like blood.
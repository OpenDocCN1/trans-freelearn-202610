can have a particle from the pool (it is not empty).

Once we know that we need to spawn a particle and we have some particles
available, we proceed to the activation. The last line on the activateParticle
method increases the counter of activated particles.

Note that the number of activated particles is the total number of particles that
have been activated by the emitter and not the current number of active particles.

The emit method from ParticleSystem is as follows:

```
public void emit (int particlesPerSecond) {
	mActivatedParticles = 0;
	mTotalMillis = 0;
	mParticlesPerMillisecond = particlesPerSecond/1000d;
	mIsEmitting = true;
}
```

Easy enough; it resets the number of activated particles and the time counter. It
also calculates the rate of particles per millisecond (the parameter is passed as
particles per second) and then sets mIsEmitting to true, so the onUpdate
method should work.

Finally, to stop emitting we just need to set mIsEmitting to false:

```
public void stopEmitting() {
	mIsEmitting = false;
}
```

Let's look at some examples of emitters.
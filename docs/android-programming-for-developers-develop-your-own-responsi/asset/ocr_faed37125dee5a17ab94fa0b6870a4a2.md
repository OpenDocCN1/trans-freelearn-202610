# # Initializers

Initializers are used by the ParticleSystem to set values for the particles based on parameters. To do this, we define ParticleInitializer as a very simple interface:

```
public interface ParticleInitializer {
 void initParticle(Particle p, Random r);
}
```

As an example, we will look at the utility method of ParticleSytem to set the initial rotation of the particles:

```
public ParticleSystem setInitialRotationRange (int minAngle, int maxAngle) {
 mInitializers.add(new RotationInitiazer(minAngle, maxAngle));
 return this;
}
```

When we set the initial rotation range for a Particle, we add an initializer of the RotationInitializer type to the ParticleSystem. Then we return this, so the methods can be chained.

Other methods that we can use to configure the initialization of particles are:

+   * setRotationSpeedRange(double minRotationSpeed, double maxRotationSpeed)
+   * setSpeedRange(double speedMin, double speedMax)
+   * setSpeedModuleAndAngleRange(double speedMin, double speedMax, int minAngle, int maxAngle)
+   * setRotationSpeedRange(double minRotationSpeed, double maxRotationSpeed)

We are not going to include the code of all the initializers we are going to use since all of them follow the same pattern:

+   * Receive a set of parameters and define the range(s)
+   * Generate a random value(s) from the range(s)
+   * Set the value(s) to the proper variable(s) of Particle (this may involve more than one field)
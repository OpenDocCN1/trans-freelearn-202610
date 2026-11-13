# Note

Initializers get a random value(s) from a range and set it to a variable(s) of Particle.

As an example on how ParticleInitializer is implemented, let's look at the code of RotationInitializer:

```
public class RotationInitiazer implements ParticleInitializer {

private int mMinAngle;
private int mMaxAngle;

public RotationInitiazer(int minAngle, int maxAngle) {
mMinAngle = minAngle;
mMaxAngle = maxAngle;
}

@Override
public void initParticle(Particle p, Random r) {
int value = r.nextInt(mMaxAngle-mMinAngle)+mMinAngle;
p.mRotation = value;
}
}
```

Straightforward enough; the initializer stores the minimum and maximum values of the angle in member variables (mMinAngle, mMaxAngle). When it initializes a particle, it generates a random value in the range and sets it to the rotation variable of the particle.

All initializers work the same way; some are a bit more complex than others. For example, SpeedModuleAndRangeInitializer uses trigonometry to convert the speed from angle and module to coordinates:

```
public class SpeedModuleAndRangeInitializer implements
ParticleInitializer {

private double mSpeedMin;
private double mSpeedMax;
private int mMinAngle;
private int mMaxAngle;

public SpeedModuleAndRangeInitializer(
double speedMin, double speedMax,

```
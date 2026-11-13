For modifiers, it is important to emphasize that the apply method receives the total amount of milliseconds spent. This is required because the modifier does not know anything about the Particle. It does not have any state, so all the information must be passed as parameters to the apply method.

# Note

Modifiers are the same for all the particles and do not save any state.

The modifier has a start and an end time. When the time is outside the interval, we set it to the initial or final value. When the time is between these boundaries, it returns the value as a linear interpolation of the increment between the initial and final values.

It is quite easy to modify this code to use other types of interpolators. If you are curious about it, you can check the code of Leonids on GitHub, which supports interpolators.

ScaleModifier is almost identical to this one, except that the value is set to mScale instead of mAlpha.
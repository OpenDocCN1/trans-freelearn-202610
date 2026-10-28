# Practical sessions

Here are some additions you could make to the code:

+   - Add support for multiple meshes containing morph target animations.
+   - Currently, only one mesh of a model can have morph target animations. For a simple head animation, this limit is fine. But if you want to control more than one part of the face, using multiple morph targets may be helpful.
+   - Add blending between two morph targets. For even better and more natural facial expressions, a direct blending between two morph targets would be nice. A detour via the neutral position is no longer needed, then, but a direct path between anger and worry is available.
+   - Add more morph targets.
+   - You may try to let the models speak. You could add expressions for different vowels and consonants, including the direct blending from the previous task. With such animations, you could mimic instances speaking to you during interactions.
+   - Add layered/masked animations.
+   - In contrast to additive animation blending, layered blending uses different skeletal animation clips for distinct parts of the model’s virtual body. For instance, all but the right arm uses the running animation and only the right arm plays the waving animation. As noted in the How additive blending works section, layered animations may need additional effort to synchronize the two animation clips. You need to add some logic to mask out parts of the model’s skeleton.
+   - Let the instance turn their head toward you on interaction. This is a feature of many games: if you start interacting with an instance, they turn their head around to look directly toward you.
+   - Let nearby instances judge you. This is like the previous task: you could also add the additive head-turning animation to instances walking near you. Add a random facial expression too, enabling the instances to show some sort of emotion when they pass near you.
+   - Let the instances smile and wave at the nearest instance passing by.
+   - This is a combination and extension of the two previous tasks: use the interaction logic to find the nearest instance for every instance in the virtual world, then move the head towards that instance. Play the smiling morph animation and a new additive animation with only the right arm waving. You might want to use a layered animation for the arm here.
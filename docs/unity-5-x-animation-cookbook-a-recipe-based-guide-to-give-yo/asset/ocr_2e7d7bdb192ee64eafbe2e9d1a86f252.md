# Using Animation Events to trigger sound and visual effects

This recipe shows a simple, generic way of playing different sound and visual effects with Animation Events.

# Getting ready

To start, you need to have a character with one looped animation: Jump. We also need a sound effect and a particle system. We will need a transparent DustParticle.png texture for the particle system. It should resemble a small dust cloud. You can open the provided example Unity project and go to the Chapter 07 Special effects\Recipe 01 Using animation events to trigger sound and visual effects directory. In the Rigs directory, you will find all the needed animations, and in the Resources folder, you will find all other necessary assets. When you play the game, you will see a character playing the Jump animation. It will also play a sound effect and a particle effect when landing.

# How to do it...

To play sound and visual effects with Animation Events, follow these steps:

+   1. Import the character with the Jump animation.
+   2. In the Import Settings, Animation tab, select the Jump animation.
+   3. Make it loop.
+   4. Go to the Events section.
+   5. Scrub through the timeline in the Preview section and click on the Add Event button. The Edit Animation Event window will appear, as shown in the following screenshot:
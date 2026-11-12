![](bbox=[0.126, 0.089, 0.865, 0.365])

Figure 10.20 – Modifying the InputAction Fire event to make noise

+   9. Drag a wire from the white output pin of the SET Player Current Ammo node, search for `make noise`, and add the Make Noise (PawnNoiseEmitter) function.
+   10. Change the Loudness input of the Make Noise node to `1.0`.
+   11. Drag a wire from the Noise Location input of the Make Noise node and add a GetActorLocation node.
+   12. Compile, save, and then click on Play to test the game.

While behind an enemy, or otherwise outside their line of sight, sprinting or firing your gun should result in the enemy approaching the position you were at when you made the noise. If they establish a line of sight with you during their investigation, then they will begin heading directly toward you.

With detection possible with both sight and sound, you might now find it difficult to avoid being spotted by enemies. We will now turn our attention to the other side of gameplay balancing and equip the player with the means to combat their enemies.

# Making the enemies destructible
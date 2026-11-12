```
acc = 0;
} else if (pos.y > height - 20 - ship.height/2 ) {
    pos.y = height - 20 - ship.height/2;
    state = FINISHED;
    acc = 0;
}
```

16. Run your sketch now and try to land the rocket on the platform by using the arrow keys. Your game window should look as shown in the following screenshot when the rocket is accelerated:

![](bbox=[0.355, 0.32, 0.645, 0.545])

# Objective Complete - Mini Debriefing

In this task of our current mission, we have completed the game for desktop and laptops running Mac OS X, Linux, or Windows. From step 1 to step 5, we implemented moon gravity to our sketch and made the rocket drop to the ground. We used a very simple physics simulation that adds a fraction of the moon's gravitational force to a speed variable at every frame and then adds the updated speed to the current position.

From step 6 to step 10, we implemented another force that influences our ship by implementing some keyboard control, allowing the player to turn the ship and to fire the thruster. We also made some flames come out of the thruster in step 11 to give some visual clues to the player on how strong the rocket is being accelerated.

Starting with step 12, we implemented a game-state system to prevent the rocket from dropping to the floor as soon as the game has been started. It also enables the player to restart the game after the rocket has landed on the platform or crashed to the moon by clicking on the program window.
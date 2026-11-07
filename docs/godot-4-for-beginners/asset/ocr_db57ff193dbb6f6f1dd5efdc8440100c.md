The Strawberry can now collide with objects and detect when an object has collided with it. We will need to write code to animate the Strawberry and to do something when the player collides with it, such as removing the Strawberry and adding 1 to the player’s Strawberry collection score.

# Implementing the Strawberry script

The Strawberry only needs to do two things: animate and disappear when the player collects it. We will now program the Strawberry to do these two things.

In the Strawberry scene, select your Strawberry node and click on the Attach New Script button/icon, as shown in Figure 7.7:

![](bbox=[0.28, 0.32, 0.73, 0.39])

Figure 7.7 – The Attach New Script button/icon

The idle animation must play repeatedly; we have already set it to loop, so, in the ready function, we just need to start the animation. To make it easier to reference other nodes in a scene, Godot provides a built-in shortcut. By preceding a node’s name with a dollar sign ($), we can quickly access that node and its functions directly.

This shortcut is particularly useful when frequently interacting with other nodes in a scene, such as controlling a character’s animations or updating UI elements, as it keeps the code more readable and efficient. Use this shortcut to play the idle animation of the Strawberry, as shown in the following code:

```
func _ready():
    $AnimatedSprite2D.play("idle")
```

When our player collides with or overlaps a Strawberry, we want to signal the Strawberry to execute a function. In the function, we can remove the Strawberry from the level and add 1 to the number of Strawberries collected by the player. Area2D nodes can respond to a variety of built-in signals, one of which is when a body enters the area of the node.

To set this up, select the Strawberry node, then click on the Node tab (to the right of the Inspector tab). In the Signals category, under Area2D, select the body_entered signal and click on Connect in the bottom-right corner, or simply double-click the signal name. This is shown in Figure 7.8:
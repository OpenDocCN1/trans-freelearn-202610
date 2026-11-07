Player will keep track of how many Strawberries it has collected. We need a variable in the Player script to keep count of the number of Strawberries the player has collected, and we need a function in the Player script to add one to the score each time a Strawberry is collected.

Return to the Player script and add the variable and function as shown in the code:

```
var jumpCount = 0
var is_wall_sliding = false
var strawberry_count = 0

func add_score(amount):
    strawberry_count += amount
```

The preceding code will increase the strawberry_count variable by an amount each time it is called. We will call it in the _on_body_entered(body) function of the Strawberry and remove the Strawberry from the scene once collected, as shown here:

```
func _on_body_entered(body):
    if body.is_in_group("Player"):
        body.add_score(1)
        queue_free() #remove strawberry
```

The built-in queue_free() function deletes a node from memory at the end of the current frame as soon as it is safe to do so. It is the recommended method for removing nodes from the scene.

With this code in place, when the Strawberry signals that another body has entered its space, a value of 1 will be added to the Strawberry count of the player, and the Strawberry will be removed from the scene.
Using the Empty template will ensure that our script is created with no default or added code and is as simple as possible.

Add code to set values for the speed, direction, and health of the Mushroom, as shown in the code:

```
extends CharacterBody2D

const SPEED = 75
var direction = 1
var health = 1
```

Note that the direction variable will have values of -1 or 1 to indicate left or right movement, respectively.

Now, we have variables to control the speed, direction, and health of the Mushroom enemy.

Also, add a function to apply gravity to the Mushroom, allowing it to fall and touch the ground, as shown in the following code:

```
func add_gravity(delta):
    # Add gravity.
    if not is_on_floor():
        velocity += get_gravity() * delta
```

If the Mushroom is not on the ground, gravity will pull it downward.

Now, create a function to make the Mushroom move. I have called mine update_direction() and all it does is make the enemy move to the right at a constant speed, as shown in the code:

```
func update_direction():
    # Move enemy at constant speed
    velocity.x = SPEED * direction
```

The velocity of the Mushroom in the horizontal plane is multiplied by speed and direction.

Both functions must also be called in the _physics_process(delta) function along with move_and_slide(), as shown in the code:

```
func _physics_process(delta):
    add_gravity(delta)
    update_direction()
    move_and_slide()
```
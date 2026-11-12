In this section, we learned how to add a joystick, a button, and a score to our game and positioned them on the screen.

In the next section, we will connect the joystick to George's movement and make him run faster when the run button is pressed. We will also update the score by 10 when George collides with an enemy.

# Moving our character with onscreen controls

In this section, we will start by connecting George's movement to the joystick.

Currently, the `George` class inherits his movement from the base class, `Character`, which it shares with the `Skeleton` and `Zombie` classes. As George will have different movement code from the enemy sprites, let's refactor the code to allow the enemy sprites' movement code. We will move the existing movement code into an `EnemyCharacter` class, which will become the new base class for the enemy sprites and remove this code from the `Character` class.

Let's get started:

+   1. In the `components` folder, create a file called `character_enemy.dart`.

+   2. Open the file and add the code at https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter05/lib/components/character_enemy.dart.

In this code, the `EnemyCharacter` class extends our `Character` class, and we have copied the `onCollision`, `update`, and `changeDirection` functions straight from the `Character` class.

+   3. In the `changeDirection` function in the `EnemyCharacter` class, you will need to update the `switch case` block to prepend the directions
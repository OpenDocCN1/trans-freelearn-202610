![](bbox=[0.292, 0.148, 0.706, 0.355])

Now that we understand how we will obtain our referenced sensed actors, we should create a way for our pawn to maintain different states of being similar to what we would do in Behavior Tree.

+   2. Let's first establish a home location for our pawn to run to when the player is no longer detected by the AI.
+   
+   In the same **Blueprint** folder, we will create a subclass of **Target Point**. Let's name this `Waypoint` and place it at an appropriate location within the world.
+   
+   3. Now, we need to open this `Waypoint` subclass and create additional variables to maintain traversable routes. We can do this by defining the next waypoint within a waypoint, allowing us to create what programmers call a linked list. This results in the AI being able to continuously move to the next available route after reaching the destination of its current route.
+   
+   4. With `Waypoint` open, add a new variable named `NextWaypoint` and make the type of this be the same as that of the `Waypoint` class we created.
+   
+   5. Navigate back to our Content Browser.
+   
+   6. Now, within our `EnemyPatrol` AIController, let's focus on **Event Begin** in EventGraph. We have to grab the reference to the waypoint we created earlier and store it within our AIController.
+   
+   7. So, let's create a new waypoint variable type and name it `CurrentPoint`.
+   
+   8. Now, on **Event Begin Play**, the first thing we need is the AIController, which is the self-reference for this EventGraph because we are in the AIController class.
+   
+   9. So, let's grab our self-reference and check whether it is valid. Safety first! Next, we will get our AIController from our self-reference. Then, again for safety, let's check whether our AIController is valid.
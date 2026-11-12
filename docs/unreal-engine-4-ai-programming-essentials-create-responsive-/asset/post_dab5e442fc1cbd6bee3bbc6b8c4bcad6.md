
- 5. Set the **Duration** value to .05 so that it will update relatively fast.
- 6. Now, we have to get the location from the pawn to create line traces. We will also use the right vector to face the pawn to the right from the pawn's current rotation when the collision ahead is detected.
- 7. Right-click on **EventGraph** and search for **Get Controlled Pawn**.
- 8. From the **Return Value** pin of **Get Controlled Pawn**, pull the **Get Actor Location** node.
- 9. From the **Return Value** pin of **Get Actor Location**, pull a vector and then drop it in an empty area.
- 10. Search for **LineTraceByChannel**, which is located under the **Collision** category, as shown in the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_45_0.png)

## Traces

We will use this to trace from the player's location to 255 units in front of the character. If anything collides with the trace, we will face the pawn to the right based on the pawn's rotation. This simple check will be enough to make our bot run along the walls indefinitely; so, perform the following steps:

- 1. Pull the Return Value pin from Get Actor Location and drop it. Then, search for Vector + Vector.
- 2. We now need to select Get Controlled Pawn and pull Get Actor Forward Vector from it. This contains vector information going in the direction that is in front of our pawn.
- 3. So we will multiply Return Value by 255. This is the vector we want to add to the actor's location. This results in adding 255 units in the direction ahead of the pawn's current location.
- 4. Now, we need to take the results of the addition of the End pin for the LineTraceByChannel node. This will trace directly in front of our pawn:

![](img/aaaa027482c900e215ba6bfab3c17337_46_0.png)

- 5. Pull from the Vector + Vector node again, and this time we will plug it into the Dest pin for the Move to Location node. This will move the character forward indefinitely:

![](img/aaaa027482c900e215ba6bfab3c17337_47_0.png)

Let's try this out! Hit **Simulate** and look at your character move forward indefinitely! The only problem is that it can't avoid the walls just yet. We will change this by introducing a **Select Vector** node that chooses a different direction when a collision is detected by **Line Trace**:

![](img/aaaa027482c900e215ba6bfab3c17337_47_1.png)

- 6. Once more, we will pull from the **Get Controlled Pawn** node and search for **Get Actor Right Vector**.
- 7. From this node, we will pull the vector and multiply it by 512.
- 8. We will add this to the pawn's current location. So, similar to what we did before going forward, we will add 512 units to the right of the pawn's current location:

![](img/aaaa027482c900e215ba6bfab3c17337_48_0.png)

- 9. Right-click on the **EventGraph** section and search for **Select Vector**.
- 10. From the **LineTraceByChannel** node, pull **Return Value** and connect it to the **Select Vector** node. Then, select **A**.
- 11. Now, pull from the right location to **A** of the **Select Vector** node.
- 12. Then, pull from the forward location to **B** of the **Select Vector** node.
- 13. Select the **Select Vector** node and pull from **Return Value** into the **Dest** pin of **Move to Location**.
- 14. Select the nodes and create a comment:

![](img/aaaa027482c900e215ba6bfab3c17337_48_1.png)

Let's see how this looks in-game:

![](img/aaaa027482c900e215ba6bfab3c17337_49_0.png)

It seems to work perfectly! This will create the chase for the next addition to this project.

## Reviewing the current progress

So now, we've created our Hero character, who isn't much of a hero as he runs indefinitely, but that is another story. Here is the current round of the changes:

- We've updated the AI instructions
- We've demonstrated a basic sensory component

Now, let's move on to creating the Enemy AI.

## The Enemy logic

Enemy needs to be able to find and run toward Hero. We will achieve this by searching for the Hero character, calculating the difference in direction, and facing Enemy toward Hero.

## Adding the Enemy AI

Let's go back to Unreal Engine and focus on Content Browser. We now need an opponent. To add this, follow these steps:

- 1. Right-click and select Blueprint.
- 2. At the lower end of the window, let's drop all the classes and search for AIController.
- 3. Select AIController under Controller and hit Select in the lower-right corner.
- 4. We will name this AIController Enemy.
- 5. Open Enemy AIController and go to the EventGraph section.

First, we must find Hero and then store it in a local variable to be used at any time. To do so, follow these steps:

- 1. Right-click in an empty area within EventGraph and search for Event Begin Play.
- 2. Pull from the exec pin and search for Get All Actors Of Class.
- 3. Set the Actor Class pin to Hero.
- 4. Pull from the **Out Actors** array and search for **ForEachLoopWithBreak**:

![](img/aaaa027482c900e215ba6bfab3c17337_51_0.png)

We want a filter for the `Hero` class within the `MyCharacter` pawn returned. To do so, follow these steps:

- 1. Pull from the **Array Element** pin and search for **Cast to MyCharacter**.
- 2. Then, pull from the **As My Character** pin and search for **Get Controller**.
- 3. Get the class of the controller using the **Get Class** node.
- 4. Compare the `Hero` class with the **Class = Class** node.
- 5. Pull from the return of **Equal** and create a **Branch** node.
- 6. From the **Loop Body** pin, link the newly created branch.
- 7. From the **True** exec pin of the **Branch** node, create the **SET** node.
- 8. Then, pull from the pure cast to the **SET** node we just created.

- 9. We want to comment this and call this section Find Hero:

![](img/aaaa027482c900e215ba6bfab3c17337_52_0.png)

We now have to constantly update the Enemy instructions to moving toward the fleeing Hero character. Note that there are that nodes that can directly achieve this, such as Simple Move to Actor, AI MoveTo, and so on. We will implement similar behavior to give you a look at how this can be done under the hood. Perform the following steps:

- 1. Right-click on the **EventGraph** section and search for **Event Tick**.
- 2. From the exec pin, drop and search for **Delay**.
- 3. Set the **Duration** pin to .05 seconds.
- 4. Pull from **Completed** and create a new node **Move to Location**:

![](img/aaaa027482c900e215ba6bfab3c17337_52_1.png)

Now, we need to get the direction from Enemy to Hero and move Enemy in this direction. We can do this thus:

- 1. Get the `Hero` variable and drop it near the **Delay** node.
- 2. From the `Hero` variable, get the actor's location.
- 3. Pull from **Return Value** of the **Get Actor Location** node, drop it, and search for **Get Direction Vector**.
- 4. Now, right-click and search for **Get Controlled Pawn**.
- 5. Pull from the **Return Value** pin of **Get Controlled Pawn** and **Get Actor Location**.
- 6. Then, pull from the **Return Value** pin of the **Get Actor Location** node and plug it into the **From** pin in the **Get Direction Vector** node.
- 7. Pull from the **Return Value** pin of the **Get Direction Vector** node and **Make Rot from X**.
- 8. Pull from **Make Rot From X** and **Get Forward Vector**.
- 9. Now, from **Get Forward Vector**, we will multiply it by **255** to get **255** units in the forward vector direction.
- 10. Lastly, we will add this to the **Get Actor Location** node of the Hero pawn.
- 11. The result of the addition is the destination for **Move to Location**:

![](img/aaaa027482c900e215ba6bfab3c17337_53_0.png)

- 12. Now, place a comment around this and name it `Chase Hero`.

Save it all!

Now, head back to the Viewport section and hit Simulate. You should now see our Hero character is running forward and turning right when it detects an obstacle in its way. Our Enemy character is chasing fast behind our Hero character. Now, imagine if you were in the Hero character's place; you would be in complete fear!

## Summary

What we demonstrated here is how you can create an Enemy AI that pursues a player or another AI. This type of behavior can be used to create a challenge and provide the player with feedback, which results in a better gameplay experience. AI can definitely be more complicated, but this should only be done if it is needed to improve the play experience. In the next chapter, we will add randomness and probability to our AI character to make it behave in a more interesting way.


Comment this area Clear any values:

![](img/aaaa027482c900e215ba6bfab3c17337_142_0.png)

- 4. Next, before the previously commented part named **Don't follow**, followers **as the leader**, we want to add space between this and the **Loop Body** pin from the **ForEachLoop** node.

- 5. First, we need to calculate **NAlignment**, which is the behavior responsible for steering the nearby agents in the same direction. We will get the average direction vector of nearby agents.

- 6. Now, we want to set a local variable, **NAlignment**, and we will do this by first getting our **Direction** variable from the **Cast to FlockingBall** node. Now, we will need to multiply the **Direction** value by an intensity based on this agent's distance from our agent.

- 7. We first need to calculate the vector length between us and the agent. We will do this by subtracting our location from the other agent's location. Then, we will get the **VectorLength** value from the results. Next, we will create **Map Range Clamped** and plug in the results from the **VectorLength** node into the **Value** pin.

- 8. Now, to configure this **Map Range Clamped** node, let's set **In Range B** to **300.0** and then set **Out Range A** to **1.0**. This will result in full intensity when it is at 0 units and there is no intensity beyond 300 units. This return now needs to be multiplied by the **Direction** value.

- 9. Then, we will add the results of this to **NAlignment** and set this back to the local **NAlignment** variable:

![](img/aaaa027482c900e215ba6bfab3c17337_143_0.png)

- 10. Now, we must calculate Cohesion, which is the behavior responsible for steering the agent toward the center of the nearby agents. This is the direction to the agent's center of mass.

- 11. First, let's create a **GetActorLocation** (A) node and a **Lerp (vector)** node that connects the **A** pin and the **GetActorLocation** (B) node from the reroute. The **Alpha** value should be **Return Value** of **Map Range Clamped** from step 6.

- 12. Then, we want to add **Return Value** of the **Lerp (vector)** node to **NCohesion**. Next, we will set our local **NCohesion** variable with the final results. This will now update our **NCohesion** variable for each agent nearby:

![](img/aaaa027482c900e215ba6bfab3c17337_143_1.png)

- 13. The last behavior to calculate is Separation; this is responsible for forcing the agent away from nearby agents. This is the **Direction** vector from our agent to the other agent.

- 14. We first want to subtract the **GetActorLocation** node from our agent's **GetActorLocation** node. Then, this will be B on a **Lerp (vector)** node. We will go back to **Return Value** of the **Map Range Clamped** node and plug this into the **Alpha** pin of the **Lerp (vector)** node. We will leave A blank.

- 15. Then, we must add the **Return Value** of **Lerp (vector)** to **NSeparation**. Lastly, we will set our local **NSeparation** variable with the results. Comment this area **Calculate Each Behavior for nearby Agents**:

![](img/aaaa027482c900e215ba6bfab3c17337_144_0.png)

- 16. Focus in on **Completed** from the **Loop Body** variable. Now, we must finish calculating the three behaviors.

- 17. Let's get the length of the array returned by **SphereOverlapActors** so that we will know how much to divide to create an average. We will then convert this Int to Float. From there, we can divide the **NAlignment** variable and normalize the results using the **Normalize** node. Then, we must get the global variables we defined in **RollingGameMode**.

- 18. Create a **Get Game Mode** node and cast it to `RollingGameMode` to get access to our variables. Next, we will get the **Global Alignment** variable from the RollingGameMode casting and multiply it by the **Normalize** results from the previous step. Then, the results of this must be set into the local **NAlignment** variable:

![](img/aaaa027482c900e215ba6bfab3c17337_145_0.png)

- 19. Next, we must calculate Cohesion. For this, we must divide the local **NCohesion** variable by the array length and then subtract it from our **GetActorLocation** node. We will then apply the **Normalize** function to gives us the results we need.

- 20. Next, we will multiply the results by our **Global Cohesion** variable from RollingGameMode. Then, set this to our local **NCohesion** variable:

![](img/aaaa027482c900e215ba6bfab3c17337_145_1.png)

- 21. Similar to the processes before, we want to get our local **NSeparation** variable and divide it by the array length from before. Multiply the results by -1. Then, multiply the results by the global **Global Separation** variable.

- 22. Finally, we will get the results and set our local NSeparation variable. Comment this area Calculating Flocking Steering Behavior - outputs NCohesion, NAlignment, NSeparation:

![](img/aaaa027482c900e215ba6bfab3c17337_146_0.png)

- 23. Then, we have to find Calculate Direction and add each of our variables to the final calculation of the Direction variable for this agent:

![](img/aaaa027482c900e215ba6bfab3c17337_146_1.png)

- 24. The last thing we need to do is prepare for the next section in this course. So, let's find an empty place above Blueprint to the left and add some new code.

- 25. Add a new custom event called ResetBall.

- 26. Next, we want to create a SetActorLocation node for this agent and set New Location to the Start Location variable that we set at the beginning.

- 27. Now, we want to set a new direction, and we will do this by first placing a **Direction** variable of the **SET** node. Then, we will split the struct and only apply to **Return Value X** and **Return Value Y** from a **Random Unit Vector** node:

![](img/aaaa027482c900e215ba6bfab3c17337_147_0.png)

## Controlling behavior through UMG

In this part of the chapter, we want to touch on using UMG to control the behaviors influenced upon our agents. We will do this by first creating a UMG widget with the proper controls to manipulate our three float variables. Then, we must assign this user widget to the owning PlayerController. Then, we will end by adding a function that will reset the Agents to their original position, starting the simulation fresh.

### A simple UI

Let's navigate to our Content Browser, create a new Widget blueprint, and name it `FlockingUI`. Let's open this up and go to the **Designer** tab to get started, as follows:

- 1. Let's drag **Vertical Box** into our **Hierarchy** panel.
- 2. Next, set the slot (**Canvas Panel**) properties: the **Size X** value to 350.0 and the **Size Y** value to 600.0.
- 3. Then, drag a button into our **Hierarchy** panel under **Vertical Box**. Rename the button **ResetButton**. Next, set the **Vertical Box** slot's **Padding** properties to 75.0 and 25.0.

- 4. Lastly, drag Text into our Hierarchy panel under ResetButton. Next, set the text property Text to "Reset":

![](img/aaaa027482c900e215ba6bfab3c17337_148_0.png)

- 5. Now, let's drag Horizontal Box into our Hierarchy panel under Vertical Box. Then, add two more widgets within Horizontal Box, which are called Text and Slider.

- 6. Now, we must duplicate Horizontal Box and its children two more times within the Hierarchy panel.

- 7. Right-click and copy Horizontal Box and then paste it into Vertical Box twice:

![](img/aaaa027482c900e215ba6bfab3c17337_148_1.png)

- 8. After this, we want to rename the widgets we will use for organization purposes. In a sequence, we want to name the first slider AlignmentSlider, the second CohesionSlider, and the last one SeparationSlider.

- 9. Now, in sequence, let's set our text property Text to these three names: Alignment, Cohesion, and Separation. Then, we will set our Padding text property to 7.5.

Now that this is completed, you should have something similar to the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_149_0.png)

- 10. To update our Global variables, we must create an event from the three sliders. We can start with AlignmentSlider, then go under Events, and then click on the + option on OnValueChanged. Then, when this event is called, we will update the Global Alignment variable in RollingGameMode.

- 11. Now, we must do this for the other two sliders — CohesionSlider and SeparationSlider — with the respective global value:

![](img/aaaa027482c900e215ba6bfab3c17337_149_1.png)

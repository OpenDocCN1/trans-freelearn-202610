9. Then, we will add the results of this to **NAlignment** and set this back to the local **NAlignment** variable:

![](bbox=[0.175, 0.19, 0.825, 0.405])

10. Now, we must calculate Cohesion, which is the behavior responsible for steering the agent toward the center of the nearby agents. This is the direction to the agent's center of mass.

11. First, let's create a **GetActorLocation** (A) node and a **Lerp (vector)** node that connects the **A** pin and the **GetActorLocation** (B) node from the reroute. The **Alpha** value should be **Return Value** of **Map Range Clamped** from step 6.

12. Then, we want to add **Return Value** of the **Lerp (vector)** node to **NCohesion**. Next, we will set our local **NCohesion** variable with the final results. This will now update our **NCohesion** variable for each agent nearby:

![](bbox=[0.18, 0.595, 0.82, 0.83])
1. In the **Components** panel, select the **PawnSensing** component. In the **Details** panel, look in the **Events** category and click the green button of the **On Hear Noise** event to add it to the Event Graph:

![](bbox=[0.12, 0.17, 0.86, 0.42])

Figure 10.15 – Adding the On Hear Noise event

2. In the **On Hear Noise**, we call the **Update Sound BB** macro if the distance between the sound location and the enemy is less than the value of **HearingDistance**:

![](bbox=[0.12, 0.57, 0.86, 0.76])

Figure 10.16 – The On Hear Noise actions

3. Drag a wire from the white pin of the **On Hear Noise (PawnSensing)** node and add a **Branch** node. The next steps will create an expression that uses the **VectorLength** function to calculate
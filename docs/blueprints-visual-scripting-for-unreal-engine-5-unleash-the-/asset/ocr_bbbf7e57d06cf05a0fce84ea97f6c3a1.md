+   1. Open the Ue4ASP_Character Blueprint located in the Content > AnimStarterPack folder.

+   2. In the My Blueprint panel, create a variable named Proring of the Boolean type:

![](bbox=[0.125, 0.205, 0.64, 0.35])

Figure 17.32 – Creating the Proring Boolean variable

+   3. Right-click on some empty space in the EventGraph and search for prone. Select the Prone event to place the node:

![](bbox=[0.125, 0.475, 0.7, 0.635])

Figure 17.33 – Adding the input action Prone event

+   4. Add a Set Proring node and a Get Proring node near the Prone event. Also, add the Not Boolean node and connect them, as shown in the following screenshot. The Not Boolean node will toggle the Boolean value (true/false) of the Proring variable every time the Prone event is executed:
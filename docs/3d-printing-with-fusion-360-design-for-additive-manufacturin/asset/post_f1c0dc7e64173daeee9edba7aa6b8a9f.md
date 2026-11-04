
![](img/baa027bed101c08b496f1f420cee8a5b_101_0.png)

Figure 3.7 – The interference between the microphone and the organic connection arm

To address this interference and to create shapes that do not interfere with the microphone, let's go ahead and suppress the AutomatedModeling3 feature from the timeline and create a new automated modeling feature. This time, in addition to selecting the four faces to connect, we will also select two bodies to avoid: Rotation Axis and Microphone, and select the Opacity checkbox so the bodies to avoid are transparent. The new setup for this automated modeling can be seen in Figure 3.8.

![](img/baa027bed101c08b496f1f420cee8a5b_101_1.png)

Figure 3.8 – A new automated modeling setup with a couple of bodies to avoid

After generating the new shapes, we now see six new design alternatives to choose from. And according to Fusion 360, they all avoid interfering with the microphone and the modeled cables. After inspecting the outcomes, let’s choose **Alternative 4** and adjust its volume slider to the minimum and select **OK** to generate the new body, as shown in *Figure 3.9*:

![](img/baa027bed101c08b496f1f420cee8a5b_102_0.png)

Figure 3.9 – Six new design alternatives that avoid interfering with the microphone

As we can see in *Figure 3.10*, we now have a new body named **Body46** in the **BROWSER** panel and we can see that a new automated modeling feature has been added to the timeline. Upon closer inspection, we can see that the new body is not interfering with the cables. However, this was done by removing the microphone and the cables from the newly generated shape. This new body theoretically meets our objectives; however, in practice, we need a much larger clearance between our cables and the connection arms so we can plug and unplug these wires:

![](img/baa027bed101c08b496f1f420cee8a5b_103_0.png)

Figure 3.10 – Outcome of the second iteration of automated modeling

To achieve this new objective of a larger clearance between our microphone and the new connection arms, we need to create a new geometry. Fusion 360 can be utilized to create a new sketch along the XZ axis, and we can project the profile of the microphone and, if needed, offset it to create an additional tolerance zone. Later, we can use the profile we created and use the REVOLVE command to create a 130-degree revolution, as seen in Figure 3.11:

![](img/baa027bed101c08b496f1f420cee8a5b_103_1.png)

Figure 3.11 – Creating a new body using the REVOLVE command based on the profile of the microphone

Once we generate this new body, we can rename it. In this example, I renamed the new body Avoid Zone. Now, we’re ready for our third and final automated modeling feature. Let’s go ahead and suppress our prior automated modeling features and activate automated modeling one last time. After selecting the same four faces to connect, we can now select the newly created **Avoid Zone** as an input for the bodies to avoid. The new setup for the final automated modeling can be seen in *Figure 3.12*:

![](img/baa027bed101c08b496f1f420cee8a5b_104_0.png)

Figure 3.12 – Final setup for automated modeling with the newly created Avoid Zone

After generating the new shapes, we now see six new design alternatives to choose from. If we choose **Alternative 6** and utilize the minimum volume, we will end up with the model preview that we can see in *Figure 3.13*:

![](img/baa027bed101c08b496f1f420cee8a5b_105_0.png)

Figure 3.13 – Alternative 6 for the final automated modeling setup

It is always a good idea to take a closer look at the outcome and make sure it meets our design objectives and that the output geometry is geometrically pleasing. As we can see in *Figure 3.14*, Fusion 360 created a new body utilizing T splines to create the shapes, used the boundary fill command to convert surface shapes into solid objects, and finally, used the combined feature cut operation to remove avoid bodies from the output shape. This final action of combined cut created two notches in our otherwise organic-looking geometry. We can easily make a change to the combined feature and remove one of the four tool bodies (in this case, **Avoid Zone**) to eliminate these two notches.

![](img/baa027bed101c08b496f1f420cee8a5b_106_0.png)

Figure 3.14 – Automated modeling creates a group of features that can be edited individually

With this final edit, we have created a design alternative that both fits our requirements and is drastically different compared to the original design. But is this design lightweight compared to the original? Even though we did not explicitly enter criteria around lightweighting the part as we were generating, we were consistently selecting the minimum volume for each selected design alternative. Let's take a closer look at the original part and its properties. As we can see in Figure 3.15, the component named **Original Connection** had a volume of 41,843 millimeters cubed. Note that to access the **PROPERTIES** dialog, we first created a new component and moved the bodies named **Connection-left** and **Connection-right** into this new component. Later, we right-clicked and selected **Properties** to access the **PROPERTIES** dialog.

![](img/baa027bed101c08b496f1f420cee8a5b_106_1.png)

Figure 3.15 – Mass and Volume properties of the original design

If you repeat the same steps, create a new component, and move the new connection body into this new component, we can take a look at its properties and see that the volume for this new component is 16,591 millimeters cubed. This means that the new design is roughly 40% of the volume and mass of the original design. Figure 3.16 shows the properties of the new connection body created using automated modeling.

![](img/baa027bed101c08b496f1f420cee8a5b_107_0.png)

Figure 3.16 – Mass and Volume properties of the new design

Even though automated modeling gave us both an organic look and a lightweight design, we do not know for sure how this geometry will perform in a real-world setting. In order to verify that our model can withstand the loads associated with the weight of the microphone in various positions, we have to either build a prototype and test it or perform a simulation. Another tool we could have used to generate both organic and lightweight designs is generative design. Generative design is its own workspace within Fusion 360 and requires similar inputs for connecting faces and obstacle geometry. Using generative design, you also have the capability to enter loads and boundary conditions to simulate the forces this connection arm will have to bear. In addition, generative design allows users to select which manufacturing model they wish to produce the output geometry. Options such as Additive manufacturing, subtractive manufacturing (2.5 axis, 3 axis, or 5 axis milling machines), as well as 2 axis cutting and Die casting, can be accounted for. Generative design also requires you to select which materials should be considered when creating designs that meet the requirements. As you can see in Figure 3.17, you can generate tens, if not hundreds, of outcomes based on the manufacturing method and material that meet certain design objectives.

![](img/baa027bed101c08b496f1f420cee8a5b_108_0.png)

Figure 3.17 – Generative design outcomes of a motorcycle component that meets design criteria

Regardless of the method we choose to generate organic and lightweight geometries, it is always good practice to perform a final simulation to make sure that these computer-generated shapes meet our performance criteria.

In this section, we covered how to apply automated modeling to generate organic-looking shapes. We went through an iterative process of design utilizing automated modeling and updated our design criteria to meet a certain objective. We used the microphone stand as an example and compared the original design to the final design to ensure the outcome was a more lightweight version. In the next section, we will follow a similar example using the Shape Optimization simulation method to make sure that the design we create considers the performance criteria.

## Utilizing Shape Optimization

In the previous section, we covered how to use automated modeling in order to generate an organic and lightweight design. In this section, we will introduce performance-based modeling and learn about how to utilize Shape Optimization for generating similar outcomes.

Shape Optimization is an analysis type available within the SIMULATION workspace of Fusion 360. Using Shape Optimization, engineers can create lightweight designs based on predetermined performance criteria. However, it is important to note that the outcome of a shape optimization study does not actually reflect the stresses created by the loads. Therefore, we should always validate our new design using additional simulations such as a static stress study. In this section, we will focus

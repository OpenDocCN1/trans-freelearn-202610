As a final touch, we can select the two faces that are between our hole and the slot and create a fillet using the FILLET feature located in the SOLID tab's MODIFY panel. While creating the fillet, we can either enter a fillet radius or utilize the Full Round Fillet type. We can repeat the same process for all four holes and create fillets for eight faces, as shown in Figure 6.30:

![](bbox=[0.13, 0.18, 0.86, 0.52])

Figure 6.30 – Adding full round fillets to improve print bed adhesion

Depending on which combination of feature modifications we choose to utilize, we will end up with a manufacturing model that has the part edits we need for our manufacturing process. These edits do not impact the original design but are stored in their respective manufacturing models.

# Summary

In this chapter, we talked about modifying parts for manufacturing purposes. We started this chapter by discussing how to use the Insert Derive workflow within Fusion 360 to manage our model changes for 3D printing. We utilized this workflow to aggregate components from multiple Fusion 360 designs and make changes that impact the downstream manufacturing processes but do not impact the upstream design document. We talked about the benefits of this workflow as it allowed us to bring in designs, along with their parameters, from multiple sources into a single manufacturing document. We also highlighted how utilizing this workflow blocks us from deleting source components, so long as those components are referenced in our manufacturing document.

Next, we introduced the concept of the MANUFACTURE workspace and how to create manufacturing models. We demonstrated how we can modify our parts within the manufacturing model of the same document. We highlighted that the main benefit of this workflow is its ability to store all relevant data
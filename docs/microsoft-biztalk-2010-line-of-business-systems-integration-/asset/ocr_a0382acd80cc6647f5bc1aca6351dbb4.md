+   c. **TrainingServiceRequest** of multi-part type **Chapter6. BuildingAccessManagement.BizTalk.ITrainingService_GetTrainingForBuildingAccess_InputMessage**
+   d. **TrainingServiceResponse** of multi-part type **Chapter6. BuildingAccessManagement.BizTalk.ITrainingService_GetTrainingForBuildingAccess_OutputMessage**

9. Add a filter on the initial receive shape of the orchestration. This filter will ensure that only messages from Riley Semiconductor come into this orchestration.

![](bbox=[0.24, 0.31, 0.76, 0.45])

10. After the topmost receive shape, add a **Construct/Transform** block that is responsible for building the service's request message.

11. Double-click the **Transform** shape and choose the option to create a new map named **BizTalk.AccessRequest_To_TrainingServiceRequest**. This map should start with the **AccessRequestInput** message and output the **TrainingServiceRequest** message.

12. Within the map, connect the **LocationId**, **BuildingNumber**, and **RoomNumber** to the corresponding nodes in the target schema. Save the map and return to the orchestration.

13. Add a **Send** shape and **Receive** shape below the previous **Construct** shape. These new shapes are responsible for calling the TrainingService and receiving the response message. Set the **Send** shape to use the **TrainingServiceRequest** message and the **Receive** shape will use the **TrainingServiceResponse** message.

14. Create a new, configured port on the **Port Surface** portion of the orchestration. For the **Port Type** value, choose to use an existing port type and point to the one created by the service reference. Our orchestration should look like the following image:
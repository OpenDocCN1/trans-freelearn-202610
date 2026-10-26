# How to stream semantic models

To implement this recipe, use the following steps:

+   1. Navigate to the workspace where North American Sales is located in the service.
+   2. Under the workspace name, select New, and then Streaming dataset (semantic model).
+   3. In the New streaming dataset (semantic model) pane, choose PUBNUB and click Next.

![](bbox=[0.22, 0.22, 0.79, 0.44])

Figure 5.24: New streaming semantic model options

+   4. Name the semantic model Streaming Sensor Data.
+   5. Use the following Sub-key: sub-c-99084bc5-1844-4e1c-82ca-a01b18166ca8.
+   6. Use the following Channel: pubnub-sensor-network.
+   7. Click the Next button.
+   8. The next page shows the schema of the semantic model; click the Create button.
+   9. The Streaming Sensor Data semantic model appears in the workspace.
+   10. Navigate to the North American Sales dashboard.
+   11. In the dashboard ribbon, choose Edit and then Add a tile.
+   12. In the Add a tile pane, choose Custom Streaming Data from the REAL-TIME DATA area and then click the Next button.
+   13. Under YOUR DATASETS, choose Streaming Sensor Data, and click Next.
+   14. For Visualization Type, choose Line chart.
+   15. For the Axis, use timestamp, and for Values, use ambient_temperature.
+   16. Click the Next button.
+   17. Enter the Title name as Temperature for the tile and click the Apply button.
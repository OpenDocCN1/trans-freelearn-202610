+   - **ClippingRegion**: Used to target a specific region using coordinates such as **Left**, **Right**, **Top**, and **Bottom**. The values in the clipping regions are blank by default. You may input the values of the coordinates if available.

+   - **SendWindowMessages**: Sends specific messages to the target application and can run in the background, but it is not the fastest option and is not compatible with all desktop applications. By default, the **SendWindowMessages** checkbox is not selected.

+   - **SimulateType** and **SimulateClick**: Sends specific messages to the target application and can run in the background; this is the fastest option and compatible with all desktop applications. By default, the **SimulateType** or **SimulateClick** checkbox is not selected.

+   - **Target**: Helps the robot in different ways to identify target elements by providing inputs in the form of selectors, fuzzy selectors, images, or anchors. **Selector** is a commonly used method to identify elements; it’s a key property for the robot to identify the target element using an XML fragment that has several attributes to locate the unique target element.

+   - **Output**: Used to store the output of the activity using variables or arguments to be used within the project. By default, the **Output** field will be empty. You may create a variable within the field to store the output generated in the selected activity.

Let's explore an example using https://www.packtpub.com/ to apply a few of the UI Automation activities and properties learned so far to ensure that we can communicate with the web application. I can see a Free 7 Day Trial page at this URL while writing this lesson. If you do not find the same, you can use any other similar pages. In this example, you will be accessing a web application and interacting with the web elements to provide input, and access the data from the web page:

+   1. Launch https://www.packtpub.com/.
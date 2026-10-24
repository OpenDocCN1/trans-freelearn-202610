

Azure Digital Twins services and DTDL provide an environment for application developers to interact with Digital Twins and create solutions to specific business challenges. Our maintenance use case is a typical example of such a business challenge that can be solved using the Digital Twin.

The application allows the testing of the Digital Twin development done in the prior section of this chapter. Let's next focus on the testing phase.

## Testing framework

The next step in Figure 6.5 is to create an application to provide maintainability and scalability over a large number of wind turbine instances and to pass real-time telemetry data to the Digital Twin. This adds ease of use to the framework, for real-life scenarios.

## Creating an application to update the Digital Twin prototype in code

**Azure Digital Twins (ADT)** is a **Platform as a Service (PaaS)** solution aimed at developers to create digital twin solutions. For the purpose of the book, we will address two typical development personas. The first is a software developer that has the necessary coding skills to develop a solution by using the APIs and SDKs of ADT. At the time of writing the book, the ADT APIs and SDKs can be accessed in .NET, Java, JavaScript, Python, and Go.

The second persona is that of a subject-matter expert such as a reliability and maintenance engineer in the wind farm. The subject-matter expert has technical skills, but typically does not have the formal software development skills to create applications from scratch.

The remainder of the section will focus on the formal software development approach to creating an application to update the wind turbine model.

![](img/f445915fceb4649a77b6797f1425850b_187_0.png)

Figure 6.15 – Process for creating a custom-coded solution to update Azure Digital Twins

_Figure 6.15_ shows the process for creating a custom-coded solution to update ADT. Step one of the process should have been completed in the previous section of the book. It is recommended to have the models in ADT, as well as one or two instances to test your application as you develop it. These instances of the twin represent the specific wind turbines.

The second step is similar to the app registration process in Figure 5.20 in Chapter 5, Setting Up a Digital Twin Prototype. In this instance, we will create an app with a recognizable name, such as digitaltwinapp. Provide app access to the Digital Twin resource similar to the process in Figure 5.23 in Chapter 5, Setting up a Digital Twin Prototype.

The custom application that uses the ADT API and SDK will now be able to access the ADT service in Azure in the same way as ADT Explorer can access the ADT services. The APIs and SDKs provide the capabilities for the final three steps in Figure 6.14. Using the APIs and SDKs requires software coding skills and access to an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio Code, or any IDE that supports your programming language of choice. We will use the .NET programming platform and Visual Studio or Visual Studio Code, which is available for free from the Microsoft website.

The Azure Digital Twins APIs and SDKs are well documented at http://bit.ly/idt-adtapisdk. The data plane APIs are of specific interest in our custom application as they will create and update the Digital Twin instances and send telemetry data to the Digital Twin instances. The APIs provide access to the Digital Twin models, instances, and event routes. They also enable a developer to programmatically query the models and instances. A detailed list of API operations is available at http://bit.ly/idt-apioperations and it is maintained by Microsoft as new operations are added.

Application examples are available at http://bit.ly/idt-adtapisdk and a basic Create Twin Instance example in C# is as follows:

```csharp
string twinId = "turbine2";
var initData = new BasicDigitalTwin
{
    Id = twinId,
    Metadata = { ModelId = "dtmi.com:idtbook:wt:turbine;1" },
    // Initialize properties
    Contents =
    {
        { "SerialNo", "YF568" },
        { "RatedPower", 1500 },
    },
};
await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twinId, initData);
```

Microsoft provides a full tutorial to code a client app at http://bit.ly/idt-tutorialapp for developers who want to create Azure Digital Twins applications with real-time telemetry data. It provides comprehensive source code, and the link is also listed on the GitHub page for this book. It demonstrates all the steps and actions to create an application that creates Digital Twins models and instances and updates them with telemetry data. This addresses the three last steps in Figure 6.15.

As a developer, you will now have successfully created Azure Digital Twins models and instances and updated the Digital Twins with telemetry data in a custom application. Figure 6.16 shows the additional wind turbine instances that were read from a database and added to ADT through a custom-developed application.

![](img/f445915fceb4649a77b6797f1425850b_189_0.png)

Figure 6.16 – Multi-turbine example

Creating an application, as the last step in the process in Figure 6.15, to view or use the model will follow the same process as described. Another approach is to view the Azure Digital Twin in a business intelligence (BI) or visualization application such as Microsoft Power BI. This approach also requires programming skills, similar to the requirements for creating the ADT application.

![](img/f445915fceb4649a77b6797f1425850b_190_0.png)

Figure 6.17 – Create a Power BI dashboard to view your Digital Twin

Figure 6.17 shows a possible approach to show ADT information on a Power BI dashboard. It follows a similar development approach as described in this Microsoft tutorial on updating a map service from Azure Digital Twins: http://bit.ly/idt-connectmaps. The sample code provided in the GitHub repository for the book includes an Azure function to take the ADT from an event grid and update a Power BI dashboard file as shown in Figure 6.18.

A Digital Twin application differs from an IoT application in the sense that the Power BI application will receive updates from the Digital Twin instances, rather than from individual sensors for the different telemetry points. It acts as a proxy between the state and telemetry updates and the business application. You can create different Power BI dashboards from the single event update service. You can also create multiple applications that are updated from the ADT service without reading from individual sensors each time.

The ADT service also differs from traditional relational databases in the sense that it does not create a new transactional row of data every time it is updated. It overwrites the previous instance state values with new state values. The instance is always a snapshot at a specific point in time of the state of the Digital Twin instance. It requires additional programming to persist data, such as telemetry time series data in a **time series database (TSDB)** such as Microsoft Time series Insights or InfluxDB.

The Power BI dashboard shown in _Figure 6.18_ is used to monitor the operation of the turbines on the wind farm. It is also used to plan maintenance services based on the condition of the turbines from the telemetry readings.

![](img/f445915fceb4649a77b6797f1425850b_191_0.png)

Figure 6.18 – Power BI dashboard example for our wind farm

We have now created an Azure-based Digital Twin as our first Digital Twin prototype application. It is a custom-coded application that can create new ADT models, instances, update telemetry data from IoT sensors, and view the Azure Digital Twins in applications such as Power BI. This approach provides a great deal of flexibility to skilled programmers who can create a wide range of specialist Digital Twin applications. Willow Inc. (https://www.willowinc.com/posts/under-the-hood-willowtwin-x-azure-digital-twins/) is a great example of Azure Digital Twins applications in the construction industry.


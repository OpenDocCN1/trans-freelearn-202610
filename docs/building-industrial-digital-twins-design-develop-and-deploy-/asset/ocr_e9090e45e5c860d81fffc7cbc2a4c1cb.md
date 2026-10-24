> > **Note**
> > Image source: US Department of Energy: `https://www.energy.gov/eere/wind/inside-wind-turbine`

The Digital Twin model will represent the data elements that we want to store for each instance of a wind turbine in the wind farm. The wind turbine model will also include sub-models that describe the data for the motor, gearbox, and generator. We need to distinguish between the different data types as properties, and the telemetry data that we will receive from the sensors on the wind turbines.

The data model that we use for our first Digital Twin contains all the basic elements to describe the wind farm, wind turbines, and telemetry data.

![](bbox=[0.1553, 0.3398, 0.8447, 0.7236])

This data model in Figure 6.4 will be enough for the maintenance use cases in our first Digital Twin of the wind turbine. The next step is to configure the models and sub-models as **Digital Twins Definition Language (DTDL)** JSON files before they can be uploaded to Azure Digital Twins. The data model that we described in Figure 6.4 contains all the data elements but not any of the relationships between the different models. One of the main reasons for using graph-based Digital Twin services such as Azure Digital Twins is to capture non-hierarchical relationships that may exist between assets.
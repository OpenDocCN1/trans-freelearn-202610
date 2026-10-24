The final model for three wind turbines in a specific wind farm should resemble the nodes and relationships in Figure 6.14.

![](bbox=[0.1467, 0.1438, 0.8533, 0.6016])

Figure 6.14 – Create a unique instance for a specific wind farm

You have now successfully created an Azure Digital Twin with models and instances for the wind farm, turbines, and its components. You can now use SQL-like queries to interrogate the instances.

The graph in Figure 6.14 shows that rotor1, gearbox1, generator1, and tower1 are part of turbine1. In this example, the numeric indicators of the components are aligned, which seldom happens in practice. An example of turbine3 with rotor156, gearbox457, generator4213, and tower876 is a better reflection of a real-world configuration.

It is quite a tedious task to capture all the instances manually. Just imagine loading 7,000 wind turbines and their components manually; it will certainly result in human data capture errors. A graph with hundreds or thousands of nodes is not the best user experience for manual navigation and data capture. We also notice that the telemetry fields cannot be updated from ADT Explorer as this is typically streaming data from sensors.
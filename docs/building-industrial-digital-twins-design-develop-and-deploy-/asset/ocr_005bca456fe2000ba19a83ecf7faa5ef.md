The Digital Twin knowledge graph in Azure Digital Twins can be visualized with Azure Digital Twins Explorer (https://bit.ly/idt-dtdlx), which shows the relationship between different Digital Twin models. It is a sample application that demonstrates how you can do the following:

+   • Upload and explore the models and graphs of DTDL-based Digital Twins.

+   • Visualize the Digital Twin graph with several layouts.

+   • Edit the properties of DTDL Digital Twins and run queries against the graph.

The following screenshot shows an example of such a knowledge graph based on the DTDL model of a composite Digital Twin. A description of this example is available at https://bit.ly/idt-dtdlx:

![](bbox=[0.1172, 0.3496, 0.8457, 0.7094])

Figure 4.11 – DTDL-based Digital Twin graph in the Azure Digital Twins service

DTDL is currently less complicated than AAS, but it is also limited in its scope and capability. It does not store historical data on the Digital Twin as it only records the current state. If the temperature input from a sensor changes, the current value is overwritten with the new value.
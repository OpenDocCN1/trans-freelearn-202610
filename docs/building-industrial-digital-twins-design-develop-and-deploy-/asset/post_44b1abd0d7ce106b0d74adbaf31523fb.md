

The DTDL model, in JSON format, for the wind turbine includes additional metamodel classes for telemetry and relationships. It also describes all the data model elements for the turbine that we defined in the *Basics of a wind turbine and a high-level data model* section:

```json
{
 "@id": "dtmi:com:idtbook:wt:turbine;1",
 "@type": "Interface",
 "@context": [
 "dtmi:dtdl:context;2"
 ],
 "displayName": "WT Turbine",
 "description": "--",
 "contents": [
 {
 "@type": "Property",
 "name": "SerialNo",
 "schema": "string"
 },
 {
 "@type": "Property",
 "name": "RatedPower",
 "schema": "double"
 },
 . . . (See Github for full json files)
 {
 "@type": "Telemetry",
 "name": "Windspeed",
 "schema": "double"
 },
 {
 "@type": "Telemetry",
 "name": "NacelleAngle",
 "schema": "double"
 },
 {
 "@type": "Telemetry",
 "name": "State",
 "schema": "string"
 },
 {
 "@type": "Relationship",
 "name": "isMemberOf",
 "displayName": "MemberOf",
 "minMultiplicity": 0,
 "maxMultiplicity": 1,
 "target": "dtmi:com:idtbook:wt:farm;1"
 }
 ]
}
```

The sub-model for the generator, in turn, contains the relationship with the turbine model, as shown in Figure 6.4:

```json
{
 "@id": "dtmi:com:idtbook:wt:gearbox;1",
 "@type": "Interface",
 "@context": [
 "dtmi:dtdl:context;2"
 ],
 "displayName": "WT Gearbox",
 "description": "--",
 "contents": [
 {
 "@type": "Property",
 "name": "GearboxSerial",
 "schema": "string"
 },
 ... (See Github for full json files)
 {
 "@type": "Relationship",
 "name": "isPartOf",
 "displayName": "PartOf",
 "minMultiplicity": 0,
 "maxMultiplicity": 1,
 "target": "dtmi:com:idtbook:wt:turbine;1"
 }
 ]
}
```

The DTDL JSON files for the wind farm, turbine, and its components are available to download in the GitHub repository for the book at `https://github.com/PacktPublishing/Building-Industrial-Digital-Twin`.

It includes the following files:

- `Farm.json`
- `Turbine.json`
- `Rotor.json`
- `Gearbox.json`
- `Generator.json`
- `Tower.json`

In the next section, we will upload the model files to our Azure Digital Twins instance using the Azure Digital Twins Explorer application.

## Uploading models to the Azure Digital Twins instance

In Chapter 5, Setting Up a Digital Twin Prototype, we configured the Azure Digital Twins service and suggested that you set up Azure Digital Twins Explorer. ADT Explorer is an application that enables you to view your Digital Twin in a graph visualization. ADT Explorer is not an officially supported product by Microsoft at the time of writing, even though it is supported by Microsoft as open source. It is not recommended for large-scale deployments but provides the basic capabilities that we require to build our first Digital Twin. The sample application, source code, and installation instructions can be downloaded from GitHub (`http://bit.ly/idt-adtexplorergit`).

Log in to your free Azure service that you configured in *Chapter 5, Setting Up a Digital Twin Prototype*, before starting the ADT Explorer application.

![](img/f445915fceb4649a77b6797f1425850b_181_0.png)

Figure 6.9 – Upload DTDL JSON files with Azure Digital Twins Explorer

Select all the wind farm and wind turbine models and sub-models and upload them to ADT Explorer, as shown in *Figure 6.9*. This will create the model files in your Azure Digital Twins service that you configured in *Chapter 5, Setting Up a Digital Twin Prototype*.

The models are now available in the model repository on the left, as shown in *Figure 6.10*.

![](img/f445915fceb4649a77b6797f1425850b_182_0.png)

Figure 6.10 – DTDL models available for use in Azure Digital Twins

The next step in the configuration of our first Digital Twin, as shown in *Figure 6.11*, is to create Digital Twin instances for our physical wind turbines. These instances will each be based on the Digital Twin models that we now have in the Azure Digital Twins service.

## Configuring Digital Twin instances for the wind farms and turbines

A wind farm can contain hundreds and even thousands of wind turbines. The Jiuquan Wind Power Base in China has about 7,000 wind turbines (see `https://www.power-technology.com/features/feature-biggest-wind-farms-in-the-world-texas`).

One of the primary benefits of the Digital Twin software design approach is the ability to have a single Digital Twin model and several thousand Digital Twin instances. A change to the attributes in the model will make the attribute available to all the thousands of applicable instances. Digital twin applications can be developed to address specific business needs based on the information architecture in the Digital Twin model.

To create the first wind farm Digital Twin instance, select the + button next to the **WT Farm** model (see *Figure 6.11*) and proceed to give the instance a unique name. This name will be the unique identifier or ID for the instance of the physical twin and cannot be changed later. This can be a serial number, an asset number, or any unique descriptor. For the purpose of the book, we will use windfarm1 as the unique name.

![](img/f445915fceb4649a77b6797f1425850b_183_0.png)

Figure 6.11 – Create a unique instance for a specific wind farm

Save the twin name, and the first instance of a wind farm will be created based on the wind farm DTDL model and the unique `windfarm1` ID.

![](img/f445915fceb4649a77b6797f1425850b_184_0.png)

Figure 6.12 – Update instance-specific Digital Twin values

You can edit the properties and edit the information for this instance in the right-hand property explorer pane, as shown in Figure 6.12. This will update the instance with that value in the field. We can update the name of `windfarm1` to `San Jose South Wind Farm`. The `$dtid` ID will remain unique and cannot be edited, but the values of the properties can be changed through ADT Explorer. `$dtid` is the instance ID, like the primary key in a relational database table.

Next, we proceed to create a wind turbine instance in a similar process for the WT Turbine model and update the specific instance data. When we have the instances for both the wind farm and the first turbine, we can proceed to link these instances with the relationship option in the top bar.

![](img/f445915fceb4649a77b6797f1425850b_185_0.png)

Figure 6.13 – Set the relationship between two instances

First, select the turbine, then select the wind farm and click on the relationship icon on the top bar, as shown in Figure 6.13. This will open a dialog box with a dropdown of all the types of relationships that you defined in the DTDL model. In this instance, we only have the **MemberOf** relationship available. Saving this will create an arrow between the two instances with a description on the arrow of the relationship. Proceed with setting up instances for additional turbines and creating unique instances for each component: rotor, gearboxes, generators, and towers. Also update the properties and values for each instance of the turbines and components.

The final model for three wind turbines in a specific wind farm should resemble the nodes and relationships in Figure 6.14.

![](img/f445915fceb4649a77b6797f1425850b_186_0.png)

Figure 6.14 – Create a unique instance for a specific wind farm

You have now successfully created an Azure Digital Twin with models and instances for the wind farm, turbines, and its components. You can now use SQL-like queries to interrogate the instances.

The graph in Figure 6.14 shows that rotor1, gearbox1, generator1, and tower1 are part of turbine1. In this example, the numeric indicators of the components are aligned, which seldom happens in practice. An example of turbine3 with rotor156, gearbox457, generator4213, and tower876 is a better reflection of a real-world configuration.

It is quite a tedious task to capture all the instances manually. Just imagine loading 7,000 wind turbines and their components manually; it will certainly result in human data capture errors. A graph with hundreds or thousands of nodes is not the best user experience for manual navigation and data capture. We also notice that the telemetry fields cannot be updated from ADT Explorer as this is typically streaming data from sensors.


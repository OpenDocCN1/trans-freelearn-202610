

When we do the technical evaluation of the Digital Twin, we must consider the integration with the systems at the M&D center. The data from different systems may have different levels of granularity; for instance, the weather forecast, such as wind speed and temperature, may be hourly, but the telemetry data from the wind turbine may be recorded every second. The data integration at the M&D center would have to handle such complexities.

Next, let’s look at the business validation considerations of the Digital Twin of the wind turbine.

## Business validation and understanding

According to a wind energy publication from July 2013, titled _Monitoring wind turbine gearboxes_, more failures have been observed in the gearbox than in the other sub-assemblies in the wind turbines. According to the US **Energy Information Administration (EIA)**, the expected lifespan of a wind turbine is about 20-25 years. The gearboxes are designed for about 20 years, but most have experienced a shorter life. The gearbox is also one of the most expensive sub-assemblies in the wind turbine. It can be as much as 13% of the cost of the wind turbine. The repair and replacement process of the gearbox is complex, as it is located at a great height and leads to long downtime. Figure 6.28 shows a view of the gearbox in a wind turbine. The oil in the gearbox protects the pitch gear, open gear, and yaw gear.

![](img/f445915fceb4649a77b6797f1425850b_200_0.png)

Figure 6.28 – Wind turbine sub-assemblies

![](img/f445915fceb4649a77b6797f1425850b_201_0.png)

One of the common causes of failures in the bearings of wind turbines is **white etching cracks (WEC)** as these bearings are subjected to a high degree of force and torque. WEC is one of the leading causes of reliability issues in the bearings of wind turbines. WEC causes structural changes in the bearings’ material below the surface. The resulting cracks eventually extend to the surface of the bearing, when they are under stress, leading to failures of the bearing. Proper lubrication maintained by monitoring the condition of the gearbox oil can help to reduce the incidents of WEC in wind turbine bearings. The condition of the oil has a key role in the overall reduction of the friction and wear and tear of the moving parts in the gearbox. According to a publication by Exxon-Mobil, the average age of wind turbine farms is 7 years in 2020. Based on an average 5- to 10-year manufacturer warranty on wind turbines, this topic of field maintenance is very timely and relevant from a business standpoint.

Historically, the monitoring of the gearbox oil was carried out in offline mode by sampling the gearbox oil. A sample of oil taken from the wind turbines was sent to the laboratory for analysis, examining properties such as the viscosity and ferrous and non-ferrous particle levels. The Digital Twin of the wind turbine, along with the telemetry, can facilitate the online monitoring of the condition of the oil. The sensors can record the temperature and derive the oil viscosity continually.

During the business validation phase, the **subject-matter expert** would look at the initial estimates of the economic impact of the Digital Twin on the **operations and maintenance (O&M)** cost of the wind turbine or the whole wind farm. The SME would also look at how trustworthy the Digital Twin is to be able to make such O&M budgetary decisions over time.

Next, let’s look at some real numbers in this context. According to Shell, the industry spends about $8 billion per year on wind farm maintenance globally (see `https://www.shell.com/business-customers/lubricants-for-business/sector-expertise/power-industry/wind-power/true-cost-of-wind-turbine-maintenance.html`). Each gearbox failure incident costs the operator between $300K and $500K. According to our research, the cost of changing the gearbox oil for an average wind turbine is about $8,000. The gearbox may hold anywhere from 200 to 800 liters of oil. On average, the gearbox oil change of a wind turbine is done every 36 months based on scheduled maintenance.

Over the 20-year average life of a wind turbine, 6-7 oil changes are required. If the condition-based maintenance using a Digital Twin reduces it by 1 that is 5-6 oil changes over the usable lifetime of the wind turbine, which will reduce the maintenance cost by $8,000 per wind turbine just on this expense item. In addition, it can reduce the incidents of downtime due to gearbox failure. Apart from health monitoring and predictive maintenance, the Digital Twin can also contribute to the optimization of the wind turbine, leading to an increase in the energy generation level of the whole wind farm.

In this section, we looked at the business validation of the Digital Twin of the wind turbine.

## Summary

In this chapter, we demonstrated how to build a Digital Twin of an industrial asset, which in this case is a wind turbine. We made use of DTDL and Microsoft Azure Digital Twins to build the Digital Twin. We discussed how a developer can make use of the PaaS capabilities exposed via ADT. We further showed the need for an application to be built on top of ADT for the business persona. We used Power BI and XMPRO Data Stream Designer to showcase these scenarios. Then we looked at the technical and business validation of the Digital Twin for wind turbines.

In _Chapter 7, Deployment and Value Tracking_, we will start to look at considerations for the deployment of Digital Twins for wind farms at a production level.

## Questions

+   1. What is the role of DTDL in building the Digital Twin using ADT?

+   2. Why do we need to look at the sub-assemblies of the wind turbine to build the Digital Twin?

+   3. Why is predictive maintenance of gearbox critical for wind farms?

+   4. How can we store historical telemetry data from the asset in ADT?

+   5. How can we evaluate the business outcomes expected from the Digital Twin solution for wind turbines?

# 7 Deployment and Value Tracking

In Chapter 6, Building the Digital Twin Prototype, we covered the development of a wind-farm Digital Twin that consists of many wind turbines, each with its own Digital Twin. The Digital Twin application was configured using the Visual Studio integrated development environment (IDE) on the Microsoft Azure Digital Twins (ADT) service. The Visual Studio IDE approach is for programmers who want to build advanced Digital Twin applications on the ADT service. We further demonstrated this using a no-code application development platform aimed at subject-matter experts (SMEs) who want to create Digital Twin applications without coding. We further assessed the testing, technical evaluation, and business validation of our first Digital Twin.

This chapter will focus on the deployment and scaling of the solution in an operational environment. This will help to provide a structured approach, from initial experimentation with a Digital Twin to a target state where it delivers business outcomes. This is important as often, experimentations with emerging technologies such as Internet of Things (IoT) and Digital Twins are not productized, due to various complexities involved, in the full life cycle. We will continue with the wind farm and wind turbine example and demonstrate the following deployment steps:

+   - Functional testing of the Digital Twin
+   - Pilot rollout
+   - Full deployment
+   - Value proposition and tracking

In the first part of this chapter, we will assess how the Digital Twin addresses the functional requirements initially set in Chapter 6, Building the Digital Twin Prototype.

## Functional testing of the Digital Twin

Functional testing is standard practice in developing software applications, and creating Digital Twin solutions is no exception. For Digital Twins, however, we recommend functional testing of both the Digital Twin infrastructure and the Digital Twin application.

In Chapter 5, Setting Up a Digital Twin Prototype, we created models and instances of a wind farm and wind turbines as your Digital Twin prototype solution. Testing the functionality and integrity of the models and ensuring that they are correctly deployed is the first step of functional testing of a Digital Twin. Once we’ve confirmed that our Digital Twin infrastructure is working effectively, we can test the actual Digital Twin applications configured in Chapter 6, Building the Digital Twin Prototype.

Let’s look at the test framework next.

## Testing the Digital Twin infrastructure

Your wind-farm Digital Twin prototype was configured on the ADT **platform as a service** (PaaS). It is one example of an **integration PaaS** (iPaaS) for Digital Twin infrastructure. We will continue with that example as we describe the first step in the functional testing of your Digital Twin prototype.

The purpose of the infrastructure functional test is to ensure that the platform components are correctly configured. This functional test will eliminate incorrect platform configuration as a potential reason for failing the actual application functional test.

We propose the testing sequence shown in the following diagram for the infrastructure functional test:

![](img/f445915fceb4649a77b6797f1425850b_204_0.png)

Figure 7.1 – Functional testing Digital Twin infrastructure


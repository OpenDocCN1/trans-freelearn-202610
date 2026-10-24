| Digital Twin Use Case | Month 1 | Month 2 | Month 3 | Month 4 | Month 5 | Month 6 |
|-----------------------|---------|---------|---------|---------|---------|---------|
| Conveyors             | Mine 1  | Mine 2  | Mine 3  | Mine 4  | Mine 5  | Mine 6  |
| Borers                |         | Mine 1  | Mine 2  | Mine 3  | Mine 4  | Mine 5  |
| Pumps                 |         |         | Mine 1  | Mine 2  | Mine 3  | Mine 4  |
| Crushers              |         |         |         | Mine 1  | Mine 2  | Mine 3  |
| Fans                  |         |         |         |         | Mine 1  | Mine 2  |
| OEE                   |         |         |         |         |         | Mine 1  |

![](bbox=[0.3066, 0.2797, 0.666, 0.2914])

This is not the only phased implementation approach that can be used to implement your Digital Twin solutions; however, we found it to be an efficient approach for large-scale complex Digital Twin projects. An alternative approach, for example, would be to roll at all the Digital Twin use cases at a single mine site before moving to the following mine site. Organizational resources, the business case, and user adoption will influence the most suited approach to your Digital Twin project.

The phased approach is lower risk, but it will take a longer time to realize its business value. It typically has a lower impact on the organization, but long timelines can also lead to frustration. Consider the pros and cons of both these approaches and decide what is best for your Digital Twin project implementation.

Let's next focus on the rollout.

# # Wind-farm Digital Twin rollout

In _Chapter 6, Building the Digital Twin Prototype_, we created a prototype of a wind farm with wind turbines on ADT. It is a basic wind turbine asset structure, and the DTDL definition files contain a subset of all the potential properties.

For a full-scale production deployment of a wind-farm Digital Twin, we recommend reviewing the DTDL ontology for Energy Grid (`http://bit.ly/ idt-energyontology`) that is adapted from the **Common Information Model (CIM)**, a global standard for energy-grid assets management, power system operations modeling, and physical energy commodity markets (`http://bit.ly/idt-cim`). International standard _IEC 61400-25, Communications for monitoring and control of wind power plants_ (`http://bit.ly/idt-iec61400`) provides a standardized data model for information exchange for wind farms and wind turbines.
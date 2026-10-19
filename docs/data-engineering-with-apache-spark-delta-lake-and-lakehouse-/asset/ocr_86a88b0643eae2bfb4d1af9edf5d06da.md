# Exploring data pipelines

In Chapter 1, The Story of Data Engineering and Analytics, we talked about the journey of data. We equated data engineering to a vehicle that makes the journey of data possible through sharp turns and roadblocks to ultimately reach its destination as securely and timely as possible. If data engineering is a vehicle, then a data pipeline is the engine that makes the journey possible. The engine is simply a collection of components, each performing a specialized operation. Ultimately, all the parts and components working together can maneuver the vehicle in the desired direction.

In simple terms, a data pipeline is an engine that can move data through various stages of collection, curation, and aggregation to reach its analytics destination. As with the various parts and components of an engine, the data pipeline uses a series of actions to complete its work. Each action performs a specialized task (once or repeatedly) to contribute toward the end goal.

Data engineers are responsible for assembling the actions in a series of steps in the most effective fashion to ultimately address the demands of analytics. This series of steps, illustrated in the following diagram, is collectively referred to as a data pipeline:

![](bbox=[0.2, 0.5, 0.8, 0.62])

Figure 4.1 – Series of actions in a data pipeline

Important note
This is the age of options. It is the job of the data engineer to choose the most appropriate tool/service that can address the action most effectively.

# Components of a data pipeline

A data pipeline is a collection of components. Each component performs a specialized operation contributing to the overall success of the journey of data. It is commonly seen that data engineers typically pay a lot of attention to the overall development of a pipeline but commonly neglect the operational aspects. While it is important to focus on the actual building of the various components, it is equally important to focus on how these components will be assembled, deployed, and monitored in the future. To understand data pipelines a little better, let's focus next on the core components of a data pipeline.
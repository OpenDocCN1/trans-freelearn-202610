+   - **Enhanced control**: This method offers more control in comparison to the time-based method. The subsequent pipeline does not start until the predecessor has completed:

![](bbox=[0.15, 0.17, 0.85, 0.58])

Figure 9.2 – The event-based method of deploying pipelines

The event-based method is preferable since it offers greater control over the eventual success of the pipelines. For the reasons stated earlier, let’s use this approach to deploy the Electroniz pipeline. In the next section, we will start creating a master pipeline that will invoke the rest of the pipelines.

# Developing the master pipeline

To start the creation of the master pipeline, make sure you have the `electroniz_batch_ingestion_pipeline`, `electroniz_curation_pipeline`, and `electroniz_aggregation_pipeline` pipelines installed and in working condition in the Azure data factory:
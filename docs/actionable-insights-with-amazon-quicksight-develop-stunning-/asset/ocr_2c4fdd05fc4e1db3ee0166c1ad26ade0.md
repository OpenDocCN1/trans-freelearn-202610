In the following diagram, we see an example data pipeline that is using both a data
warehouse and a data lake on AWS. Data is loaded from the operational data stores into
the Amazon S3 object store in the raw layer of the data lake. Then, with a set of ETL jobs,
the data reaches a stage that can be loaded into the data warehouse for BI purposes. For
cost-effectiveness, you might not want to load all the data into the warehouse. Instead, you
might want to leave the data in the data lake but have the ability to query the data when
needed. This architecture considers the temperature of the data (how frequently the data
is accessed) to determine the best storage. **Hot** data that needs to be accessed frequently
is loaded into the data warehouse, while **colder** data remains in the data lake, a cheaper
long-term storage option.

![](bbox=[0.117, 0.304, 0.854, 0.464])

Figure 1.4 – Example data pipeline on AWS

Now that we have had an overview of the modern data architecture on AWS, let’s build a
basic modern data architecture on AWS.

# # Creating a basic modern data architecture

In this section, we will go through a hands-on example to create a basic modern data
architecture. This tutorial will use the AWS CLI and the AWS console. By the end of this
section, we will have spun up a working data lake and a data warehouse environment with
demo data loaded.

> > **Important note**
> > The resources for this tutorial might introduce charges to your AWS account.
> > Once you finish with the exercise, make sure you clean up the resources to
> > prevent incurring further charges.

# # Creating the data lake storage

In this step, we will add the data lake storage. Then we will upload a demo dataset and will
discover its schema automatically.
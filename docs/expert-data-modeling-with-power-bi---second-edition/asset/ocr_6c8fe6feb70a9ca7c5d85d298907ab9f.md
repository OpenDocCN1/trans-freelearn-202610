# Introduction to Dataflows

In the previous chapter, we learned some more advanced data modeling techniques that are more related to data warehousing: dealing with SCDs and degenerate dimensions. This chapter briefly introduces Dataflows, another available feature in the Power BI platform that can become handy in real-world scenarios. To get the best out of Dataflows, we need to pay attention to numerous technicalities that directly impact our design and implementation, such as the storage mechanism and licensing details making it virtually impossible to cover all in a single chapter. So, this chapter is designed to cover the basics and help you to build robust building blocks for your learning journeys. This chapter covers the following areas:

+   - Scenarios for using Dataflows
+   - Dataflow terminology
+   - Create Dataflows
+   - Export/import Dataflows
+   - No-code/low-code experience in Dataflows
+   - Introduction to composite models
+   - Query plans in Dataflows

# Introduction to Dataflows

In November 2018, Microsoft released Dataflows in public preview. Later on, in April 2019, Dataflows became generally available. Dataflows used to be available in **Premium** licenses only, but it is now available in Power BI Pro with some limitations. The purpose of Dataflows is to support collaborative **self-service data preparation** and **Extract, Transform, and Load (ETL)** processes in an organization by leveraging the power of Power Query in the cloud. The Dataflows technology is also known as **Power
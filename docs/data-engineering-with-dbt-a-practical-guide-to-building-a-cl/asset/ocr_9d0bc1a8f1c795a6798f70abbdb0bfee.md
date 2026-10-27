Tools such as Fivetran, Supermetrics, and Funnel are born with the marketing department in mind, while others, such as Stitch Data, approach the data movement topic from an IT department/ETL perspective. They all end up offering easy-to-configure interfaces to retrieve data from hundreds of common cloud services so that non-technical users can easily download the data from them without having to learn their APIs and with close to no configuration besides the access parameters.

+   - **ETL or data science tools:** The use of ETL-specific tools, such as Matillion, Talend, or Informatica, or data science tools, such as Alteryx or Dataiku, is an effective way to move data using already-available tools, as these tools offer a wide range of possibilities to deal with both files and data in tables. With these tools, someone has to code, or draw using a visual UI, the transformation that is then generated/executed by the tool.
+   - **Direct coding:** Directly coding with general-purpose programming languages such as Python or Java is a possible choice when there is a strong IT department and no established tool suitable for the purpose. A method still common in on-premises settings is the use of some scripting (such as Shell or Perl) coupled with common Unix-style tools, such as FTP or ad hoc built executable.

We have listed the preceding categories of tools as a modern way of dealing with data movement, but in the end, you can choose whatever way fulfills the need to move your data without errors and manual intervention.

The second dimension in the data movement is about what happens to the data during the movement. The options are as follows:

+   - **Data is transformed during the move:** This is the “old-school” way, called **ETL**, which stands for **Extract, Transform, and Load**. Transforming the data while moving from the source system to the destination was a reasonable thing to do when the amount of data was limited, databases were row-oriented, slow, on-premises and had limited memory and storage.
+   
+   Nowadays, this is not so common anymore when we consider logical transformations, but it is not uncommon that a format conversion is needed when dealing with older systems providing XML or proprietary data formats or even a complex JSON format that is not suitable for easy ingestion in the desired platform.
+   - **Data remains unaltered during the move:** This is the modern way of doing data warehousing in the cloud age, called **ELT**, which stands for **Extract, Load, and Transform**.
+   
+   The logical shift is much bigger than just swapping actions, as the new paradigm is to load the data as is on the data platform and then transform it, making full use of modern, extremely powerful databases that can muster huge amounts of memory, practically unlimited storage, and linearly scalable performance.

When working with dbt, you will start working on data after it has been moved. You can start the transformation of the data with the tool you use to move it, but our suggestion is to keep the data unaltered as much as possible and as close as possible to how it was in the source system.
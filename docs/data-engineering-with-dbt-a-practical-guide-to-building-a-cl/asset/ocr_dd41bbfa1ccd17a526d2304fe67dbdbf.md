+   3. **Data refinement**: Here is where the real data transformation happens: turning raw data into useful information. The two main parts of this transformation are applying **Master Data (MD)** to convert source system-specific data into enterprise-wide usable data and then applying **Business Rules (BR)** to transform data into usable information.
+   
+   While the MD part can follow some guidelines and patterns, the BR part is where experienced analysts and data professionals really make the difference by keeping order and creating reusable data models instead of a spaghetti web of ad-hoc transformations with no organization.

+   4. **Data delivery**: In the data delivery layer, we will have traditional reporting data marts that deliver data to reporting tools using a dimensional model, other data marts that provide data as wide tables, and other “marts” with more technical uses, providing a simple way to build and manage APIs for external tools, such as AI or ML workloads, or refined information that is taken back to the operational systems, such as updated customer profiles or full-fledged segments for use in marketing tools.

The signature feature of the architecture that we propose is the separation of the source data ingestion from the application of the business logic and the creation of the business concepts.

This core separation of concerns is the basis of our modern data platform idea, as it allows a lot of freedom in the details of the implementation and delivers the following substantial advantages:

+   - Decoupling the two most important activities, ingesting and building the business concepts, simplifies the overall development, allowing the use of the best solution for each part of the problem.
+   
+   - The extensive use of patterns allows much quicker development, keeps complexity low, makes for easy people training, and offers reliable outcomes and predictable delivery times.
+   
+   - The preservation of the source data enables proper auditing, allows for advanced source system analysis and improvement, and guarantees that we are always able to use all our data, as the original data is not changed by the application of BR that might not be valid in future and the source data is not changed in ways impossible to revert.
+   
+   - A well-organized refined layer with a simple MD Management playbook, oriented dependencies that are easy to navigate, and a few recurrent types of models with clear roles allows us to build even the most complex business concepts without creating a huge spaghetti ball of code that quickly becomes too complex to manage and maintain.
+   
+   - The practice of constantly recreating the refined and delivery layers from the stored historical source data allows you to easily change MD and BR and have it immediately deployed to all of your data, removing any state that needs maintenance.
+   
+   - The ability to remove any accumulated state from the refined and delivery layers makes the system simple and quick to change and as easy to maintain as a stateless application, gaining back developer and stakeholder confidence and a lot of human effort previously spent on data repair or fix tasks. This is in direct contrast with legacy data warehouses where changes are
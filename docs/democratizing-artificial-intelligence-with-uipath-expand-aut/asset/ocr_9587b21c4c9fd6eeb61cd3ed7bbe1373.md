# UiPath Document Understanding technical framework

Let's walk through the technical framework of UiPath Document Understanding. You will have a good overview of each component in the framework and how they relate to each other. You will be able to leverage UiPath Document Understanding in automating use cases with a document understanding activity. The following diagram shows the UiPath Document Understanding framework:

![](bbox=[0.185, 0.26, 0.85, 0.6])

Figure 3.13 – UiPath Document Understanding

The UiPath Document Understanding framework includes five components. They are as follows:

+   1. **Load Taxonomy**: The first step is to define document types and fields for extraction to create a dedicated taxonomy structure in Taxonomy Manager.
+      - **Use Case**: Extract information from travel and entertainment receipts for expense reporting.
+      - **Document type**: Receipts
+      - **Fields to be extracted**: Transaction date, Vendor, Total amount, Tax amount
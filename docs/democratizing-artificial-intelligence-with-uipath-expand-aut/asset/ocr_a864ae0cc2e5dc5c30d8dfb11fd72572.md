With the Document Understanding framework introduced, let's dive deeper into each component, starting with taxonomy.

# Introducing the taxonomy

The first step in the Document Understanding framework is creating a taxonomy.
A taxonomy is a system classification, and with Document Understanding, a taxonomy is a classification of document types. A document type is the classification of a logical type of document handled by a business process. Examples of document types can include the following:

+   - Invoices
+   - Tax forms (W-2 and 1099)
+   - Contracts
+   - Records

# The Taxonomy Manager

During the creation of the taxonomy, we will categorize the document types needed and identify the fields within each document type – this will essentially become the schema (structure) for our data. To create our taxonomy, we will use the Taxonomy Manager within UiPath Studio to create a taxonomy of fields and document types. The Taxonomy Manager can be found in the Design ribbon of UiPath Studio (Figure 6.6):

![](bbox=[0.24, 0.55, 0.7, 0.64])

Figure 6.6 – UiPath's Taxonomy Manager within the Design ribbon

> **Important Note**
> The Taxonomy Manager requires the UiPath.IntelligentOCR.Activities package installed as a dependency for your project. Once installed, a Taxonomy Manager button will appear in the Design ribbon of Studio.
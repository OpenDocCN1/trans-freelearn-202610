# Creating the taxonomy

As the first step of the *Document Understanding framework*, we need to create a taxonomy to classify our documents and build a schema of the relevant information we want to extract from our receipts. As discussed in *Chapter 6, Understanding Your Tools*, a taxonomy is a system classification, and with Document Understanding, a taxonomy is a classification of **document types**.

From our current state understanding of the process, we defined the following as necessary for this process:

+   - Vendor name
+   - Receipt date
+   - Tax amount
+   - Total amount

Thus, as we build out our taxonomy, we must include these fields. One feature of the *Document Understanding Process template* is pre-built taxonomies for common documents, such as receipts. Let's review the out-of-the-box taxonomy and determine whether it fits our use case:

+   1. Open **Taxonomy Manager** within the design ribbon of UiPath Studio (*Figure 8.17*):

![](bbox=[0.269, 0.532, 0.698, 0.612])

Figure 8.17 – UiPath Taxonomy Manager within the design ribbon

With Taxonomy Manager, note the pre-existing document types that are loaded – **Documents, Semi-Structured Documents, and Structured Documents**. Let's review the semi-structured documents.
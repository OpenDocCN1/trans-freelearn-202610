# Linking Datasets

However, the vision was rather abstract and difficult to many people. Several of the concepts relied on concepts such as ontologies and reasoning, which can become very complex rapidly. *Tim Berners-Lee*, inventor of the Web and one of the creators of the Semantic Web vision, realized this and launched the Linked Data principles (http://www.w3.org/DesignIssues/LinkedData.html). These principles shifted the focus of the Semantic Web to the creation of data that was interlinked with other datasets. The principles are as follows:

+   1. Use URIs as names for things
+   2. Use HTTP URIs so that people can look up those names
+   3. When someone looks up a URI, provide useful information using the standards (RDF, SPARQL)
+   4. Include links to other URIs so that they can discover more things

So, the first principle asks to use unambiguous identifiers for your concepts and the second principle asks specifically for HTTP URIs, which are thus URLs. These principles have been used in the datasets we will use for reconciliation. The third principle is concerned with how your data is published. In fact, the RDF extension allows us to export our dataset as RDF, so that's covered as well. The fourth principle is about linking to other datasets, which is exactly what reconciliation will do for us.

Without further ado, let's start reconciliation. Like in *Recipe 1 – reconciling values with Freebase*, go to the **Categories** dropdown and navigate to **Reconcile | Start reconciling...**, which will bring up the reconciliation dialog on your screen. This time, however, select the **LCSH reconciliation service** from the left-hand side which you installed in the previous recipe. OpenRefine will now try out some of the cell values to see if it can determine its type. After a few seconds, it suggests that the values have type skos:Concept. This is indeed true, as all **Category** values are in fact concepts. You can leave all settings as they are and click on **Start Reconciling**.

![](bbox=[0.241, 0.626, 0.764, 0.68])

When the reconciliation is done, you will notice three things. First, OpenRefine has created the two facets that help us find matched and unmatched rows. Second, you see a green bar on top of the **Category** column, which indicates how many of the cell values have been reconciled. Third, the cell values are displayed differently; some of them are in blue and others have new options.
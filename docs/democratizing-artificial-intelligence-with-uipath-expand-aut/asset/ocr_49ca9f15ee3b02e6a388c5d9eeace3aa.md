Within the Classify Document Scope activity, notice the Input and Output parameters:

+   - **Input:**
+     - **Document Path**: The full path to our target document
+     - **Taxonomy**: The taxonomy returned from the Taxonomy Manager
+     - **Document Text**: The document text returned from digitization
+     - **Document Object Model**: The DOM returned from digitization
+   - **Output:**
+     - **Classification Results**: The results stored in an IReadOnlyList<ClassificationResult> object, containing results such as classification confidence and OCR confidence (Figure 6.20):

![](bbox=[0.322, 0.378, 0.642, 0.874])

Figure 6.20 – An example of the classification results
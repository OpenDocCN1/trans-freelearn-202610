We will also leave the **DegreeOfParallelism** property to 1. By default, the property is set to 1. If we were to set it to -1, UiPath uses the number of cores on the machine to process pages in parallel. Any other positive value uses that specific number of logical processors to process pages.

Additional information on the Digitize Document activity and when to use OCR can be found at https://docs.uipath.com/document-understanding/docs/digitization-overview.

We have now loaded the taxonomy and digitized our invoice document. In the next section, we'll learn about classifying the invoice document.

# Introducing classification

After digitizing our input data into machine-readable text, our automation can now interpret the text within our input document. However, while our automation can read the text, it still does not know what type of document it read; thus, the next step is to have the automation classify the text. Classification in the Document Understanding framework is using digitized input data to identify what type of document is input.

Document classification becomes extremely important when an input document contains multiple document types (Figure 6.18):

![](bbox=[0.25, 0.52, 0.7, 0.82])

Figure 6.18 – Classifying documents from an input
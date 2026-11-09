# 6

# Cleaning Data in PDF Files

In the last chapter, we discovered different ways of separating the data we want from the data we do not want. We imagined that the data cleaning process was a little like making chicken stock, in which our goal was to keep the broth but strain out the bones. But what happens if the data we want is not so easily distinguishable from the data we do not want?

Consider a fine, older wine with considerable sediment. At first glance, we might not be able to see the sediment suspended in the liquid. But after the wine spends some time in a decanter, the sediment falls to the bottom, and we are able to pour out a cleaner, more aromatic wine. A simple strainer would not have been able to separate the wine from the sediment in this case—a special-purpose tool would have been needed.

In this chapter, we will experiment with several data decanters to extract all the good stuff hidden inside inscrutable PDF files. We will explore the following topics:

+   - What PDF files are for and why it is difficult to extract data from them
+   - How to copy and paste from PDF files, and what to do when this does not work
+   - How to shrink a PDF file by saving only the pages that we need
+   - How to extract text and numbers from a PDF file using the tools inside a Python package called **pdfMiner**
+   - How to extract tabular data from within a PDF file using a browser-based Java application called **Tabula**
+   - How to use the full, paid version of Adobe Acrobat to extract a table of data
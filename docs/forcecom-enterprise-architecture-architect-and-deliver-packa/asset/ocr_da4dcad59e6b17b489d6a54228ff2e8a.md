Profiles and Permission Sets also reference Visualforce pages; users must be given access to a page before they can use it from these areas.

# # Lightning Components

The following is a list of platform areas that accept Lightning Components:

+   - Record Pages
+   - Home Pages
+   - Lightning Tabs and Pages
+   - Lightning Component Actions
+   - Lightning Experience Utility Bar
+   - Lightning App Builder
+   - Lightning Community and Community Builder
+   - Lightning Out for Visualforce
+   - Lightning Out for Any App

In Chapter 6, Application Domain Layer, we explored adding the Verify Compliance Lightning Component to Record Pages. In the next chapter, we will build some new Lightning Components to allow us to take a closer look at some of the other areas in the preceding list. You should also refer to the Lightning Developer Guide for full details.

# # Generating downloadable content

The contentType attribute allows you to control how the browser interprets the page output. With this attribute, you can, for example, output some CSV, JSON, or XML content. Using a controller binding, this can be dynamically generated output. This can be useful to generate content to download. The following changes have been made to the FormulaForce application and are included in the code for this chapter:

+   - Added a new method, generateSummaryAsCSV, to RaceService
+   - Added a new getCSVContent method to RaceSummaryController
+   - Added a new racesummary.aspx Visualforce page
+   - Added a new Custom Button, Download Summary, to the Race layout
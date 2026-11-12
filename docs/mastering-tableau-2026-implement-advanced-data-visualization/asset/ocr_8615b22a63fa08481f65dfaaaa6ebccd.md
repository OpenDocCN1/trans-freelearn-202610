# # Introducing LODs

Tableau's default is to show measures in a view based on the dimensions also present in the view. If you have a dashboard with Sales data and dimensions like State and City, and you drag the State and Sales data onto the view, the Sales data will be divided by State, showing you Sales per State. If you want to divide the Sales data further into smaller chunks, you might add the City field, resulting in Sales data per City, per State. LODs can manipulate this default behavior.

After completing this chapter, you will be able to divide or partition measures by dimensions that are not in the view and show measures using fewer dimensions than are visible in the view.

To do this, we will build and use two **playgrounds**. Delivering reports as required by one's job duties may lead to a thorough knowledge of a limited set of capabilities; that is, a deep but narrow understanding. It can be difficult to set aside time (and justify that time) to explore the capabilities of Tableau that, on the surface, may seem to have no direct correlation to job duties. Playground environments can help overcome any difficulties and objections by providing efficient avenues of exploration. In this chapter, we'll build two playground environments specifically for LODs, to help make the task of deep and broad understanding easier by providing an efficient avenue for exploration and understanding. You can always come back to the workbook accompanying this chapter and test functionality related to LODs.

# # Working with FIXED and EXCLUDE operations

The first playground we will build is designed to explore two of the three LOD functions: FIXED and EXCLUDE. We will use a set of parameters and associated calculated fields to efficiently explore how these functions work. Before we dive into the functions themselves, let's walk through the workbook setup.

Before we get into the technical setup, let me explain what all these parameters and calculations actually do. This will make the following steps much clearer.

| Component | What It Controls | How It Works |
| :--- | :--- | :--- |
| 1st Dim, 2nd Dim, 3rd Dim, 4th Dim (parameters) | Which dimension appears in each of the four slots on the Rows shelf? | Each parameter has a list of dimensions (Category, Region, Segment, etc.). When you select a dimension from the dropdown, the parameter passes a number (1, 2, 3..) to a corresponding calculated field. |
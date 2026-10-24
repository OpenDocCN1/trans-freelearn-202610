There are a couple of constraints to keep in mind:

+   - Scenarios are limited to a single worksheet, meaning all inputs in the scenario must be on the same worksheet.

+   - Each scenario is limited to 32 inputs. However, you can apply scenarios sequentially to go beyond the 32-item input.

+   - There is no limit to the number of scenarios that you can store on a given worksheet.

First, let’s set up a calculation that serves as a vehicle for understanding scenarios. Following this, we’ll create multiple scenarios that we’ll swap through the calculation, and then use comparison reports to see the results of our scenarios side by side. Additionally, you’ll see how to transfer scenarios between worksheets and between workbooks.

# ## Setting the scene for a scenario

Let’s say that you’re contemplating purchasing two houses at two different price levels, and you want to compare **15-year** financing with **30-year** financing. For such a simple calculation, you’d probably set the calculations up in four columns. Instead, we’ll use **Scenario Manager** to compare the results so that you can extrapolate how this can help streamline larger worksheets. The **Scenario Manager** worksheet in this chapter’s example workbook has the inputs in place for our first scenario, as shown in Figure 6.2:

![](bbox=[0.131, 0.518, 0.852, 0.66])

Figure 6.2 – The first of four scenarios

+   - Cell C2 contains a **Description** input, which, in this case, is 678 Oak Street – 15 Year Loan

> > **Tip**
> > If your scenario has fewer than 32 inputs, store a description of the scenario in a worksheet cell that you can include in the scenario. Doing so will help you stay aware of which scenario is presently applied to the worksheet. It’s easy to lose track when swapping multiple scenarios into a worksheet.
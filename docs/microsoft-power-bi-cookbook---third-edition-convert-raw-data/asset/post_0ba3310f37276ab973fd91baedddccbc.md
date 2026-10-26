
32. From the View tab, check the options for Gridlines and Snap to grid in the Page options section of the ribbon.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_245_0.png)

Figure 4.48: Page options in the View ribbon

33. Moving the group of visuals or resizing visuals now snaps to the grid points.

34. Deactivate Gridlines and Snap to grid.

35. Right-click the Scratch page tab and choose Hide.

36. Repeat the previous step for the Product Tooltip and Customer Drill through pages.

37. Use the + tab to add a page called Information.

38. On the Information page, use the Insert tab of the ribbon to create a Text box similar to the image in Figure 4.49.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_245_1.png)

Figure 4.49: Information page of report with active mailto link

39. In this case, Helpdesk was selected and made into an active mailto: link.

40. From the View tab, choose Lock objects in the ribbon.

41. Select the Europe page and save your work.

42. From the Home tab, choose Publish in the ribbon to publish the report to the Power BI service.

## How it works

Once a report is ready to be published, it is a good idea to use the Lock objects setting to prevent unintended movement or resizing of visual elements. It is important to note, however, that Lock objects only affects the movement or resizing of visuals by mouse and keyboard. The Height, Width, Horizontal, and Vertical settings in the General sub-pane of the Format pane within the Properties format card can still be used to reposition and resize visuals.

Before publishing a report, you should hide or remove pages used during development, such as the Scratch page. Pages used for report page tooltips should also be hidden. Drill through pages may or may not be hidden, depending on their utility in the report. Hidden pages will not be shown to the report reader when viewing the report in the Power BI service.

It is often a good idea to include an **Information** page. Information pages can be used to describe the data sources used in the report, helpful tips for navigating the report, and even support information.

Positioning and alignment are key qualities in a professional report. As demonstrated in this recipe, Power BI provides many options for ensuring that visuals are aligned and sized in symmetrical ways. The automatic alignment guides are present whenever moving or resizing a visual element on a page.

These alignment guides appear relative to the edges of other visual elements on the page, as well as *common edges*, such as the center of the page. Alternatively, the **Align** options present in the **Format** ribbon offer a way to mass-align many visuals simultaneously. In addition, the distribution options present in the **Align** options are a great way to ensure even spacing between similar visuals. The **Gridlines** and **Snap to grid** features available in the **View** ribbon can also assist you in getting the visuals properly oriented on the page.

Shapes can better organize visuals and can improve report aesthetics relative to the borders and backgrounds of each visual. Shapes are commonly used for report title backgrounds, and they can also customize the plot area of charts, such as by splitting a scatter chart into four squares, with each square having a distinct background color.

## There’s more…

With a report and semantic model published to the Power BI service, we can now configure Q&A for the report. To see how this works, do the following:

- 1. View the Germany page in the Power BI service. Hover over one of the KPI visuals and use the pin icon to pin the visual to a dashboard.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_246_0.png)

- 2. Select **New dashboard**, enter a name for the dashboard, and click the **Pin** button.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_247_0.png)

Figure 4.51: Pin to dashboard dialog

Q&A natural language queries are submitted from dashboards against the underlying semantic models. In order for Q&A queries to access a semantic model and its dependent reports, at least one tile of the given Q&A dashboard must be dependent on this semantic model.

- 3. Navigate to the dashboard and note the **Ask a question about your data** prompt, then click the prompt.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_247_1.png)

Figure 4.52: Dashboard Q&A

- 4. Test Q&A using your own questions, or one of the suggested questions; for example: *top Germany resellers by reseller net sales.*

- 5. Click on the gear icon in the top-right corner of the Power BI service and select **Power BI settings**.

- 6. Select the **Semantic models** tab and the specific semantic model to be queried via Q&A.

- 7. Expand the Featured Q&A questions section and add questions; after clicking the Apply button, these questions will now appear as suggestions when using Q&A from the dashboard.

Q&A

Featured Q&A questions

Featured questions are shown as suggestions for this semantic model in Q&A.

Europe Internet Net Sales by calendar yr - mo before 5/1/2017

Germany Internet Net Sales by Product Name

France reseller net sales by calendar yr - mo in 2017 as line

Fright Trend

Add a question

Apply

Discard

Figure 4.53: Featured Q&A questions option in the semantic model settings

Note that Q&A can also be managed within Power BI Desktop by going to the Modeling tab and selecting Q&A setup. Here, synonyms can be defined, as well as suggested or featured questions. In addition, questions asked by users in the service can be reviewed.

## See also

- - Tips for asking questions in Power BI Q&A: http://bit.ly/2pRIZdQ

## Designing Mobile Layouts

Power BI reports can be optimized for consumption on mobile devices via the Mobile layout view in Power BI Desktop. This layout allows users accessing reports through the Power BI mobile applications to more easily view and interact with the most important content of these reports on iOS, Android, or Windows mobile devices. Given the importance of the mobile experience and the unique design considerations for reports with multiple pages, optimizing Power BI reports for mobile access is essential.

In this recipe, the Europe and United Kingdom report pages of the example report provided in the first recipe of this chapter are configured with the mobile layout. Additional details for optimizing Power BI dashboards are included in the There’s more… section.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Open the Formatting Reports for Publication.pbix file and save the file as Designing Mobile Layouts.pbix.

- 2. Switch to the Scratch page and use Ctrl + A and then Delete to remove any visuals.

## How to design mobile layouts

To implement this recipe, use the following steps:

- 1. Select the Europe page.
- 2. From the View tab, select Mobile layout in the ribbon.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_249_0.png)

Figure 4.54: Mobile layout

3. Populate the Mobile layout view: click and drag the visualizations to the desired position in the device grid. Note that the Snap to grid feature in the View tab is especially helpful here.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_249_1.png)

Figure 4.55: Populated mobile layout

4. Switch to the United Kingdom page and also configure Mobile layout.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_250_0.png)

Figure 4.56: United Kingdom Sales and Margin mobile layout

- 5. Exit the mobile layout by clicking on Mobile layout in the ribbon of the View tab.
- 6. Save your work.
- 7. Publish the report to the Power BI service.
- 8. Test the mobile views using the Power BI app on your phone.

## How it works

The utilization of Power BI reports often varies significantly across devices. For example, a report page with multiple slicers and table or Matrix visuals may be appropriate for a detailed, interactive experience on a laptop but may not lend itself well to mobile consumption. In many scenarios, the user prefers simple, easy access to only a few high-level visuals, such as Cards or KPIs on their mobile device, rather than a sub-optimal representation of all the visuals included on the page.

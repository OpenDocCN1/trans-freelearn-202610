
- 6. Where you see the **Opportunities** dataset, click on the + button to add another filter from this dataset, and choose **Billing Country**.
- 7. Add a third filter for **Opportunity Type**. It does not yet appear on the dashboard because the **Filters Per Row** option is set to only 2.
- 8. Come out of the **Manage Global Filters** edit screen and click on the widget, then increase **Filters Per Row** to 3.
- 9. Edit the filter titles by clicking on the pencil icon while in the **Dashboard Global Filters** panel. The values should be **Country**, **Industry**, and **Type**.
- 10. Drag the handles on the **Filter** widget and increase its size, leaving room for one widget on the right.
- 11. Edit the format of the **Filter** widget to suit your personal preference. You will find an example in Figure 8.6.
- 12. You're going to need more room to fit the **Toggle** widget in, so resize and move the highlights panel widgets to make space.
- 13. Drag a **Toggle** widget onto the container, to the right of the new **Filter** widget.
- 14. Click on **Toggle** and select **Account Type**.
- 15. Change **Measure Field** to -- none --.
- 16. Change the **Toggle** widget border to **White**, width 2, radius 4.
- 17. Increase the **Text Size** setting to 16.
- 18. Change the **Text Color** setting to a light blue.
- 19. Click on **Save** to save the dashboard.

Go into **Preview** mode and see what happens when you select various filters and use the toggle. It is very easy to drill into your dashboard using these features, as well as faceting by clicking on dimensions in charts. Your dashboard should now look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_217_0.png)

Figure 8.6 – Sales dashboard with filters and toggle added

## Adding Image, Date, List, and Range widgets to the sales dashboard

You will now add Image, Date, List, and Range widgets by following these steps:

- 1. Add a logo to your dashboard by dragging an Image widget onto the canvas and clicking on Image.
- 2. Upload a suitable image, such as a fake business logo.
- 3. Remove the widget border.
- 4. Move the dashboard title to the bottom of the container and modify the font size as required.
- 5. Shrink the Image widget and place it above the dashboard title. Change Image Scale to Fit Height.
- 6. Drag a Date widget onto the canvas, under the toggle, then click on Date, and choose Close Date.
- 7. Change the Title field to Close Date.
- 8. Resize the **Date** widget to half the width of the space, make it taller, and move to the right edge of the canvas.
- 9. Drag a **List** widget onto the canvas, click on **List**, and select **Segment**.
- 10. Resize the **List** widget to match the **Date** widget.
- 11. Change the **Title** field to **Segment**.
- 12. Click on the **QUERY** tab on the right and change **Selection Type** to **Multiple Selection**.
- 13. Drag a **Range** widget onto the canvas, under the two widgets just added.
- 14. Stretch the **Range** widget to fill the available space.
- 15. Click on **Range** and choose **Amount**.
- 16. Click on **Save** to save the dashboard.

Add two more charts to your dashboard that break down **Pipeline by Industry** and **Top Ten Account Owners** (use **Query Limit** for this). Format as you see fit. Save the dashboard; it should look something like this:

![](img/06379e43783b9ddfee45e9da5192efe1_218_0.png)

Figure 8.7 – Sales dashboard with the first page completed

## The devil is in the details

In this section, you will learn about the detailed options available for editing your dashboard.

While in your new dashboard, check out the query panel and the options available there. You can see an overview of this in the following screenshot:

![](img/06379e43783b9ddfee45e9da5192efe1_219_0.png)

You have five actions available for each query from the panel, outlined as follows:

- **Edit**: Opens the query for editing and formatting.
- **Clone**: Creates a copy of the query and opens it in **Edit** mode.
- **Properties**: Gives access to query configuration options such as faceting, selection type, and initial selections. More detail will follow in this section.
- **Delete**: Deletes the query from the dashboard.
- **Preview**: Opens a preview window in a values table format.

Regarding the query properties, here is some more detail:

- **Display label**: The user-facing query label can be edited here.
- **Apply global filters**: Uncheck if you do not want the dashboard global filters to apply to this query and the widgets that it drives.
- **Faceting**: You have four options here—**All**, **Include**, **Exclude**, and **None**. The default value of **All** means that selecting values on other widgets will apply those faceting values to this query and the widgets that it drives, as in standard dashboard behavior. This default behavior can be modified by selecting **Include** (choose which specific queries to include as faceting values for this query), **Exclude** (choose which specific queries to exclude as faceting values for this query), or **None** (no faceting will apply to this query).
- **Selection Type**: Choose which type of selection will apply when a user clicks on a widget selection: **None**, **Single Selection**, **Multiple Selection**, **Single Selection (required)**, and **Multiple Selection (required)**.
- **Broadcast selections as facets**: You can turn off the default faceting behavior by unchecking this, meaning that making selections on this query will not facet other queries.
- **Initial selections**: Shows the initial selections that have been configured for this dashboard.

These query actions and properties give you great flexibility in dashboard design. Now, let's look at the powerful functionality of dashboard pages.

## Understanding the top-down design with pages

From a business user perspective, it is inconvenient to have to constantly move from dashboard to dashboard when investigating and analyzing data. Also, filters applied to one TCRM dashboard are not carried over to another dashboard. A versatile, top-down design for dashboards using TCRM pages overcomes these obstacles and provides a simple, flexible interface for slicing and dicing data.

The best way to understand the pages functionality is to apply it to a dashboard, so edit your **My First Dashboard** dashboard, and follow these steps:

- 1. Open the **user interface (UI)** for TCRM pages by clicking on the drop-down arrow next to **Untitled**, to the right of **Default**.
- 2. Click on **Rename** and rename the existing page **Sales**.
- 3. You could click on + to add a new blank page, but we want to carry across the highlights panel, so select the drop-down arrow and then select Clone. A new page appears, named Clone of Sales.
- 4. Rename the new page Marketing.

Here is the UI for TCRM pages:

![](img/06379e43783b9ddfee45e9da5192efe1_221_0.png)

Figure 8.9 – The TCRM pages UI

Now, go ahead and build out the new page, as follows:

- 1. Delete every widget except for the highlights panel; you can select multiple widgets by holding down Cmd (Mac) or Ctrl (Windows) while selecting each with a mouse click. Don’t worry—they won’t be removed from the Sales page!
- 2. Remove widgets on the highlights panel except for the logo and title. You will be asked: Delete widget and global filter? Click on **Delete Widget Only**, as you do not want to delete the global filters.
- 3. Click on the title text widget. Notice on the Widget panel on the right that there is a message: This widget exists on multiple pages. Changes here apply to instances on other pages, unless you unlink it. This is important—if you modify this widget, it will modify all other copies of this widget.
- 4. Click on **Unlink**.
- 5. Change the title to Marketing Insights.
- 6. Resize the title text widget so that the text appears correctly.
- 7. Save your dashboard and close it.

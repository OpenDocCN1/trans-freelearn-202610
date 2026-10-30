
This is what your new dashboard should look like:

![](img/06379e43783b9ddfee45e9da5192efe1_237_0.png)

Figure 9.5 – The finished home page for Clever Pages

The next stage is to clone this page and use it to create an expansion page for the hero chart, using these steps:

1. Rename the existing page Home.
2. Clone this page and rename the new page Expansion.
3. Unlink the link widget that appears as the title Open Pipeline by Close Date and Stage.
4. Edit this link on the Home page to point to the Expansion page.
5. Go to the Expansion page and edit the layout so that the Open Pipeline by Close Date and Stage hero chart is highly prominent. You can remove the pie chart.
6. Resize the link widget acting as a title to make room for a second link widget on the right.
7. Point the title link widget to the **Home** page.
8. Add a second link widget on the right, above the hero chart, that points back to the **Home** page. The link text should be Home.
9. Click **Save** to save your dashboard.

The finished result should look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_238_0.png)

Figure 9.6 – The Expansion page on the Clever Pages dashboard

Test the navigation—from the **Home** page, clicking on the title of the hero chart will expand that chart and allow detailed analysis; clicking on the **Home** link takes you back, as does clicking on the hero chart title. Nice! This functionality is very useful in dashboard design. For example, it can be used to zoom in and out on charts for further analysis, reveal greater detail on a hero chart, or facilitate data segmentation.

Let's add a different type of animation with pages where drill-down can be enabled with just one click. Open the **Clever Pages** dashboard and follow these steps:

1. Clone the **Home** page and rename this new page **Industry and Lead Source**.
2. Remove the link widget with the text **Open Pipeline by Close Date and Stage**.
3. Remove all charts except **Deals Won by Industry and Lead Source**.
4. Move the heat map chart to the top left of the page.
5. Clone the heat map and edit the copy as **Pipeline by Industry and Lead Source**; change the filters to suit.
6. Add a stacked bar chart titled **Breakdown of Deals Won and Lost by Industry**.
7. Add a bar chart titled **Open Pipeline by Lead Source**.
8. Remove the global filters (widget only).
9. Add list widgets for **Lead Source and Industry** to the highlights panel.
10. Remove the toggle widget in the highlights panel.
11. Add a link to the highlights panel with the text **Go Back**, pointing to the **Home** page.
12. Go to the **Home** page and unlink the **Deals Won by Industry and Lead Source** chart.
13. Replace the title of this chart with a link that has the same text and points to the **Industry and Lead Source** page.
14. Unlink the page title in the highlights panel and change the text to **Sales Breakdown**.
15. Test the navigation. Clicking on the title for **Deals Won by Industry and Lead Source** now takes you to a page that slices and dices your sales data by industry and lead source. Cool!
16. Click **Save** to save your dashboard.

The result will look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_240_0.png)

Figure 9.7 – Sales breakdown by industry and lead source

Another way of using page animation to enhance the user experience (UX) is to use pages to show or hide a detail panel. Open your Clever Pages dashboard and follow these steps:

1. Go to the Expansion page and clone it.
2. Rename the page Expansion Detail.
3. Remove the heat map chart.
4. Move the combo chart to the left and make it fit the width of the hero chart.
5. Add a container widget that runs from top to bottom; see Figure 9.8.
6. Change the container widget background color to match the highlights panel.
7. Unlink the link widget that says Home and change its color to white. Move this to the top right of the container.
8. Add another link widget to the left of the **Home** link widget. **Text** should be **Back**; **Link To** will be **Page in layout** and **Page** is **Expansion**.
9. Go to the **Expansion** page and unlink the **Open Pipeline by Close Date and Stage** link widget.
10. Edit this widget so that it now links to the **Expansion Detail** page.
11. Go back to the **Expansion Detail** page and add a timeline chart titled **Trend of Deals Won**.
12. Add a bar chart titled **Pipeline by Stage**.
13. Add a donut chart titled **Pipeline by Product Family**.
14. Click **Save** to save your dashboard.

The **Expansion Detail** page should look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_241_0.png)

Figure 9.8 – The Expansion Detail page

If you test the page navigation by clicking on the hero chart title, you see that the pages enable drill-down, as follows:

- Home page
- Focus on Open Pipeline by Close Date and Stage
- Details for Open Pipeline by Close Date and Stage
- Back to Focus on Open Pipeline by Close Date and Stage
- Back to Home page

As our last example of intelligent design with TCRM pages, let's look at embedding a navigation link on an image. Follow these steps:

1. Edit the Clever Pages dashboard.
2. Go to the Expansion Detail page.
3. Reduce the width of the text widget that says Open Pipeline by Close Date and Stage while keeping it centered above the chart widget.
4. Add a container widget to the page, shrink it in size, and move it to the left of the text widget that you just resized.
5. Download a transparent image from the internet.
6. Add this image to the new container as a background image.
7. Click Save to save your dashboard.

This is what your progress should look like so far:

![](img/06379e43783b9ddfee45e9da5192efe1_242_0.png)

Figure 9.9 – Formatting the container with a background image

Time to add a link to your container! Follow these steps:

1. Add a link to your page and shrink it so that it will fit inside your new container.
2. Move the link onto the container with the **Home** icon.
3. Remove all text from the link.
4. Make sure that the link widget has a transparent background.
5. Set the link to navigate to the **Home** page.
6. Click **Save** to save your dashboard.

Test your dashboard—the home icon will now take you back to the **Home** page.

Let's now have a look at an introduction to SAQL—your first foray into code in TCRM!

## Beginning with code – an introduction to SAQL

This section will introduce you to coding in TCRM using SAQL.

## Definitions

We'll now look at detailed definitions regarding what SAQL is and when it can be used.

## What is SAQL?

**SAQL** is a runtime query language that enables ad hoc analysis of datasets. A SAQL script consists of a sequence of statements that are made up of keywords (such as filter, group, and order), **identifiers (IDs)**, literals, or special characters. It is **JavaScript Object Notation (JSON)**-based and **Apache Pig Latin (PIGQL)**-familiar. A SAQL query loads an input dataset, operates on it, and outputs the results.

Here is a SAQL example:

```
q = load "My_Dataset";
q = group q by 'Division_Name';
q = foreach q generate 'Division_Name' as 'Division_Name', sum('Amount')/sum('Quantity') as 'sum_Amt/Qty'; q = order q by 'sum_Amt/Qty' desc;
```

What you see in the preceding snippet are basic SAQL elements, as follows:

- Load statement to load data
- Stream ID—in this case, "q"
- Statements using various SAQL functions and keywords

## When do you use SAQL?

You may use SAQL when you need to do custom calculations, advanced data manipulations on the fly, co-grouping (joining) data from different datasets, and top/bottom lists, in conjunction with binding for dynamic values on charts, and so on.

## Which syntax is used for SAQL?

Here is some basic SAQL syntax:

- Keywords and functions are written in lowercase and they are case-sensitive.
- **Application programming interface (API)** names being called from the dataset should be enclosed in single quotes, like this: 'Division_Name'.
- API names are case-sensitive.
- Strings must be enclosed in double quotes, such as "Open".

## Where do you edit SAQL?

Every step of a lens is SAQL-based so that it can be edited and/or custom SAQL can be added. These can be viewed in JSON editor mode or copied to a JSON editor of your choice.

Follow these steps to see how to view and edit SAQL:

1. Create a new lens from the **Opportunities** dataset.
2. Add filters to the lens as you wish.
3. Add groupings to the lens as you wish.
4. Select two measures.
5. Change to query mode by clicking on the **Query Mode** button at the top right of the chart in your lens.

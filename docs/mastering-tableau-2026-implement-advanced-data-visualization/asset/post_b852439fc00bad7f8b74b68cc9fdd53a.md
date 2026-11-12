
![](img/66cf6b89349fda3581a4f196cb3c98a4_362_0.png)

Figure 8.44: Custom Diverging

Your final worksheet should look like the following:

![](img/66cf6b89349fda3581a4f196cb3c98a4_362_1.png)

Figure 8.45: Final dartboard polygon

The final dashboard shows a dartboard with different colors. The gray color means that this area has not been hit during the game, whereas the redder the color of a field is, the more often it has been hit. By using the parameter, you can compare Matthew's score with David's, or you can show the total hits, like in the preceding screenshot.

You may have noticed that the curved sections of the polygons in this exercise aren't quite perfect. Look, for example, at the triple ring and double ring areas surrounding the bullseye; you can see that the area is not 100% even. That's simply the nature of working with points on a Cartesian grid in Tableau. This means that Tableau is based on a grid structure, so every curve we draw will still be many small lines connecting dots on a grid layout. The more dots you have, the shorter the lines between the dots, the more likely you will draw curves evenly. But if you followed along with the bookshelf exercise, you can see that a lot of effort has to go into such a background image. Nevertheless, I believe that polygons are absolutely worth it. Here we used fun exercises that might not add value to your day-to-day job, but think about a picture-perfect landing page that you designed in a different program. By using polygons, you can make the design elements clickable and interactive.

The next topic is a little bit different from the rest of this chapter. Have you heard about Tableau Exchange yet? Tableau Exchange combines three products: extensions, accelerators, and connectors. Interested? Continue with the next section!

## Tableau Exchange

To access Tableau Exchange, go to this website: https://exchange.tableau.com.

![](img/66cf6b89349fda3581a4f196cb3c98a4_363_0.png)

Figure 8.46: Tableau Exchange

Accelerators and extensions can also be accessed from within Tableau Server and/or Tableau Desktop. Let us dive right into Tableau Exchange and see what it has to offer!

## Tableau extensions

Extensions are the oldest product and are accessible from the Object pane on your Tableau dashboard. Basically, extensions allow you to use third-party tools directly in Tableau. Some selected extensions are available at https://extensiongallery.tableau.com, and the list is always growing.

Please be aware that Tableau does not provide support or guarantees/security measures for extensions. Those APIs are built by external parties and should be treated as such. Check with your IT security team first if you want to use them at work.

Let's have a look at one example from Infotopics – a Sankey chart extension.

## Using an extension (dashboard or viz)

In the associated workbook, create a new sheet called Show me More and select the Coffee Chain dataset. Now reproduce the following worksheet layout by adding Type to Columns, Product Type to Rows, and Profit to the Text shelf:

![](img/66cf6b89349fda3581a4f196cb3c98a4_364_0.png)

Figure 8.47: Show me More

We want to utilize a Sankey chart for this worksheet, which is a kind of flow chart that shows, based on the size of flow-lines, how much of a given measure and a given dimension flows into a second given dimension. In this case, we want to know how much Profit from Regular type products comes from Coffee, Espresso, Herbal Tea, or Tea, and how much Profit can be associated with the Decaf types.

Let's say you know how to build a Sankey chart using calculated fields, but it is time-consuming. Instead, there is an extension to do so. The following are the steps required to use the extension and ultimately save us time when building a Sankey chart:

- 1. In the associated workbook, open a Dashboard tab and call it Show me More DB
- 2. Drag the Show me More DB sheet onto the dashboard. Now also drag the Extension object onto the dashboard

![](img/66cf6b89349fda3581a4f196cb3c98a4_365_0.png)

Figure 8.48: Extension

- 3. From Tableau 2021.1, you will see the following pop-up window

## Viz Extensions

Use custom visualizations in your worksheets. Learn more

- Sort by
- Number of Downloads

- Features
- Built by Tableau + Salesforce
- Free
- Sandboxed
- Works with Tableau Public
- Tableau trusted partner-built

## All Viz Extensions

- Sankey (by Tableau): Sankey Diagram is used to show the flow of data between multiple categories.
- Tableau Table (by Tableau): Create a rich table experience with conditional formatting directly in Tableau.
- Radial (by Tableau): Displays data in a circular format in which the total of all segment values add up to 100%.
- Donut (by LaDataViz): Create a Donut chart without
- Tree Diagram (by LaDataViz): Create beautiful radial.
- Gauge (by LaDataViz): Create a fully customizable

Figure 8.49: Extension Gallery

> Note
> Tableau distinguishes between Dashboard and Viz Extensions; both work with the steps explained in this section.

- 4. In the Extension Gallery search for the **Show Me More 2.0** extension. Click on it and select **Add to Dashboard** on the next screen

![](img/66cf6b89349fda3581a4f196cb3c98a4_367_0.png)

## Description

Figure 8.50: Show Me More 2

- 5. Then, select **Get started** and choose the **Sankey Diagram** visualization

![](img/66cf6b89349fda3581a4f196cb3c98a4_368_0.png)

Figure 8.51: Show Me More II

- 6. After selecting the Sankey diagram, configure the variables as follows. There are many more options, as partly shown in Figure 8.52; feel free to test them out:

![](img/66cf6b89349fda3581a4f196cb3c98a4_368_1.png)

Figure 8.52: Sankey diagram settings

- 7. Hit the **Ok** button, and voilà, you have your Sankey diagram:

## Show me More

| Product Type | Decaf | Regular |
|---|---|---|
| Coffee | 13,989 | 60,694 |
| Espresso | 29,502 | 39,118 |
| Herbal Tea | 63,254 | |
| Tea | | 52,986 |

![](img/66cf6b89349fda3581a4f196cb3c98a4_369_0.png)

Figure 8.53: Sankey diagram

The Sankey chart was just used as an example; Tableau added the Sankey Chart as a native visualization natively. I would highly recommend you have a look at the extensions library and find some more advanced visualizations to experiment with. There are many extensions for multiple different use cases; some are free, some have to be paid for, but you can also build your own. It is worth taking a look!

## Accelerators

This feature works on a more basic level: how to design, structure, and put together a dashboard. Accelerators are pre-defined dashboards from different sectors and with different focus points. Currently, there are over 100 available in multiple languages, catering to different departments, industries, and enterprise applications. Instead of starting from scratch, you can use an **Accelerator** dashboard by clicking on the links on your Tableau Desktop landing page or Tableau Server:

![](img/66cf6b89349fda3581a4f196cb3c98a4_369_1.png)

Figure 8.54: Tableau landing page

In Figure 8.54, the two dashboards on the left always show on the Tableau Desktop landing page, and you are familiar with those already: the Superstore and the World Indicators datasets. Next to them you can see an icon of two people with a headset. That is the third accelerator dashboard that I loaded. But looking at the top right, in the red frame, you can see a hyperlink called **More Accelerators**; click on it.

Imagine that you want to use financial trading data and an accelerator dashboard to kick-start your project. Search for trading:

## Accelerators

Rapidly build robust dashboards tailored to industry or enterprise application, all using your data.

- Learn more

![](img/66cf6b89349fda3581a4f196cb3c98a4_370_0.png)

- Sort by
- Number of Downloads
- Connection Type
- All
- Language
- All
- Features
- Built by Tableau + Salesforce
- Data Mapping enabled
- Categories
- All
- Corporate Finance
- Customer Service
- Digital & Social Media
- Digital Agency

## All Accelerators

- Salesforce Sales Cloud - Weighted Sales Pipeline
- by Tableau
- Sales Cloud: Conduct overall sales pipeline review (opportunities having a...
- Budget Controlling
- by Tableau
- Monitor, track, and manage financial performance and expenditures vs budget
- Retail Sales
- by Tableau
- Assess the performance of your network & grow your sales
- keyrus
- Customer Insights with Data Cloud
- Call Center Agent Analytics
- now
- Executive Dashboard

Figure 8.55: Search accelerator

By clicking on the Trading by Tableau dashboard, a new Tableau workbook will open – with a fully built trading dashboard and, even better, data mapping capabilities, which means that you can replace the current data with different data without breaking the dashboard layout and pre-build functionality. The description in each of the accelerator dashboards contains the data mapping possible or impossible information.


| Left table | Right table | Relationship |
| --- | --- | --- |
| stops | stop_times | Original Stop Id = Stop ID |
| stop_times | trips | Trip Id = Trip ID |
| trips | routes | Route Id = Route Id |

Table 9.2: Files and their relationships

For better performance, you can now create an extract on the data source page.

- 3. Connect the following four files with the respective relationships:

- 4. Create a calculated field called Trips per Stop:

```
{ FIXED [Stop Name] : COUNTD([Trip Id]) }
```

- 5. Create a calculated field called Hour of Day:

```
DATEPART('hour', DATETIME([Departure Time]))
```

- 6. Create a new worksheet called Station Popularity Map.

- 7. Drag Stop Lon to Columns.

- 8. Drag Stop Lat to Rows.

- 9. Drag Stop Id to the Detail shelf and change mark type to Line.

- 10. Drag Stop Sequence to the Path shelf and right-click to select Dimension (not Measure).

- 11. Duplicate the Stop Lon field on Columns (hold Ctrl and drag to the right).

- 12. Right-click the second Stop Lon pill and select Dual Axis. You'll now have two layers in the Marks card.

- 13. On the bottom layer, change the mark type to Circle.

- 14. On that layer, drag SUM(Trips per Stop) to Size.

- 15. Create a calculated field Busy Stop Label:

```
IF [Trips per Stop] > 1000 THEN [stop_name] END
```

- 16. And drag this field to Color on the same layer:

![](img/66cf6b89349fda3581a4f196cb3c98a4_425_0.png)

Figure 9.48: Station Popularity Map

- 17. Create a new Worksheet called **Service Frequency by Hour**.

- 18. Drag **Hour of Day** to Columns.

- 19. Drag **Route Short Name** to Rows.

- 20. Drag **CNTD(Trip Id)** to Color.

- 21. Change mark type to Square.

- 22. Add reference lines at 7:00 and 17:00.

This makes it easier for the user to distinguish the morning, evening, and the hours in between:

![](img/66cf6b89349fda3581a4f196cb3c98a4_426_0.png)

Figure 9.49: Service frequency

- 23. Create a new worksheet called **Busiest Stations**.

- 24. Drag **Stop Name** to Rows.

- 25. Drag **CNTD(Trip Id)** to Columns.

- 26. Sort descending.

- 27. Drag Route Type to Color.

![](img/66cf6b89349fda3581a4f196cb3c98a4_427_0.png)

Figure 9.50: Busiest station

- 28. Create a new dashboard called Timetable Explorer
  - Place on the left: Station Popularity Map.
  - Place on the right: Service frequency by hour (top) and Busiest stations (bottom).

### Timetable Explorer

### Station Popularity Map

![](img/66cf6b89349fda3581a4f196cb3c98a4_428_0.png)

### Service Frequency by Hour

![](img/66cf6b89349fda3581a4f196cb3c98a4_428_1.png)

### Busiest Stations

![](img/66cf6b89349fda3581a4f196cb3c98a4_428_2.png)

Figure 9.51: Timetable Explorer

The dashboard helps to understand three core aspects

In the **Station Popularity Map**, all train stations are sized by how many trains stop there (busiest stations appear as large circles). This identifies key transportation hubs and reveals network density patterns across Switzerland and neighboring countries.

In the **Service Frequency by Hour**, we show a heatmap indicating which routes run at which hours of the day. This helps plan travel, shows peak vs. off-peak service, and reveals routes that only run during rush hour.

Lastly, the **Busiest Stations** worksheet contains a ranked bar chart of the top stations by train frequency. This is a quick reference for the most important nodes in the network.

## Summary

In this chapter, we explored how to extend Tableau's mapping capabilities without leaving the interface by capturing Tableau-generated latitude and longitude data and how to look this up on Google. We created polygons and a heatmap for Texas based on a .shp file as well as a dashboard with a dual map axis and another dashboard with 4 map layers. Next, we explored various ways to extend Tableau's mapping using other technology. We connected to a WMS server and then explored the Mapbox offering, followed by an excursion to the world of polygons.

In the next chapter, we will explore using Tableau for presentations. Specifically, we will look at how to get the best images out of Tableau, how to effectively and efficiently use Tableau with PowerPoint, and how to use Tableau directly for presentations without relying on third-party tools.

## Get this book's PDF version and more

Scan the QR code (or go to packtpub.com/unlock), search for this book by name, confirm the edition, and then follow the steps on the page.

![](img/66cf6b89349fda3581a4f196cb3c98a4_429_0.png)

UNLOCK NOW

Note: Keep your invoice handy. Purchases made directly from Packt do not require an invoice.

## 10

## Designing Dashboards and Best Practices for Visualizations

In this chapter, we will delve into the essential principles and best practices of data visualization, focusing specifically on dashboard design. By exploring how to present data in a visually compelling and informative manner, you will gain the knowledge and learn techniques that will help to create impactful dashboards to effectively communicate insights, drive decision-making, and engage your audience. This chapter was influenced by some giants in data visualization: _The Functional Art_ by Alberto Cairo, _Information Dashboard Design_ by Stephen Few, _The Big Book of Dashboards_ by Steve Wexler, Jeffrey Shaffer, and Andy Cotgreave, _Universal Principles of Design_ by William Lidwell, Kritina Holden, and Jill Butler, _Data Story_ by Nancy Duarte, and _The Visual Display of Quantitative Information_ by Edward Tufte.

In this chapter, we will cover the following topics:

- Visualization design theory
- Formatting rules
- Color rules
- Visualization type rules
- Keeping visualizations simple
- Dashboard design
- Dashboard best practices
- Something extra

Now that we have defined the topics of this chapter, let’s dive into design theory, which will provide you with rules that can be applied to every single dashboard you will build.

### Visualization design theory

Your visualization thought process should always begin with the basics of design from the lowest level. In other words, we will start talking about the worksheets displayed on dashboards and ensure that those worksheets are well designed.

Therefore, our discussion will begin with a consideration of visualization design principles, such as the following collection, which is inspired by the 7 principles of design:

- Emphasize what is most important
- Amplify important data points
- Align and balance elements with appropriate use of color, size, and texture
- Use contrast to highlight elements or groups
- Limit the use of colors or fonts to three to increase brand identity and keep control
- Use human-user friendly proportions, like the golden cut
- Keep the user's eye moving in a Z line
- Use lines for benchmarks or thresholds
- Use white space to organize and structure

You can find more about this here: https://www.turing.com/kb/what-are-the-7-principles-of-design-detailed-breakdown

In the next section, we will consider various rules, while keeping in mind that once you're comfortable with the basics, rules can be broken to serve a purpose.

### Formatting rules

The formatting rules that we look at in this section encompass fonts, lines, and bands. Fonts are, of course, an obvious formatting consideration. Lines and bands, however, may not be something you typically work with—but in Tableau, lines and bands should certainly be considered. This illustrates that data visualization is closely related to graphic design, where lines and bands play a much bigger role than they do in, for example, Excel, and that formatting considers much more than just the textual layout.

Let's look at the rules.

### Keeping the font choice simple

Typically, using one or two fonts on a dashboard is advisable. More fonts can create a confusing environment and interfere with readability.

Fonts chosen for titles should be thick and solid, while body fonts should be easy to read. In Tableau, choosing appropriate fonts is simple: select Format | Font to display the Format Font window to see and choose the fonts you like.

The Tableau fonts always work, like Tableau Bold, Tableau Book, Tableau Medium, Tableau Light, and so on. And for a special occasion, I like to use Corbel or play around with new fonts.

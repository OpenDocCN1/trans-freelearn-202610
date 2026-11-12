
- 11. Format SUM(Sales):
  - Right-click the field in the Data pane | Default Properties | Number Format...
  - Choose Currency (Custom), 0 decimals

Now you have this:

![](img/66cf6b89349fda3581a4f196cb3c98a4_486_0.png)

Figure 10.55: KPI - Sales

- 12. Duplicate the sheet (right-click KPI – Sales | Duplicate)
- 13. Rename to KPI – Profit Ratio
- 14. Replace the measure on Text in the marks card:
  - Drag Profit Ratio onto the Text shelf, replacing Sales
  - Format Profit Ratio: Default Properties | Number Format | Percentage, 1 decimal
- 15. Change the label to show: <AGG(Profit Ratio)> Profit Ratio

- 16. Duplicate again and rename to KPI – Ship Days
- 17. Replace the measure with AVG([Shipping Days])
- 18. Format as Number with 1 decimal and change the Label text to: <AVG(Shipping Days)> Avg Days to Ship
- 19. Open a new worksheet
- 20. Rename it to Sales Trends
- 21. Drag Order Date to Columns
  - Right-click it and choose Month
- 22. Drag Sales to Rows
- 23. Select Mark type: Line

| | Columns | MONTH(Order Date) |
| --- | --- | --- |
| | Rows | SUM(Sales) |

![](img/66cf6b89349fda3581a4f196cb3c98a4_487_0.png)

Figure 10.56: Sales Trend

- 24. Create another new worksheet and rename it to Map – Sales by State
- 25. Drag State/Province to Detail
- 26. Drag Sales to Color
- 27. Set color to a single divergent palette (e.g. light to dark).
- 28. Drag Region and Segment to the Filter shelf

![](img/66cf6b89349fda3581a4f196cb3c98a4_488_0.png)

Figure 10.57: Map – Sales by State

- 29. Click New Dashboard
- 30. Set Size to Automatic
- 31. Drag a Horizontal layout container onto the top of the dashboard
- 32. Drag a blank object into the container
- 33. Drag a Text object on top of that blank and type:
  - Superstore Performance Overview
  - Set the background to teal and the font to white, bold, 16–20pt
- 34. Under the header, drag another Horizontal container.

- 35. Drag the worksheet Map – Sales by State temporarily into the dashboard.
- 36. Select Filters | Month of Order Date.
- 37. Right-click the filter control (on the dashboard) | Apply to Worksheets | All Using This Data Source.
- 38. Also select Multiple Values (dropdown).
- 39. Do the same with Country/Region, Region and Segment filters.
- 40. Arrange the filter controls in that horizontal container to make a clean "filter row", then you can remove the temporary sheet if you want. Use the Distribute Contents Evenly function for perfect alignment.
- 41. Drag a Horizontal container below the filters.
- 42. Drag each KPI sheet into this container:
  - KPI – Sales
  - KPI – Profit Ratio
  - KPI – Ship Days

This row should feel like three neat "cards" across the top of your dashboard body.

- 43. Drag a Vertical container under the KPI row
- 44. Inside it, drag:
  - Map – Sales by State (top)
  - Sales Trend (bottom)
- 45. Adjust the relative heights by dragging the divider between them
- 46. Make sure tooltips are clean and informative

To stay true to the original Super Sample Superstore design (which is praised for being clean and corporate style), do a final check:

- Consistency: same fonts and colors across all views.
- Whitespace & alignment: use layout containers so things line up in a tidy grid and avoid random gaps.
- Color with meaning: blue/grey theme; use bright accent color only when highlighting something important (e.g. negative profit).
- Minimal clutter: remove unnecessary gridlines, borders, and chart junk.

I made a couple of cosmetic changes, and my final dashboard looks as follows:

## Superstore Performance Overview

## Key Performance Indicators

$2,326,534 Sales   12.6% Profit Ratio   4.0 Avg Days to Ship

## Map - Sales by State

![](img/66cf6b89349fda3581a4f196cb3c98a4_490_0.png)

## Sales Trend

![](img/66cf6b89349fda3581a4f196cb3c98a4_490_1.png)

Figure 10.58: Dashboard

This follow-along exercise is valuable because it teaches not just how to reproduce a polished dashboard, but why each design choice matters, helping you build the practical skills and visual judgment needed to create clear, effective, and professional Tableau dashboards.

## Summary

We began this chapter by considering visualization design theory. We looked at formatting rules, color rules, and rules about which visualization types to use and which we need to avoid. We also explored how to compromise when contradictory design goals are expressed by end users. Then, we discussed dashboard design principles. We covered three popular layouts: the Golden Rectangle, quad, and small multiple.

Afterward, we looked at how to use sheet selection techniques as an ally in good design. Specifically, we explored how to allow the end user to choose which visualization type to view, and how to hide and display filters so as to make the best use of screen real estate. Next, we discussed actions and download buttons for a better user experience, as well as item hierarchies and the Used In feature, which is very handy for organizing your dashboard. This, in turn, will help to improve the layout design. Finally, to combine everything we learned, we analyzed three Makeover Monday dashboards and explained why those work based on the rules discussed prior.

In the next chapter, we will focus on use cases. Going from a dataset to a product will be the theme, and we will practice doing this with, inter alia, geospatial data. The knowledge you gained from the previous chapters will be very useful!

## Join our community on Discord

Join our community's Discord space for discussions with the authors and other readers:

https://discord.gg/VXpz9YPUTW

![](img/66cf6b89349fda3581a4f196cb3c98a4_491_0.png)

## 11

## Leveraging Advanced Analytics

This chapter focuses on advanced self-service analytics. Self-service analytics can be seen as a form of business intelligence, where people in a business are encouraged to execute queries on datasets themselves, instead of placing requests for queries in a backlog with their IT team. Then, query analysis can be done, which should lead to more insights and data-driven decision-making. But how do you start creating useful self-service dashboards if it's your first time doing so? How do you go from a dataset to a product? Have you ever asked yourself how other people start working on a dashboard, how they clean data, and how they come up with a dashboard design? If so, this is the right chapter for you! I want to share three use cases with you, written as a train of thought in order to give you an idea about how I work. Please note that this is just my personal experience; there are many different ways to reach your goal.

We will cover the following topics:

- Visualizing world indices correlations
- Geospatial analytics with Chicago traffic violations
- Building a map of intersections
- Extending geospatial analytics with distance measures

Now that we have had this introduction let's start with the first use case, where we'll build an exploratory dashboard that helps stakeholders compare any two indices on demand.

> **Note**
> 
> Your purchase includes a free PDF copy and exclusive extras
> 
> Your purchase includes a DRM-free PDF copy of this book, a 7-day trial to the Packt+ library (no credit card required), and additional exclusive extras. See the _Free benefits with your book_ section in the _Preface_ to unlock them instantly and maximize your learning.

## Visualizing world indices correlations

Imagine you are working on the world indices dataset and your line manager gives you the following task: Create a dashboard for me in which I can easily spot all correlated world indices and their distribution. I need it by tomorrow morning.

Now, take a few minutes before you continue reading and think about how you would tackle this task. The dataset contains 67 columns with various indices, like birth registrations or emission values, exports and imports, and forest areas, divided into 188 rows, where each row represents one country.

Write down your planned steps, open the workbook related to this chapter from https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter11, and follow your steps; time it in order to get a better feel for time estimates when working with Tableau. This way, you can make sure that you can deliver on time and manage expectations if you are ever asked how long it will take to build a certain dashboard.

## Plotting a scattergraph

And now, I will illustrate one way I could imagine solving this task:

- 1. First, think about how can I visualize all correlations at once, yet not have it be too much information? I have 65 different columns, which makes for 4,225 possible combinations. No, I decide that displaying everything won't work. The task is very generic; therefore, I decide to go for a two-parameter approach, which will enable my end user to select each of the index combinations themselves. I start sketching what the dashboard might look like and come up with the following:

![](img/66cf6b89349fda3581a4f196cb3c98a4_494_0.png)

Figure 11.1: Sketch

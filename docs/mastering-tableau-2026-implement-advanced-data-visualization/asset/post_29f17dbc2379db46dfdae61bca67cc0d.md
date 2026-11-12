
An example of the second point is using a treemap over a pie chart. Both are non-Cartesian chart types (visualizations with no fields on the **Rows** or **Columns** shelves) used to show parts of a whole. Pie charts are the more common of the two, but treemaps usually present the data better. There are at least three reasons for this:

- A treemap can represent more data points
- The rectangular nature of a treemap fits monitor space more efficiently
- Pie slice sizes are more difficult to distinguish than sizes of treemap segments

Sometimes, using a less common visualization type may elicit complaints: I like pie charts. Give me a pie chart! In such cases, a compromise may be possible. Later in this chapter, we will consider sheet swapping. As you will learn, sheet swapping can allow the end user to determine which visualization type to view. In the end, if a compromise is not possible and the person responsible for your paycheck desires a less-than-ideal chart type... well, I recommend you do as they ask!

## Dashboard design

Now that we have completed our discussion of visualization theory, let's turn our attention to dashboard design. We'll begin by asking the question, what is a dashboard? This is rather difficult to answer; however, its usage in everyday conversation in many organizations would suggest that people have a definite idea as to what a dashboard is. Furthermore, search engine results provide no shortage of definitions. But those definitions can differ significantly and even be contradictory.

Why is it so difficult to define a dashboard? In part, it is because data visualization as a whole, and dashboarding specifically, is an emerging field that combines many other disciplines. These disciplines include statistical analysis, graphic, and web design, computer science, and even journalism. An emerging field with so many components is a moving target and, as such, is difficult to define.

For our purposes, we will begin with Stephen Few's definition as it first appeared in an issue of *Intelligent Enterprise* in 2004. He states that a dashboard is a visual display of vital statistics we need to reach, and all these details are present on a sole screen so that this information can be observed in one place. Then, we'll extend and adapt that definition for Tableau dashboards.

Although this definition is good, Tableau takes a broader approach. For instance, a Tableau dashboard may be contained on a single screen but can be designed (and quite effectively so) to require scrolling. More importantly, Tableau dashboards are typically interactive, which opens up a world of exploration, analysis, and design options. Therefore, let's attempt a Tableau-centric dashboard definition:

> A Tableau dashboard is a display that contains one or more data visualizations designed to enable a user to quickly view metrics. This display may provide interactive elements, such as filtering, highlighting, and drill-down capabilities that enable further exploration and analysis.

## Dashboard layout

The layout of a dashboard is important for the same reason that the layout of a magazine foldout or a web page is important. Placing the right information in the right place helps the viewer quickly and efficiently gather information and draw conclusions.

In order to appreciate this fact, consider the last time you had to hunt through a poorly constructed web page to find important information. Your time could have been better used actually applying that important information!

## The golden ratio layout

You have probably heard of the Fibonacci sequence or the Golden Ratio. Since it may have been a few years since you attended a math class, a brief reminder may prove helpful.

The Fibonacci sequence is a series of numbers where every number is the sum of the previous two numbers—for example, 1, 1, 2, 3, 5, 8, 13, 21.

A Golden Rectangle is achieved when the ratio of the longest side to the shortest side of a rectangle is approximately 1.618. This ratio is known as the Golden Ratio. Mathematically, the Golden Ratio is represented as follows:

```
\varphi = \frac{1 + \sqrt{5}}{2} = 1.618033988749...
```

You can see the connection between the Fibonacci sequence and the Golden Ratio when dividing each number of the Fibonacci sequence by the previous number; for example, take the following sequence:

```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

This leads to the following:

```
1/1=1, 2/1=2, 3/2=1.5, 5/3=1.67, 8/5=1.6, 13/8=1.625, 21/13=1.615, 34/21=1.619
```

Now, let's consider a dashboard layout using the Golden Ratio. The layout shown here is constructed of rectangles so that each is 1.618 times longer or taller than the next. The spiral (known as the Golden Spiral) is displayed to demonstrate the order of the rectangles:

![](img/66cf6b89349fda3581a4f196cb3c98a4_450_0.png)

Figure 10.12: Golden Ratio (https://www.pixpa.com/blog/golden-ratio)

The Fibonacci sequence/Golden Rectangle/Golden Ratio appears endlessly in nature and throughout history. Many seashells and flowers adhere closely to the Fibonacci sequence. The Great Pyramid of Giza appears to have been constructed with the Golden Ratio in mind. Phidias likely used the Golden Ratio to design his statues for the Athenian Parthenon. Indeed, the Parthenon itself was designed with Golden Rectangle proportions. Even if more recent studies have shown that the ratio can't be replicated to decimals, nature, and art are still very close to the mathematics behind it.

So, does the Golden Ratio, as pictured in the preceding diagram, represent the ideal dashboard layout? Perhaps it's more pertinent to say that this image represents one acceptable dashboard layout. The ideal is not so much found in the abstract as it's found in the application. Dashboard layouts may sometimes approximate the Golden Ratio but, as we will see, other dashboard layouts may be better for different scenarios.

The dashboard pictured here (which is also available in the Tableau workbook that accompanies this chapter) utilizes the Golden Rectangle:

![](img/66cf6b89349fda3581a4f196cb3c98a4_451_0.png)

Figure 10.13: Golden Rectangle

Notice that this example does not attempt to follow the Golden Ratio through to smaller and smaller rectangles. There are practical limits.

The Golden Rectangle layout is particularly good for guiding the viewer from coarser to finer granularity. In this example, the left portion of the dashboard displays the coarsest granularity, 17 subcategories. The map is next. Finally, the scatter plot displays the finest granularity. Creating actions that follow this path would make a natural interaction for the end user. For example, an end user might first click on **Tables** and then click on the state of **Pennsylvania** to observe outliers in the scatter plot.

## The quad layout

The quad layout divides a dashboard into four more or less equal quadrants. It's easy to implement. On a blank dashboard, simply double-click on four worksheets in the **Dashboard** pane. The result is a quad layout, though some small adjustments may need to be made to account for legends, filters, and parameters. To observe a quad layout, refer to the diagram in *Figure 10.7*.

## The small multiples layout

A small multiple layout displays many views on a single dashboard. Like the quad layout, a small multiple layout can be implemented simply by double-clicking on each desired worksheet in the **Dashboard** pane.

Small multiples are useful when viewing information that utilizes the same visualization type repeatedly. Also, a consistent theme is helpful; for example, the following screenshot demonstrates a theme of profit performance per state in the USA. Attempting to create a small multiple layout with multiple visualization types and multiple themes will likely be messy and difficult to interact with:

![](img/66cf6b89349fda3581a4f196cb3c98a4_452_0.png)

Figure 10.14: Small multiple layouts

Some layouts are essentially variations of a theme. For instance, a layout that displays three views where one view on the dashboard covers double the real estate of each of the other views may essentially be a variation of the quad layout. Other layouts defy easy categorization and are unique to a given dashboard. Regardless, this will hopefully provide some food for thought as you design dashboards in the future.

We already mentioned the next section when we discussed compromises – sheet swapping, which is a great feature for compromises. If your stakeholder asks you to build something you know might not be as good or doesn't follow a visualization best practice, you can always add a sheet swap, making your stakeholders happy and delivering an alternative to other users. We will walk through a sheet-swapping exercise in the next section.

## Utilize sheet swapping

Sheet selection, often referred to as sheet swapping, allows the Tableau author to hide and display visualizations, as well as to move worksheets on and off the dashboard.

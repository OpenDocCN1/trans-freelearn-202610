+   - Reports for various teams can be developed quickly if models include the required data and grain.
+   - Users deeply familiar with the data require less in the way of titles and descriptive text and labels.
+   - It is recommended to involve the business users or a representative early on in the report design process, and potentially before all elements of the semantic model are complete. Any initial iterations of the report and feedback can contribute to the design of the final report to be published.

The second question can be addressed as follows: The prioritized business questions directly drive visualization choices, such as line/bar charts and tables. If the trend or fluctuations of measures is the top priority, then line charts, with custom scales and supporting trend and reference lines, may be chosen. If precise individual data points are required, either as standalone numbers or in relation to one or two dimensions, then cards, KPIs, and tables or matrices should be used. The choice of visuals and the visuals’ sizes, colors, and positioning on the canvas relative to other visuals should not be an arbitrary decision or guess. Consider the following:

+   - Establish the starting point for the analysis, including measures and grain such as weekly sales.
+   - Stakeholders often have many related questions and need to navigate the model quickly.
+   - Additional drilling and filtering features can be added to a report’s starting point.
+   - Standard line, bar/column, and scatter chart visualizations have natural advantages in terms of visual perception and user comprehension. Other visuals should be utilized for their specific use cases and strengths, such as a Funnel visualization for stages of a process and a Waterfall visualization for the contributions of dimension values to an overall total.

How will the report be accessed and utilized? How a report will be accessed and how business users desire to interact with the report are key questions that should be answered before beginning report development. Report access and interactivity are key components that a report designer should understand in order to ensure that the report contains the proper elements, views, filters, and measures. Consider the following:

+   - Will users only view the report, or is the report a starting point for further analysis?
+   - If there is no interaction or limited interaction desired, plan to embed conditional logic and exceptions in the report.
+   - If there is high interaction, plan to use hierarchies, tooltips, and slicers to enable data exploration.
+   - How does the report relate to existing dashboards?
+   - Identify the components of the report that will contribute to an existing dashboard.
+   - If creating a new dashboard based on the report, identify the tiles and interaction.
+   - Will the report only be accessed via the web browser, or will mobile devices be used regularly?
+   - If mobile consumption is expected, reports can be designed to optimize this experience.
+   - Power BI dashboards can also be optimized for phone layout in the Power BI service.
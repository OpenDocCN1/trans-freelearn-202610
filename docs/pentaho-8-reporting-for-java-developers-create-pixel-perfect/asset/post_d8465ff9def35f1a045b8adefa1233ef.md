
Analytics Platform is required (you will learn more about it in Chapter 15, *Using Reports in Pentaho Business Analytics Platform*). After the definition of credentials (Username and Password), Solution, and Path, the Fetch button lets you extract all the available Pentaho CDA deployed in the Pentaho Business Analytics Platform. Once listed, in the bottom panel you can edit the parameters passed to the data access, using the Edit Parameter button.

To complete the description, the Preview button is the same as you saw for the previous data sources and the OK/Cancel buttons are the standard ones used to confirm/cancel the current operation.

## Summary

In this chapter, you learnt everything Pentaho Report Designer can do with data sources, discovering all the available types and their specific features. You started with an introduction to how a data source looks in the user interface and how the fields are used in the layout of the report. Then you moved on to discover how to manage all the possible types of data sources using Report Designer, finding out all about:

- The JDBC data source
- The metadata data source
- The MongoDB data source
- The Pentaho Data Integration transformation data source
- The OLAP data source in various versions (legacy and generic, normalized and denormalized, standard, and custom)
- The XML data source
- The table data source
- The advanced data sources (scriptable, external, sequence generator, and OpenERP)
- The Pentaho community data access data source

Now that you have read this chapter, you should feel comfortable with all the ways Pentaho Report Designer offers to connect to a source. This is the very first step to go through in the development of a simple and complex Pentaho report.

In the next chapter, you will learn everything about including graphics and charts in your Pentaho reports.

## Including Graphics and Charts in Reports

After the discovery of all the possible data source types, in this chapter you will learn how to incorporate graphics and charts in your Pentaho reports. You will start understanding how to include graphics and charts from the Pentaho Report Designer and then you will move on to the description of all the settings and configurations. Graphics and charts will be described one by one, as we did for the data sources, with the goal to make you a real expert for the best final result; the most useful and appealing report for your customers and manager.

In particular, the charts discussed are bar charts, line charts, area charts, bar line charts, pie charts, multipie charts, ring charts, bubble charts, scatter plot charts, XY bar charts, XY line charts (regular and extended), XY area charts, XY area line charts, waterfall charts, radar charts, thermometer charts, bar-sparklines, line-sparklines, and pie-sparklines.

This chapter is written as a reference manual for developers and information technologists. The goal is to share all the relevant and advanced features about graphics and charts that are available in Pentaho Report Designer.

### Including static images in reports

Including static images in your Pentaho report is useful, because in that way you can add logos, and graphics in general, to make the final result more appealing for the users.

To include static images in your report, select the image icon (🖼️) from the Report Designer palette and place it into your report. Double-click on the element, or right-click on it by selecting Edit Content, to bring up a resource dialog where you can browse to the specific file location. You may select the Link to option to link an external resource, or you may select Embed to include the image in the `.prpt` file.

![](img/9337ac94537bf7e092e1ea6531dce8fd_332_0.png)

> *If you are in doubt, using the embedded image is always a good choice, as you can avoid the bad side effect of a change in the external link. When embedding the image, the dimensions of the `.prpt` file will be bigger.*

### Including dynamic images in reports

Including dynamic images in your Pentaho report is useful, because in that way you can add logos, and graphics in general, that may change according to a data source. As an example, we can imagine a report changing its logo depending on the department of the user or the image of a product in a product list.

To add dynamic images to your report, use the image-field icon (🖼️) from the Report Designer palette and place it in your report. The content field accepts different types of image inputs for rendering. By double-clicking on the element, you can edit the source pointing to the right field of your dataset. If you have a field that contains a URL or file system location to your image, the image-field element will render the specified image.

### Including charts in reports

Including charts in your Pentaho reports makes a difference because of two main reasons: it's powerful and it's easy. Pentaho charts are powerful because they give you the ability to represent complex information in a single image (the chart itself) with many different styles. They are easy because the inclusion of chart in a report can be easily done with a configuration (and not development), starting from a data source.

> *Pentaho Reporting relies on JFreeChart. The JFreeChart project was founded in February 2000 and today it is the most widely used chart library for Java, with more than 2.2 million downloads to date.*

To add charts to your report, drag the chart icon (📊) from the Report Designer palette and drop it in the place where you want to see the chart. Usually, a chart is placed in a grouping band like Report Header or Group Header (and rarely on footers or Page Header). This is because a chart makes sense if used on a collection of data and not on single values. In any case, nothing really limits your fantasy and you can place the chart anywhere in the report.

Once dropped in the report canvas, an image similar to the following one is shown:

![](img/9337ac94537bf7e092e1ea6531dce8fd_334_0.png)

Double-clicking on the image or right-clicking and selecting chart, the chart editor similar to the following screenshot is shown:

![](img/9337ac94537bf7e092e1ea6531dce8fd_335_0.png)

Looking at the chart editor, three main areas appear when editing a chart. The first area is on top of the panel and lists all the available types of charts (17 in Pentaho Reporting 8.0). The second area, on the left side, contains a set of properties related to chart rendering. The third tabbed area of properties, on the right side, is related to the data that populates a chart.

Selecting one type of chart, the property panel is updated according to the selection, as well as the panel of data properties. In the following sections, you will see in detail all the available properties for all the types of charts.

As you can expect, the different types of charts are suitable for different use cases. If you want to show the strength of a trend for a single value over time, the best chart types are:

- Line
- Area
- XY line area
- XY line

If you are directly comparing two or more related values, the best chart types to choose are:

- Pie
- Ring
- Bar
- Line
- Area
- Radar

If you want to show how one set of values directly affects another, the best chart types are:

- Bar line combination
- Waterfall

If you are comparing a large number of data points, the best chart types are:

- XY dot (scatter plot)
- Bubble
- Pie grid (multipie)

## Types of datasets

All chart types receive their data from different types of datasets. Each chart can be compliant with one or more dataset types. Dataset types can be selected and managed in the chart editor through the right panel.

Following are sections on each type of dataset, describing in detail their properties and their meanings. During the description, you will also discover the chart types compliant with each type of dataset. Also, in case of dataset types, the properties are grouped for better management and understanding.

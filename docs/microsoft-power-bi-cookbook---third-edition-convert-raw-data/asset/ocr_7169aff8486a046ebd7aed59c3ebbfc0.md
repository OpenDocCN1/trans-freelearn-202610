# There’s more...

In the drillthrough area for a page, you may have noticed the Cross-report toggle. Power BI does indeed allow you to drill through reports, and not just pages. The source report must be enabled to use drillthrough targets from other reports, either in Report settings of CURRENT FILE in Options and settings | Options, or in the Power BI service. The target report would then be configured to toggle the Cross-report option to On and the drillthrough fields configured appropriately. It is important to note that Cross-report filtering requires that the semantic model schema of the reports match.

![](bbox=[0.087, 0.23, 0.867, 0.465])

Figure 4.44: Cross-report drillthrough

# See also

+   - Create tooltips based on report pages: http://bit.ly/2OdzeHA
+   - Extend visuals with report page tooltips: http://bit.ly/3cGwBHC
+   - Set up drillthrough in Power BI reports: http://bit.ly/31SH2Iv
+   - Use report page drillthrough: http://bit.ly/3sKg5w0
+   - Use cross-report drillthrough in Power BI Desktop: http://bit.ly/3f zfQjI

# Formatting Reports for Publication

Power BI Desktop includes features to control and enhance the formatting and layout of reports at an extreme level of detail. Prior to publishing reports to the Power BI service, visuals can be aligned, sized, and evenly spaced to deliver an organized, symmetrical layout. Additionally, supplemental report elements, such as shapes, text boxes, and images, can be added to further organize and enrich report pages with textual and visual aids. Finally, the actual canvas size can be changed from the default 16:9 width-to-height ratio (i.e, a height of 720 px and width of 1280 px) to a 4:3 ratio or even a custom size.

This recipe demonstrates how to control the positioning, alignment, and distribution of report visuals, as well as tips for presenting and working with visuals in groups. Additionally, helpful tips are presented for final clean-up items prior to publishing a report, as well as setting up Q&A after publishing.
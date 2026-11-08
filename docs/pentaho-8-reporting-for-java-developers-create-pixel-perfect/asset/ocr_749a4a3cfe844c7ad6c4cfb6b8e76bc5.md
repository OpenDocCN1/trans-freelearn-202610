image in the report.

![](bbox=[0.143, 0.135, 0.207, 0.185])

> *Most of the time, it's a good idea to embed the image in the report to a problems in case the internet connection is not available or the link is*

In our case, let's point to the image named `world.64x64.png` stored in the `Chapter 04 - Creating a report with Report Designer/resource` path of the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository. To point to the image from Pentaho Report Designer, clone the project locally or download the image directly into your laptop.

Now that the logo is correctly positioned in your report, let's add the title. The task is very similar to the previous one: drag the label icon (Ab) from the Report Element palette and drop it into the Report Header band, as shown in the following screenshot. Note that the label is positioned in the middle of the image and its length has been extended till the end of the page.

![](bbox=[0.18, 0.456, 0.822, 0.635])

Click on the label element in the Report Header band and change the format, acting on the toolbar highlighted in the image. Select bold option by clicking on the B button and change the font dimension to `20` in the drop-down menu. Now that the format is updated, double-click on the `label` element to have access to the content. Change the text to `Actuals per position title` and press *Enter* to confirm the new content of the label.

Now that the header is correctly updated, let's preview it by clicking on the eye icon (👁). The following screenshot shows how the preview should look:
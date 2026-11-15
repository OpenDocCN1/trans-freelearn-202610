
### Tip 2: Hexagon Map

A `hexagon map` (`hex map`) has been used instead of the default map that comes with Tableau. The reason is that smaller states on the east coast disappear when printed in the actual ratio and Alaska and Hawaii are so far away that they usually don’t fit on the map either. A hex map gives the same space to each state and hence, even if printed, the user can see what’s going on in Alaska as well as in Rhode Island.

### Tip 3: Color Indicators

The colors indicate that the profit ratio is especially bad in Ohio and Colorado, but your audience might want to know more. That’s when the table comes into play. The profit ratio is profit divided by sales and the table shows both numbers so that the audience can get a better picture of the current situation and you can avoid them asking.

### Tip 4: Bottom 20 States

In order to avoid a printed scroll-down bar, I selected the bottom 20 states only. If the topic is loss leaders, it is not very likely that you will need to show all the states that do well.

I hope this example gave you some ideas on how to prepare your dashboard for printing! If printing happens more frequently, I advise you to add it as a page to your interactive dashboard on Tableau Server such that you always have it ready to go if asked for. If you are interested in replicating the hex map, check *Chapter 8*, *Going Beyond the Basics*, or go ahead and check out the following article: `https://www.tableau.com/about/blog/2017/1/viz-whiz-hex-tile-maps-64713`.

Next, we will look at another example, this time an automation for a weekly PowerPoint presentation.

## Semi-automating a PowerPoint presentation

The previous exercises (*Creating a template* and *Creating a dashboard for print*) demonstrated two methods for generating a template for a PowerPoint presentation and gave you some tips on how to adjust your dashboard for printing and/or PowerPoint presentations. Usually, however, your Tableau dashboard isn’t static, and the data will change by the next time you need it. You can, of course, export a new PowerPoint file with the updated data, but you might have other slides as well that you don’t want to lose. The following steps will help you with a semi-automated way of updating your PowerPoint presentation:

1.  Open the dashboard created previously entitled `Superstore Loss Leaders`.
2.  Click on `Dashboard` | `Export Image…`:
    ![](Images/B18435_10_18.png)
    Figure 10.18: Export Image…
3.  Click `OK` and, in the resulting dialog box, choose to save the image as `SuperstoreLossLeaders.png`.
4.  Open the PowerPoint presentation. Open a slide.
5.  Select the `Insert` ribbon and choose `Pictures` | `Picture from File…`:
    ![](Images/B18435_10_19.png)
    Figure 10.19: Inserting a picture into PowerPoint
6.  Navigate to the PNG image you just exported.
7.  In the `Insert` dialog box, choose the drop-down selector next to `Insert` and select `Insert and Link` for Windows, or select `Options` | `Link to File` | `Insert` for Mac. Note that this step is important because it will allow pictures to be swapped out easily, as will be demonstrated:
    ![](Images/B18435_10_20.png)
    Figure 10.20: Link to File
8.  Save the PowerPoint presentation as `AutoUpdate.pptx`.
9.  Close PowerPoint. In Tableau, modify the dashboard; for example, remove a state from the table.
10. Overwrite the previously exported `SuperstoreLossLeaders.png` image via `Dashboard` | `Export Image…`.
11. Open `AutoUpdate.pptx` and note that the new image of the dashboard displays.

Something I come across frequently when I teach in Tableau classes is people who are responsible for providing PowerPoint presentations on a weekly or monthly basis. These presentations include charts and graphs that are updated, but the overall layout remains unchanged. The technique covered in the preceding exercise can make that process much quicker. Although Tableau Desktop only allows the author to update one image at a time, Tableau Server can be configured to export multiple images simultaneously, thereby making the process even more efficient. Exporting images from Tableau Server is discussed in *Chapter 14*, *Exploring Tableau Server and Tableau Cloud*.

Wouldn’t it be even nicer if we could present our web browser, Tableau Server, directly on a slide, including all the interactivity? The next section will cover this while sharing some thoughts on doing so.

## Embedding Tableau into PowerPoint

It is possible to embed Tableau directly into a PowerPoint presentation or, to be more accurate, it’s possible to embed a web browser through which an instance of Tableau Server may be accessed. There are various methods for accomplishing this, including the Web Viewer app, a third-party add-in called LiveWeb, and VBA code.

The Web Viewer app is available at `https://appsource.microsoft.com/en-us/product/office/WA104295828?tab=Overview`. Although it works well for Tableau Public, the default Tableau Server settings disallow access via Web Viewer. Your Tableau Server Admin will be able to help change the settings though. LiveWeb (available at `http://skp.mvps.org/liveweb.htm`) works well but requires an additional installation. You must download the installation from the URL provided and start PowerPoint. You will then be able to add another add-in to your PowerPoint ribbon, which, in return, allows you to add a Tableau Server URL that will be displayed directly in Tableau. Detailed instructions can be found on the website too.

## Embedding Tableau into Google Slides

Google Slides is another widely used presentation tool and, due to its web-based nature, a convenient one. Just like in PowerPoint, you can insert images, audio, and video but unfortunately no web page or a Tableau dashboard.

At the time of writing, I could not find a workaround either. However, if you don’t want to switch between applications, Google Slides can be embedded into Tableau, so the other way around. This limits the tools at least to one!

Let’s give it a try:

You do need a Google account to do this.

1.  Open Google Slides, `https://docs.google.com/presentation`, and click on `Blank` to open a new presentation.
    Type something like:
    ![](Images/B18435_10_21.png)
    Figure 10.21: Google Slides
2.  Fill a second page:
    ![](Images/B18435_10_22.png)
    Figure 10.22: Test slides
3.  Click on `File` | `Share` | `Publish to web`.
4.  Change the dropdown `Auto-advance slides` to `every minute`. Then click on `Publish`:
    ![](Images/B18435_10_23.png)
    Figure 10.23: Publish to the web
5.  Copy the URL that now appears.
6.  Open a new dashboard page in your Tableau workbook.
7.  Drag the `Web Page` object onto the dashboard:
    ![](Images/B18435_10_24.png)
    Figure 10.24: Web Page
8.  Paste the URL that you previously copied from Google Slides and remove the highlighted part, `&delayms=60000`:
    ![](Images/B18435_10_25.png)
    Figure 10.25: URL
9.  Now you should be able to see your presentation in Tableau! The control for the slides can be found at the bottom left:
    ![](Images/B18435_10_26.png)
    Figure 10.26: Slides in Tableau

In this section, you have learned how you can embed Google Slides into a Tableau dashboard and hence use one tool instead of switching between two. Next, we will discuss ways to bring animation to Tableau that can support your data storytelling.

## Animating Tableau

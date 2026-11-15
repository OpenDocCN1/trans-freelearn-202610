
As we briefly mentioned in the `Creating a collapsible menu` section, if you open the `Latin American Population` dashboard, you will notice `Item hierarchy` at the bottom left of the `Layout` pane. By clicking on the arrow next to the word `Tiled`, you will be able to see all the different parts of the dashboards and the worksheets used in each container. You can also spot containers in containers and the structure of your layout:

![](Images/B18435_11_48.png)

Figure 11.48: Item hierarchy extended

Try to keep a logical order within your item hierarchy. This will make it easier for you to make changes to your dashboard later on. Which logic you choose is up to you; one I use often is high-level to more detailed, most important to least important, or from spacious worksheets to smaller ones. A nice article on keeping your dashboard clean can be found here: [https://www.thedataschool.co.uk/joe-beaven/layout-containers-how-to-get-your-item-hierarchy-under-control](https://www.thedataschool.co.uk/joe-beaven/layout-containers-how-to-get-your-item-hierarchy-under-control).

## Used In

The `Used In` feature shows you on which dashboards a certain worksheet is used. Just right-click on a worksheet and select `Used In`:

![](Images/B18435_11_49.png)

Figure 11.49: Used In

This feature is especially useful if you want to make changes to a worksheet. Before execution, you can check which dashboards will be affected, and the risk of breaking an existing dashboard will decrease tremendously.

### Something extra

When I first started using Tableau I was, and still am, a really big fan of Makeover Monday ([https://www.makeovermonday.co.uk](https://www.makeovermonday.co.uk)). Makeover Monday is an initiative hosted by Eva Murray and Andy Kriebel. Every Monday, the Tableau community (you) is given a dataset accompanied by a visualization and asked to improve it. Those can come from newspapers or websites for example. The participants will then analyze the dataset, build a dashboard, and publish that version on Twitter using the hashtag `#makeovermonday`. If you compare the work from the community with the original work, you will surely learn to see which dashboards work best and which designs you prefer, and thus you can slowly develop your own style, especially when participating.

The learnings from the first years of Makeover Monday were summarized in a book with the same title. In 2022, Makeover Monday took a break but is back now in a slightly different format and led by Andy alone. Check it out; it is worth it. Below are some examples and my explanation of why it works; you will recognize the rules we have discussed in this chapter (which I will put in brackets):

The first example was published in `The Guardian` and shows a decrease in exported goods in the UK in January 2020.

Link to the data: [https://data.world/makeovermonday/2021w13](https://data.world/makeovermonday/2021w13).

![](Images/B18435_11_50.png)

Figure 11.50: UK exports

### What works?

Using a line chart (rule: keeping visualizations simple) for time series data is a good idea. I also like that the line is red (rule: psychological implications of colors) because the visualization is showing something negative, a steep drop in exports. The Y-axis indicates the monetary value of those changes, giving me as the user an idea of the absolute loss.

Now let’s see what the Makeover Monday folks came up with:

Kimberly Furdell created the chart shown in `Figure 11.51` and without reading much further, ask yourself, what are the differences compared to the original visualization, and which one do you prefer and why?

Link to the visualization: [https://public.tableau.com/app/profile/kimberly.furdell/viz/UKexportstoEU\_16172321299550/Dashboard1](https://public.tableau.com/app/profile/kimberly.furdell/viz/UKexportstoEU_16172321299550/Dashboard1).

![](Images/B18435_11_51.png)

Figure 11.51: Brexit exports

### Why it works better

The story from the article published in `The Guardian` revolved around the 40% drop in exports in January 2021 compared to January 2020. That the number was related to the previous year was not clear in the original visualization, and Kimberly has enough information in her visualization that you do not need to read the full article to understand (rule: maximizing documentation on a dashboard). To support the storyline further, Kimberly did an excellent job in changing the graph.

This visualization works better because the `Y`-axis shows a percentage. Now, you can clearly see that the drop in January 2021 is more severe than the drop in 2020, which looked similar to the original graph because of the nature of absolute values. Increasing the time series data by more years stresses that the developments are outside the past years’ range. The gray lines on her dashboard represent the various export categories, and as such, it is a great idea to have an area chart for the total values, and line charts, in a less prominent color, for the details (rule: presenting dense information, using pure colors sparingly). The reference lines indicating events during this period of time make the story complete (rule: telling a story). Lastly, users wondering which category contributed the most to the drastic export decline can select a second view, a horizontal bar chart, from the drop-down menu (design: sheet swapping).

Next, we will look at another Makeover Monday challenge and two examples of how to do it better.

This time, we are concerned about the caffeine content in various coffees from different known chains. The article and data were initially published on [www.which.co.uk](http://www.which.co.uk).

Link to the challenge: [https://data.world/makeovermonday/2023w9](https://data.world/makeovermonday/2023w9).

Here you can find the original visualization, which is not that bad in my opinion:

![](Images/B18435_11_52.png)

Figure 11.52: Coffee

### What works?

The coffee cup images are good; without checking the table or reading the header, you will know that this visualization is about coffee. It will catch your attention. There is a limited use of colors but the background is not my favorite; the blue is catchy but unrelated to the topic and a bit heavy on the eye. The table, however, works; is it easy to compare the different drinks per brand and the same drinks across different brands (rule: keep it simple and robust).

Showing the caffeine as well as the ml amount is almost too much information and could be better normalized. But let’s see what the Tableau community came up with.

This first example, `Figure 11.53`, was published by Will Sutton. He created a story, where each page is a step toward the improved version. What a great idea!

Link to the visualization: [https://public.tableau.com/app/profile/wjsutton/viz/HighStreetCaffeine/ImprovingabarchartMakeoverMonday2023Week9](https://public.tableau.com/app/profile/wjsutton/viz/HighStreetCaffeine/ImprovingabarchartMakeoverMonday2023Week9).

![](Images/B18435_11_53.png)

Figure 11.53: Callouts

### Why it works better

Part of why it works better can be seen next to the horizontal bar chart; the numbers were put into context (rule: telling a story). Other than that, Will changed the table to a very simple bar chart; he focuses on one drink only and marks the highest possible health risk by imposing espresso in a dark red (rules: making a dashboard simple and robust, simple colors). He walks the user through a story, whereas the original table leaves a lot of room for context and answers to unknown questions.

But I will show you a redo with all the coffee chains and all the drinks too; this next one is by Gabriela Comanescu.

Link to the visualization: [https://public.tableau.com/app/profile/gabriela.comanescu/viz/HowmuchcaffeineisinyourcoffeeMakeoverMonday/Coffee](https://public.tableau.com/app/profile/gabriela.comanescu/viz/HowmuchcaffeineisinyourcoffeeMakeoverMonday/Coffee).

![](Images/B18435_11_54.png)

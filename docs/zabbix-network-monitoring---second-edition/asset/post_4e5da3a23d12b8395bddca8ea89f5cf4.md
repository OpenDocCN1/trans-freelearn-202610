
## Summary

We have learned to create graphs of different types and how to customize them. This allows us to place multiple items in a single graph, change their visual characteristics, choose different graph types, modify y axis scaling, and several other parameters. We were able to show basic trigger information and a percentile line on a graph.

We discovered simple, ad hoc, and custom graphs, with each category fitting a different need.

Simple graphs show data for a single item. Ad hoc graphs allow us to quickly graph multiple items from the latest data, although there's no way to save them. Custom graphs can have several items, all kinds of customization, and are similar to triggers—they are associated with all the hosts that they reference items from.

The creation of network maps also should not be a problem any more. We will be able to create nice-looking network maps, whether they show a single data center, or lots of locations spread out all over the world. Our maps will be able to show real-time data from items, network link status, and use nice background images.

In the next chapter we will look at additional ways to visualize data. Zabbix screens will allow us to combine graphs, maps, and many other elements on a single page. We will also discover how a single screen could be easily adapted to change the displayed information to a specific host. Combining the screens, slide shows will be able to show one screen for some period of time, then another, and cycle through all the selected screens that way.

## 10
## Visualizing Data with Screens and Slideshows

Having become familiar with simple, ad hoc, and custom graphs as well as network maps, we will explore a few more visualization options in this chapter. We will cover the following topics:

- Screens that can include other entities, including global and templated or host screens
- Slideshows that change displayed information on a periodic basis automatically

We looked at the individual visualization elements before; now is the time to move forward. Compound elements (which have nothing to do with map elements) allow us to combine individual elements and other sources to provide a more informative or good-looking overview. We might want to see a map of our network together with a graph of main outbound links, and perhaps also a list of current problems.

## Screens

The graphs and maps we are familiar with cannot be combined in a single page on their own—for that, we may use an entity called a **screen**. Let's proceed with creating one together: navigate to **Monitoring | Screens**, and click on the **Create screen** button. Enter **Local servers** in the **Name** field and 2 in the **Columns** field. We will be able to add more later, if needed:

![](img/f5313a804532fabacc8d07cc8607eeba_386_0.png)

> The same as with network maps, the way screens are configured has changed in Zabbix 3.0—it's now done in the monitoring section. Screens may also be created and shared by users.

Click on **Add**, and then click on **Constructor** next to **Local servers**. We are presented with a fairly unimpressive view:

![](img/f5313a804532fabacc8d07cc8607eeba_386_1.png)

So it's up to us to spice it up. Click on the left-hand **Change** link, and we have an editing form replacing the previous cell contents. The default resource type is graph, and we created some graphs earlier: click on **Select** next to the **Graph** field. In the upcoming window, make sure **A test host** is selected in the **Host** dropdown, and then click on **CPU load & traffic**. That's all we want to configure here for now, so click on **Add**.

> It is not required to save a screen explicitly, unlike most other configuration sections. All changes made are immediately saved.

Now, click on the right-hand Change link and then on Select next to the Graph field. In the upcoming window, click on Used diskspace (pie). Remember how we tuned the pie-chart dimensions before? When inserting elements for screens, we override their configured dimensions. This time, our pie chart has to share space with the other graph, so enter 390 in the Width field and 290 in the Height field, and then click on Add. While we can immediately see the result of our work here, let's look at it in all its glory – go to Monitoring | Screens and click on Local servers in the NAME column:

![](img/f5313a804532fabacc8d07cc8607eeba_387_0.png)

We now have both graphs displayed on a single page. But hey, take a look above the screen: the controls there look very much like the ones we used for graphs. And they are: using these controls, it is possible to do the same things as with graphs, only for all the screen elements. We can make all screen elements display data for a longer period of time or see what the situation was at some point in the past.

Two graphs are nice, but earlier, we talked about having a map and a graph on the same page. Let's see how we can make that happen. Click on All screens above the currently displayed screen, and click on Constructor next to Local servers. We want to add our map at the top of this screen, but we can see here that we created our screen with two columns and single row, so we have to add more. Couldn't we do that in the general screen properties, using the same fields we used when we created the screen? Of course we could, but with one limitation: increasing the column and row count there will only add new columns and rows to the right or at the bottom, respectively. There is no way to insert rows and columns at arbitrary positions using that form. That's why we will use a different approach.

Reducing the column and row count is only possible from the right-hand side and bottom when using the generic screen properties form. Any elements that have been configured in the removed fields will also be removed.

Look at those + and − buttons around the screen. They allow you to insert or remove columns and rows at arbitrary positions. While the layout might seem confusing at first, understanding a few basic principles should allow you to use them efficiently:

- Buttons at the top and bottom operate on columns
- Buttons on the left and right operate on rows
- + buttons add a column or row before the column or row they are positioned at
- − buttons remove the column or the row where they are positioned

In this case, we want to add another row at the top, so click on the upper-left + icon in the first column – the column that has + and − controls only, not the one that has a graph already. This adds a row above our graphs with two columns, both having a Change link, just like before. Click on the first Change link. It's not a graph we want to add, so choose Map from the Resource dropdown. Click on Select next to the Map field, and then click on First map. If we leave other parameters as they are, the map will appear on top of the left-hand column. Having it centered above both columns would look better. That's what the Column span option is: enter 2 in that field, and then click on Add. As can be immediately seen, this screen element now spans two columns. This capability is not limited to maps; any element can span multiple columns or rows.

## Dynamic screens

We now have a screen containing a network map and two graphs, showing data about **A test host**. Now, we should create a screen showing data for **Another host**. We'll probably have to repeat all the steps we performed for this one as well. That would be quite bad, especially for many hosts, wouldn't it? That's why there is a different, much easier approach.

Click on the **Change** link below the **CPU load & traffic** graph in the screen configuration, and look at the last parameter in there:

![](img/f5313a804532fabacc8d07cc8607eeba_388_0.png)

Let's find out what a dynamic item means—mark this option and click on **Update**. While that seemingly did nothing, edit the other graph, mark the **Dynamic item** checkbox, and click on **Update**. It's now time to check out the result—go to **Monitoring | Screens**, and click on **Local servers** in the **NAME** column. Look at the available dropdowns at the top of the screen:

![](img/f5313a804532fabacc8d07cc8607eeba_389_0.png)

As soon as we marked some elements as dynamic, we got given the choice of other hosts. Let's check out how well this works. Select **Linux servers** from the **Group** dropdown and **Another host** from the **Host** dropdown:

![](img/f5313a804532fabacc8d07cc8607eeba_389_1.png)

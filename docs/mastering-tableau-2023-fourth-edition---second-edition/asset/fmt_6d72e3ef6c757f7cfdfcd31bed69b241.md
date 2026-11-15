
By hovering over Arizona on the map, the same state will be highlighted in the right-hand table. As you can see, implementing a highlight filter can help your users find related data points faster. In contrast to the navigation action, this filter should be used for worksheets on the same page and as visual help to find a value.

## URL actions

The next action is the URL action. This one allows you to open a URL when clicking on a data point in the dashboard. Follow these steps:

1. Go to the `Quantity by State` worksheet and open `Worksheet | Actions`.
2. Delete every action that is visible.
3. Now, select `Add Action` and select `Go to URL...`. Copy all the parameters, as shown below, and click on the arrow next to the Wikipedia URL. Fields from your data source will appear, which enables you to add a field to the URL that will automatically return a value based on the dashboard. Select `State`:

![](Images/B18435_11_32.png)

Figure 11.32: URL action

4. Click on a state on your map on the `Quantity by State` worksheet, and see how Wikipedia will open the respective web page.

URL actions are very useful when you want to show your users additional information available on websites. You can also use latitude and longitude values to open Google Maps or link the URL action to internal company websites with regulations, or maybe a ticketing system.

## Navigation actions

The next action used to be commonly used in combination with a button to give the user a more interactive experience. Instead of clicking on tabs to go from one to another page, you will add buttons with an action to go to a specific sheet:

1. Go to the `Qty & Profit` dashboard and create a calculated field called `String` with the code `"Go back to […]"`. This will serve as our button shortly.
2. Open a new worksheet and call it `Button`.
3. Place the calculated field `String` by dragging and dropping `Details` as well as `Label` onto it.
4. Select `Shape` from the `Marks` card dropdown and select any shape (and color) of your choice.
5. Hide the title and disable `Tooltip`.
6. Click on `Label` and select `Bottom-Center Alignment`. The result will look as follows:

![](Images/B18435_11_33.png)

Figure 11.33: Button

7. Go back to the `Qty & Profit` dashboard, select `Floating`, and drag the `Button` worksheet onto the dashboard—for example, into the bottom-left corner:

![](Images/B18435_11_34.png)

Figure 11.34: Button on the dashboard

8. Click on `Dashboard | Actions` and copy the following parameter:

![](Images/B18435_11_35.png)

Figure 11.35: Go To Sheet Action

9. Click `OK` and click on the `Button` worksheet on the `Qty & Profit` dashboard.

It will now take you back to the `Latin American Population` dashboard! Nowadays, this action is less used since the release of a specific navigation object, but it is useful to know about it for the sake of completeness.

## Parameter actions

The next one in line is **Parameter Actions**. This one is used to change a parameter by clicking on a data point in the dashboard rather than using the parameter control. Follow these steps:

1. Place the `Categories` worksheet on the `Qty & Profit` dashboard—for example, in the top-right corner:

![](Images/B18435_11_36.png)

Figure 11.36: Parameter Actions

2. Click on `Dashboard | Actions` and copy the following parameters:

![](Images/B18435_11_37.png)

Figure 11.37: Parameter Action setup

3. Create a calculated field called `Cat Param` with the following code:

```
If [Parameter Action] = [Category]
Then [Category]
Else ""
END
```

4. Place `Cat Param` on the `Filter` shelf on the worksheet `Profit by State by Category`, select the empty value, and enable `Exclude`, as shown here, before selecting `OK`:

![](Images/B18435_11_38.png)

Figure 11.38: Category filter

5. If you now hover over any `Category` at the top, the following table will adjust accordingly:

![](Images/B18435_11_39.png)

Figure 11.39: Category-filtered dashboard

## Set actions

The last action that we will discuss is the set action. A set splits your data in two, `In` or `Out`. If you were to create a set called animals based on the following list: `[cat, tree, dog, leopard, house, car]`, you would mark cat, dog, and leopard as `In` and tree, house, and car as `Out`. You can then use the set as a new field for your dashboard. Follow these steps:

1. Go to the `Qty & Profit` dashboard and select the western states `California`, `Oregon`, and `Washington`. Note that the pop-up window shows two rings in the top-right part. Using this icon is one way to create a set; go ahead and click on it:

![](Images/B18435_11_40.png)

Figure 11.40: Set action

2. The following window will open; click `OK`:

![](Images/B18435_11_41.png)

Figure 11.41: Set

In the data pane, a new field has been created—a set called `West Coast`.

3. See how it works by changing the `Profit by State by Category` worksheet as follows, removing `State` from `Rows`, and adding `West Coast` instead (you can remove `Cat Param` from the filter shelf or leave it; both will work):

![](Images/B18435_11_42.png)

Figure 11.42: Setting field states

The profit in the `Out` band takes into account all states except for `California`, `Oregon`, and `Washington`, whereas the `In` band shows numbers relative to those three aforementioned states.

4. Go back to the `Qty & Profit` dashboard and select `Dashboard | Actions` to copy the following parameter into a `Set Action`:

![](Images/B18435_11_43.png)

Figure 11.43: Set Action parameter

5. If you now click on any one or multiple states, all selected states will be the `In` group of your set. Therefore, the profit numbers will show the profit of your selection against the rest, like so:

![](Images/B18435_11_44.png)

Figure 11.44: Set action dashboard

6. You can also rename the `In` and `Out` values by clicking and selecting `Edit Alias…`:

![](Images/B18435_11_45.png)

Figure 11.45: Edit Alias...

Those were the six Tableau actions; with the **Filter** and **Highlight** actions, you can make it easier for your users to find more detailed information by drilling down. The **Go to Sheet** and **Go to URL** actions can be seen as interactive buttons that will redirect your user to additional information, either on the web or on another sheet. And lastly, the **Change Parameter** and **Change Set** actions can be used to adjust a predefined value or multiple values, which will allow your users to interact with and analyze the data from different angles.

## Download

Next to actions, another item that’s part of the user experience is a built-in download button. Adding such a button to your dashboard will allow your users to share an image or a screenshot, if they like, with others, use it for documentation, or use it for presentation purposes. Adding a button is super easy; just follow these steps:

1. Open the dashboard `Golden Rectangle`, select `Floating` in the bottom-left corner, and drag `Download` to the top-right corner:

![](Images/B18435_11_46.png)

Figure 11.46: Download button

2. In `Presentation Mode`, click (`Alt` + click for non-presentation mode) on the newly added button; a `Print to PDF` popup will appear:

![](Images/B18435_11_47.png)

Figure 11.47: Print to PDF

3. Click `OK`, after which you can open the dashboard as a PDF from your desktop.

That’s it! In only one simple step, you can make many users that like to have a printout very happy.

A Tableau workbook as well as Tableau dashboards can become very overwhelming, depending on how many worksheets are used to create them. Luckily, Tableau helps us with organizational features like `Item hierarchy` and `Used In`.

## Item hierarchy

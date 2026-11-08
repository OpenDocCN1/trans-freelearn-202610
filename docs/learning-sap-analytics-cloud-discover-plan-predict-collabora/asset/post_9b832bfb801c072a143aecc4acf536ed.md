
## Creating formatting rules

The Formatting Rules section under the Styling tab lets you create rules, wherein you specify how to format dimension members and hierarchies. In this exercise, you will create a couple of rules to highlight the net revenue earned by the sales manager Kiran Raj in the three states. Execute the following steps to achieve this task:

40. Select the table, and open the Styling tab under Designer. Click on the Add Formatting Rule link in the Formatting Rules section. On the Formatting Rule panel, enter State Rule (A) for the rule name. Click and hold the first cell showing 50.05 (B) in the Net Revenue column and drag down to select all the three net revenue figures for Kiran Raj. As you select these three cells, relevant information (C) appears under the Selected Content area in the Formatting Rule panel. From the Level list representing location, select Self & Children (D), as shown in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_270_0.png)

The checkboxes in the Selected Content section are provided to select which dimensions you want to apply the rule to and how the rule applies to different members of the dimension. For example, if you select Self from the Level list for the Account dimension, the formatting will be applied to the Account dimension only. The Level list provides a number of options (briefed hereunder) for hierarchical dimensions. For others, it has just two options: Self and All.

- Self: This applies the formatting only to the currently selected members of the hierarchy
- Self & Children: This applies to the currently selected members as well as those one level below them in the hierarchy
- Self & Sibling: This one applies to the selected members as well as to other members at the same hierarchy level
- Self & Descendants: This applies to the selected members and to all the lower levels of the hierarchy
- Children: This applies to one level below each of the currently selected members
- Descendants: This applies to all members below the top level currently selected

41. In the Style section, select New Style to set a new style for the state rule:

![](img/c69568baeee565fe02397867c05883e1_271_0.png)

42. In the Style dialog box, enter StateStyle (E) for the name of the new style. In the Font section, select a font, size, and a different color (F). Click on the letter B (G) to present the data boldfaced. Also, select a light color in the Fill section (H) to highlight the data. Click on OK to close the Style dialog box, and then click on the Apply button in the Formatting Rule panel:

![](img/c69568baeee565fe02397867c05883e1_271_1.png)

The output of the rule is illustrated in the following screenshot. The net revenue values for the three states are highlighted and are also presented in a different color to distinguish them from the rest of the table data:

![](img/c69568baeee565fe02397867c05883e1_272_0.png)

Now, expand the California state’s node. The same rule is applied to the cities under this state, as shown in the following screenshot. This is because you selected Self & Children option in step 40-D.

![](img/c69568baeee565fe02397867c05883e1_272_1.png)

Let's create another rule in which you will set up another style to differentiate cities and states data. Here are the steps for this rule:

43. Select the table, and in the Styling tab, click on the Add Formatting Rule link in the Formatting Rules section. On the Formatting Rule panel, enter City Rule (A) for the rule name. Expand the California state node to view its cities. Click and hold the first cell showing 11.5564644 (B) in the Net Revenue column and drag down to select all the cities in this state (C). Once again, the Selected Content section will be populated with the same dimensions and their members, as you saw in the previous rule. This time, accept all the default values appearing in this section, including the level Self (D), set for the location dimension. By keeping the Self level, you specify to apply the rule to the selected data set, that is, cities in California, as shown in the following screenshot:

![](img/c69568baeee565fe02397867c05883e1_273_0.png)

44. Once again, select New Style in the Style section to set up a new style for the City Rule:

![](img/c69568baeee565fe02397867c05883e1_273_1.png)

45. In the Style dialog box, enter CityStyle (E) for the name of the new style. In the Font section, select a font, size, and a different color (F). Make the text bold by clicking on the letter B (G). Also select a different color in the Fill section (H). Click on OK to close the Style dialog box, and then click on the Apply button in the Formatting Rule panel:

![](img/c69568baeee565fe02397867c05883e1_274_0.png)

Here is the output of the rule in which cities data (I) can easily be distinguished from the state data (J):

![](img/c69568baeee565fe02397867c05883e1_274_1.png)

## Setting visibility filters

In the previous chapters, you went through many techniques to filter the results delivered by your page objects. In those exercises, you applied conventional filtering options to control the display of dimension members. In this section, we will go through another type of filter called **visibility filter**. The main difference between a conventional filter and the visibility filter is that when you apply a conventional filter, the filtered-out members vanish from the scene and do not take part in the calculation, whereas if you apply the visibility filter to some dimension members, they also vanish from the page objects, but they still participate in the calculation. Let's go through an example to elaborate this concept. First, you will add a conventional filter, and then you will apply a visibility filter to observe the difference between the two.

Currently, the value of Net Revenue being displayed in the table object is 320.84 million (A), as illustrated in the following screenshot. Follow the instructions to first create a conventional filter:

![](img/c69568baeee565fe02397867c05883e1_275_0.png)

46. Select the table, and click on the Manage Filter icon (B) representing the Location dimension. In the Selected filters for Location dialog box, place check marks in the Exclude selected members option (C) and Oregon state (D) boxes to exclude this state’s data from the table. Once you click on OK to get back to the page, you will notice that the Oregon state’s data no longer exists in the table, as depicted in the lower view of the following screenshot. Due to this exclusion, the total value of the Net Revenue measure (E) is also affected.

![](img/c69568baeee565fe02397867c05883e1_276_0.png)

Having seen the function of a conventional filter, let’s see how the visibility filter works:

47. Once again, click on the same Manage Filter icon (F) representing the Location dimension. In the Selected filters for Location dialog box, uncheck Exclude selected members (G), and check All Members (H) to include all dimension members to the table. Then, click on the Set to invisible icon (I) next to the Oregon state to make this state’s data invisible. Click on OK to get back to the page. Again, the Oregon state is not shown in the table, but its data contributes to the totals (J). Using the visibility filters, you can restrict your view to the areas you are most interested in without disturbing the overall aggregation:

![](img/c69568baeee565fe02397867c05883e1_276_1.png)

## Swapping the table axis

If you want to see the table results from a different perspective, you are provided with the swapping option, which converts the table rows into columns and columns into rows.

48. Select the table object, and click on the Swap Axis icon (A) in the Builder panel. The Net Revenue measure will be swapped to rows, and the two dimensions (Sales Manager and Location) will appear in the column position:

![](img/c69568baeee565fe02397867c05883e1_277_0.png)

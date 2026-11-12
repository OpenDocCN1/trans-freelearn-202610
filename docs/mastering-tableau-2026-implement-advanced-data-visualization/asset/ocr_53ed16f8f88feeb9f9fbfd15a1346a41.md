Then, I notice that the intersection order is different; for example, Ashland-71st and 71ST AND ASHLAND. I might consider restructuring the datasets and creating a loop that would put the two streets in alphabetical order in the two splits, but I don’t have time for this now.

Another solution is to first join split 1 and split 1 as well as split 2 and split 2. In a second join, I could join split 1 and split 2 as well as split 2 and split 1. Afterward, I could union the two joins and create an output file (or directly load the Prep Builder dataset into Tableau Desktop). With this approach, I would still not include all the data, but I would have 380,000 rows out of 444,000. This could be enough to get a rough idea of patterns. If I have any time left, I can continue mapping the remaining mismatches.

However, I instead decide to drop the locations dataset altogether and just use the violations dataset because I realize that I would only miss locations in which no violation has happened yet. The locations of violations are sufficient for now. If you were, however, to determine if a traffic light control stopped working, you have to use both datasets.

# Building a map of intersections

I continue opening the violations dataset in Tableau:

+   1. Longitude and latitude values are not automatically recognized, so I have to change both to Number (decimal) by clicking on the data type icon:

![](bbox=[0.39, 0.46, 0.72, 0.81])

Figure 11.37: Change data type
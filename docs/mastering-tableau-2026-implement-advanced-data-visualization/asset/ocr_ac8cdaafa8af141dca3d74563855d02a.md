![](bbox=[0.08, 0.08, 0.86, 0.51])

# Figure 4.48: Dashboard

However, what happens if you need to find out how many people were involved in a type of event per day?

This question is rather difficult to answer using the current data structure because we have one row per patient with multiple dates in that row. In Figure 4.49, you can see the difference: the right-hand side is our current data structure, and the left-hand side is the data structure that will make it easier to count events per day:

| Patient | Event | Date | | Patient | Event A Date | Event B Date | Event C Date |
|---|---|---|---|---|---|---|---|
| U | A | X | | U | X | Y | Z |
| U | B | Y | | V | Y | X | |
| U | C | Z | | | | | |
| V | A | Y | | | | | |
| V | B | X | | | | | |

# Figure 4.49: Patient data structure
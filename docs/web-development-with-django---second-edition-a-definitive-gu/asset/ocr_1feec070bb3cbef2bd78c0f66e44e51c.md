You can see how long each query took to execute and in what order they were executed. It also flags similar and duplicate queries so that you can potentially refactor your code to remove them.

Each SELECT query displays two action buttons, Sel, short for select, and Expl, short for explain. These do not show up for INSERT, UDPATE, or DELETE queries.

The Sel button shows the SELECT statement that was executed and all the data that was retrieved for the query (Figure 15.16):

![](bbox=[0.125, 0.215, 0.835, 0.595])

Figure 15.16 – The DjDT SQL selected panel

The Expl button shows the EXPLAIN query for the SELECT query (Figure 15.17):
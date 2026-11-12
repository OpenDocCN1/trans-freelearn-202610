At this point, you can generate a dataset based on how often each location is occupied. For example, you might note that for one game, position 81 was occupied for 40 moves, in which case the player may simply have never moved that rook. However, in another game the position may be occupied for only 10 moves, perhaps indicating the player performed a castle early in the game. The point of layering the chess game and the square file was to identify which square belongs to which field on a chess board.

# Visualizing a chess game

In this exercise, we will take a look at a dataset generated from a chess game and discover how to visualize the results based on the work done in the previous exercises. In order to follow the steps, you will need to download the assets associated with this chapter. To do so, simply follow the link to the GitHub repository for this book: https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter08.

The following are the steps:

+   1. Within the workbook associated with this chapter, navigate to the worksheet entitled Chessboard and select the Chessboard data source
+   
+   2. Right-click on the Chessboard data source and select Edit Data Source in order to examine the dataset
+   
+   3. In the dialog box asking Where is the data file?, steer to the Chessboard.xlsx file provided with the assets associated with this chapter
+   
+   4. The table named **Board Grid** contains each location code and the X and Y coordinates associated with each location code. This dataset was taken from the **Location Codes** worksheet created earlier in this chapter. The table named **Squares Occupied** contains the fields **Location Code** and **Moves Occupied**. **Board Grid** and **Squares Occupied** are connected using a left join on **Location Code**

![](bbox=[0.08, 0.532, 0.87, 0.775])

Figure 8.36: Join
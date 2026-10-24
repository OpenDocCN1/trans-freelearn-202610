
## Filtering

The Filter feature enables you to focus on data that you wish to see by temporarily hiding rows that aren’t relevant for a given search. For instance, let’s say you only want to see bridges located in China:

1. Click on the Filter button option in cell D4 of the Feature Comparison worksheet.

2. Clear Select All, choose China, and then click on OK.

> **Tip**
> A faster way to filter is to type one or more characters into the Search field and then press Enter when only the desired item(s) appear on the list.

3. Excel hides all rows in the Table except when China is in column D, as shown in Figure 7.13:

![](img/0c7a357df7e57e06b6d9f631c219040c_230_0.png)

Figure 7.19 – The filtered list

4. Choose Data | Clear (or Home | Sort & Filter | Clear) to display all rows in the Table again.

> **Tip**
> Also, you can choose Clear from the drop-down list in cell D3. Keyboard shortcut aficionados can press Alt + Down (Option + Down) in cell D3, use the down arrow to select the Clear command, and then press Enter. In a similar vein, the spacebar toggles checkboxes off or on within the drop-down menu.

As with a normal range of cells, you can filter a **Table** on as many columns as you want. Now, let’s see how to automate filtering specific fields with **Slicers**.

## Slicers

**Slicers** are objects that float on the worksheet and function as remote controls for the **Filter** drop-down menus. You can add one or more **Slicers** to a **Table** or **PivotTable**, but let’s keep our focus on **Tables**:

1. Click on any cell in the **Table** on the **Feature Comparison** worksheet and then choose **Table Design | Insert Slicer**.

> **Tip**
> Additionally, you can click on any cell within a **Table** or **PivotTable** and choose **Insert | Slicer**.

2. Choose the field(s) that you wish to filter, such as **Country** and **Type of Bridge**, and then click on **OK**.

3. A **Slicer** appears for each field that you chose, as shown in **Figure 7.20**:

![](img/0c7a357df7e57e06b6d9f631c219040c_231_0.png)

**Figure 7.20 – Slicers**

## Automating Tasks with the Table Feature

4. Choose **China** from the **Country Slicer** and **Metro** from the **Type of Bridge Slicer** to filter the list, as shown in **Figure 7.21**:

![](img/0c7a357df7e57e06b6d9f631c219040c_232_0.png)

Figure 7.21 – A list filtered by Slicers

5. Click on the **Clear Filter** button in the upper-right corner of a **Slicer** to clear your selection and show all items again; alternatively, you can choose **Data | Clear**.

6. Notice how slicing enables you to collapse a list with a single click. Let’s say that you wanted to see bridges located in China or the United States. To do so, simply hold down _Ctrl (%_) while you select the items you wish to see.

> **Tip**
> You can also select a group of items within a **Slicer** by holding down your left mouse button while you drag down the **Slicer**. Or you can click on the **Multi-Select** button in the **Slicer** and click on items you don’t want to be visible in the **Table**.

To move a **Slicer** a short distance on screen, grab the margin of the object with your left mouse button and reposition as desired. To move a **Slicer** a longer distance, say from the bottom of a long list back to near the top, right-click on the **Slicer** and choose **Cut**. Then, paste the **Slicer** in the desired location. **Slicers** can be moved anywhere on a worksheet but cannot leave the worksheet.

A **Slicer** tab, as shown in **Figure 7.22**, appears in Excel’s **Ribbon** when you have selected a **Slicer**, such as by clicking inside the margin of the object. The **Columns** field enables you to specify the number of columns that you wish a **Slicer** to have:

**Figure 7.22 – The Slicer tab**

The Slicer | Slicer Settings command displays the Slicer Settings dialog box, as shown in Figure 7.23, which you can use to control the caption, sorting, and filtering options:

**Figure 7.23 – The Slicer Settings dialog box**

> **Tip**
> Slicers work with PivotTables in the same fashion as Tables. Choose PivotTable Analyze | Insert Slicer (or Insert | Slicer). To control two or more PivotTables at once, right-click on a Slicer and choose Report Connections, select the PivotTables to link, and then click on OK. This technique is only available for PivotTables that are based on the same data source.

You have a couple of options if you decide you no longer need a Slicer:

- Right-click on a Slicer and choose Cut (or Remove).
- Left-click on the edge of a Slicer and press Delete.
- Convert the Table back into a normal range of cells. Slicers can only be used with Tables and PivotTables, so Excel automatically removes any associated Slicers when a Table is converted into a normal range of cells.

## PivotTable integrity improvements

PivotTables are a report writing feature in Excel that allows you to instantly transform lists of data into meaningful reports. Let’s begin with a brief overview of how to create a PivotTable, and then I will explain how the Table feature can improve the integrity of a PivotTable:

1. Click on any cell within the list that starts in cell A3 of the PivotTable Data worksheet, and then choose Insert | Recommended PivotTables.

2. As shown in Figure 7.24, select Sum of Feet by Country and then click on OK:

![](img/0c7a357df7e57e06b6d9f631c219040c_234_0.png)

Figure 7.24 – The Recommended PivotTables dialog box

> **Tip**
> To temporarily hide Slicers, choose Home | Find & Select | Selection Pane, and then click on the icon on the right-hand side of a Slicer in the list to hide the object. Click on the icon again to display the Slicer again.

Now, let’s see how Tables can improve the integrity of PivotTables.

### Nuance

**Recommended PivotTables in Excel for macOS** unilaterally creates a **PivotTable** without giving you any choices.

3. As shown in **Figure 7.25**, a **PivotTable** appears on a new worksheet:

Figure 7.25 – The PivotTable report

4. Right-click on any cell within the **Sum of Feet** column, choose **Number Format**, choose a number format of your choice, and then click on **OK**.

5. Double-click on the new worksheet tab and assign the name **PivotTable Report**. Then, press **Enter**.

> **Tip**
> The **Number Format** command is only available when you right-click on a **PivotTable**. It allows you to format an entire column of a **PivotTable** without selecting any cells.

6. Select cells **G4:K13** on the **PivotTable Data** worksheet and press **Ctrl + C (**⌘**+ C)**. Alternatively, you can choose **Home | Copy**.

7. Select cell **C14** and then press **Enter**, or choose **Home | Paste**.

8. Activate the **PivotTable Report** worksheet, and choose **PivotTable Analyze | Refresh**.

![](img/0c7a357df7e57e06b6d9f631c219040c_235_0.png)

> **Tip**
> You can also right-click on a PivotTable and choose Refresh or press Alt + F5 in Excel for Windows.

9. Brunei Darussalam, Indonesia, Kuwait, and Peru do not appear because PivotTables based on a normal range of cells cannot automatically resize themselves. To manually resize a worksheet-based PivotTable, perform the following steps:

   I. Choose PivotTable Analyze | Change Data Source.

   II. Select cells A3: E23, as shown in Figure 7.26, and then click on OK, which will also refresh the PivotTable and cause the new countries to appear:

   **Figure 7.26 – The Change PivotTable Data Source dialog box**

   > **Nuance**
   > The initial PivotTable alphabetized the country names; however, sometimes, resized PivotTables do not sort automatically. In this case, right-click on any country, choose Sort, and then Sort A to Z to alphabetize the list.

10. Now that you know how to manually resize a PivotTable, let’s use the Table feature to eliminate the repetitive task:

   I. Activate the PivotTable Data worksheet.

   II. Select any cell within cells A3:A23, choose Insert | Table, and then click on OK.

   III. Copy the data from cells M4:Q8, and paste it into cell A24.

![](img/0c7a357df7e57e06b6d9f631c219040c_236_0.png)

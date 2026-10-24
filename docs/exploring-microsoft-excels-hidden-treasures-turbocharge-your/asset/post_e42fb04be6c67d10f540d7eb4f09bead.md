

12. Right-click on **Removed Top Rows**, choose **Rename**, and then type Removed Top 3 Rows and press **Enter**.

**Tip**
Notice the **Gear Icon** next to the **Removed Top 3 Rows** (or **Removed Top Rows**) step. You can click this icon to redisplay the Remove Top Rows dialog box if you want to adjust the number of rows removed.

13. Choose **Home | Use First Row as Headers** to replace the generic **Data Preview** column headings with the titles in the first row.

14. **Changed Type** step appears in the **Applied Steps** list because **Power Query** changed the data type for one or more of your columns. Sometimes this is helpful, sometimes it isn’t. Fortunately, you can determine which columns changed:

+   I. Click on the **Promoted Headers** step, which causes the **Data Preview** window to present how your data looked _before_ the **Changed Type** step was applied.

+   II. Notice the ABC123 icon adjacent to the word **Unit** in the header row, which indicates that the column contains text and/or numbers. This is also referred to as the **Any** data type in **Power Query**, which means a column could contain text, numbers, or both types of data.

+   III. Click on the **Changed Type** step and notice that **123** now appears next to the word **Unit**, which indicates that the column has been transformed into whole numbers.

+   IV. If you continue clicking back and forth between **Promoted Headers** and **Changed Type** as you work your way across, you’ll notice other column type changes as well.

15. Choose **Home | Choose Columns** (if you clicked the bottom half of the command, then click **Choose Columns** a second time).

16. Type **Col** in the **Choose the columns to keep** field to select all blank columns at once.

17. Clear the **(Select All Search Results)** checkbox and then click on **OK**.

**Nuance**
You can easily modify a step like this later by clicking on the **Gear** icon and updating your choices. You have two alternatives if you need to update a step that doesn’t have a **Gear Icon**: remove the step and add it back again or edit the **M** code in the **Formula Bar** area. **Power Query** uses a programming language called **M** behind the scenes that the **Power Query Editor** mostly shields you from, but also makes available when you’re ready.

18. Change the **Name** of the the **Removed Other Columns** step to **Removed Blank Columns**.

## Power Query

19. Select the **Tenant** column, and then choose **Transform** | **Fill** | **Down**, shown in **Figure 12.12**, to populate the tenant **Name** on rows where it does not appear:

![](img/0c7a357df7e57e06b6d9f631c219040c_400_0.png)

# Figure 12.11 – The Fill | Down command

> Nuance
> The Fill command only works when the word null appears in one or more cells in a column.
> If the cells you wish to fill are blank, select the column and then choose Transform | Replace
> Values. Leave the Value To Find field blank, type null in the Replace With field, and then
> click on OK. In this context, the word null must be all lowercase. You can then use Transform
> | Fill | Down to populate the missing values.

20. Repeat Step 24 to fill the Acc# column down in the same fashion.
21. Rename Filled Down as Filled Tenant Name/Acc# Down in the Applied Steps list.

> Nuance
> Notice that Power Query only added one Filled Down step to the Applied Steps list. Consecutive
> versions of the same task are automatically merged into a single step. In such situations, you
> cannot remove the task from one column without removing it from the other columns as well.
> The only way around this is to carry out a different task before then carrying out the task that
> would get merged with the initial instance.

22. Click the Filter arrow in the Date column, clear the Null checkbox, and then click OK to
remove the rows that do not contain transactional data.

> Nuance
> Fields in Power Query can be labeled as null, or they can be entirely blank. In this case, any
> row that doesn’t contain a date is a row that we want to eliminate. Identifying such patterns
> within your data helps make transformations painless.

23. Rename Filtered Rows as Filtered Null Date Rows in the Applied Steps list.

24. Select the **0-30** column, and then hold down the *Shift* key and select the **Total** column with your mouse. Alternatively, click on **0-30**, hold down the *Shift* key, and use the right-arrow key to select the aging columns through **Total**, and then choose the **Transform | Data Type: Any | Currency** options to convert the aging columns into currency values.

**Nuance**
Setting a column to **Currency** in **Power Query** does not apply the **Currency** number format to the cells in Excel, so you’ll still need to format the worksheet cells.

25. Rename the **Changed Type** command *Changed Amount Fields to $*.

26. Choose **Home | Close & Load** to return the data to Microsoft Excel.

27. Notice that the **F** column may show the dates in serial number format:

![](img/0c7a357df7e57e06b6d9f631c219040c_401_0.png)

**Figure 12.12 – The F column may show serial numbers instead of dates**

If this occurs, you could manually format these dates in Excel, or you can make the change in **Power Query**:

+   1. Choose any cell within **Table** and choose **Query | Edit**.

+   2. Click the **ABC123** icon in the **Date** column header and choose **Date** from the list, as an alternative to choosing **Transform | Data Type: Any | Date**.

+   3. Right-click and rename the **Changed Amount Field to $** caption in the **Applied Steps** to needs data entry font since the step now includes two consecutive transformations of the same type.

+   4. Choose **Home | Close & Load** to return the data to Excel.

**Nuance**
You won’t always be able to tell when you need to change a data type, such as for the **Date** column. In this case, the Data Preview tricked me by showing dates instead of serial numbers.

## Setting a locale or region

I live in Atlanta, which means that in 2024, the *Ides of March* will fall on 03/15/2024 for me, but  
perhaps on 15/03/2024 for you. Similarly, data that you import into **Power Query** may reflect the  
locale where the data originated, or the locale of the last person that saved the file. **Power Query**  
refers to world regions as **locales**. Locales affect how dates, times, and numbers are formatted. Let’s  
see how you can change a locale within your operating system, for a workbook or text file, and for  
columns within a query:

+   - **Operating System**: Power Query defaults to the **Regional Settings** preferences in Windows  
+     and the **Region and Language** settings in Excel for macOS. To change these settings:
+   
+     - **Windows**: Choose Start | Settings | Time & Language | Region and make a choice from  
+       the **Regional format** list and then close the **Settings** window.
+   
+     - **Mac**: | **System Preferences** | **Language & Region**, make a choice from the **Region** list, and  
+       then close the **Language & Region** dialog box.
+   
+   - **Workbook**: In Excel for Windows, you can override the operating system’s **Regional Settings**  
+     by specifying a locale for a workbook: Choose **Data** | **Get Data** | **Query Options** | **Regional**  
+     **Settings**, make a choice from the **Locale** list, and then click **OK**.
+   
+   - **Text file**: You can set the locale for text files during the import process or at the query level,  
+     which will also override the operating system’s settings:
+   
+     - **At import**: The steps differ between platforms:
+   
+       - **Excel for Windows**: Choose **Data** | **From Text/CSV**, change the **File Origin** field, and  
+         then choose **Load** or **Transform Data**.
+   
+       - **Excel for macOS**: Choose **Data** | **Get Data (Power Query)** | **Text/CSV**, click **Browse**  
+         and select a file, and then click **Next**. Change the **File Origin** and then click **Load** or  
+         **Transform Data**
+   
+       - **Query settings**: Click the **Gear Icon** for the **Source** step in the **Applied Steps** list of the  
+         **Query Settings** task pane, update the **File Origin** field, and then click **OK**.
+   
+   - **Change Type action**: You can override both the **Operating System** and **Power Query** by  
+     specifying a locale on a column-by-column basis:
+   
+     I. Click the **Data Type** button in the **Header** field of a query column and choose **Using**  
+        **Locale**, (or right-click on the **Header** and choose **Change Type** | **Using Locale**) to display  
+        the **Change Type with Locale** dialog box shown in **Figure 12.14**:


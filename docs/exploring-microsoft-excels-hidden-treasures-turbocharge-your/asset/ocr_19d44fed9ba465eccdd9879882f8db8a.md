# # Power Query

> > **Tip**
> > If you need to rename multiple columns in a query, click on the first column and then press _F2_
> > (_Fn + F2_ on some keyboards), type the new field **Name**, and press _Enter_. Use the left or right
> > arrow keys to move to the next column, and then press _F2_ to rename the next field.

+   14. Right-click on **Vendor Name** and then choose **Move | To Beginning** to move the **Vendor**
+   **Name** field to the first column position in the query (or drag the column into a new position
+   with your mouse).

+   15. Click on the **ID** field and press _Delete_, and then click on the **Vendor** field and press _Delete_ to
+   eliminate two extraneous fields from the _Fruit Sales_ query.

> > **Tip**
> > Deleting columns in this fashion only removes the fields from the results. If you click on
> > the _Chapter 12 - January Transactions_ or _Chapter 12 - February_
> > _Transactions_ queries, you'll see that the **ID** and **Vendor** columns are still in place.

+   16. Click **Home | New Source | File | Text/CSV**, select the _Chapter 12 - Products.csv_
+   file, click **Import**, and then click **OK**.

+   17. Now, let's merge the **Product Names** into the _Fruit Sales_ query:

+   I. Confirm that the **Product** field in the **Transactions Table** has the same data type as the
+   **ID** field in the **Products Table**, as discussed in _Step 9_.

+   II. Select the _Fruit Sales_ query from the **Query** list.

+   III. Choose **Home | Merge Queries** and click on the **Product** column from the **Fruit Sales**.

+   IV. Choose _Chapter 12 - Products_ from the second list and select the **ID** column in the
+   **Chapter 12 - Products** list.

+   V. Click **OK** to close the **Merge** dialog box.

+   18. Click the double-headed arrow in the header of the _Chapter 12 - Products_ column.

+   19. Clear the checkbox for **ID** so as not to duplicate the existing product ID column, and then
+   click **OK**.

> > **Tip**
> > **Use original column Name as prefix** will already be cleared if you chose to do so earlier in
> > this sequence.

+   20. Double-click on the **Product.1** header, enter `Product Name`, and then press _Enter_.
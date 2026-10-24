# How to do it...

+   1. Load `datetime-example.shp`.
+   2. Open the attribute table of `datetime-example`.
+   3. Create a new field. As this is a shapefile, we'll need to use a String of length 30 (or you can use the empty field called `calculated`).
+   4. Turn on layer editing (this is the pencil icon, which is the first icon to the left of the window toolbar).
+   5. From the drop-down list, select **Calculated**.
+   6. Now press the **Calculation** button.

> In older versions of QGIS, you'll need to open the Field calculator, which also works in newer versions but has slightly more steps.

7. In the calculator, we'll use the `substr` String operation (that is, Substring) in combination with the `||` concatenation to rearrange the values from existing fields into a valid `DateTime`:

+   1. The simplest variant is just to combine `shpDate` with `Time` and put a space in between:
+   
+      `"shpdate" || ' ' || "Time"`

+   2. For a bit more of a challenge, use `Date`:
+   
+      `substr("Date",7,4) || '-' || substr("Date",1,2) || '-' || substr("Date",4,2) || ' ' || "Time"`

> Note the use of single quotes (') to denote a string as opposed to double quotes (") , which indicate a field name.
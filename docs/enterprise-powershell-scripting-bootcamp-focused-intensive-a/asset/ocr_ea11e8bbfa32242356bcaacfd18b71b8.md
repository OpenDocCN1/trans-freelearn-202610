The output of this is shown in the following screenshot:

![](bbox=[0.07, 0.07, 0.9, 0.125])

The generated merged CSV file is shown in the following screenshot:

![](bbox=[0.07, 0.19, 0.9, 0.6])

In this example, you merge all of the created CSV files into a single CSV file:

+   1. You first start by defining the log location by declaring "C:\temp\POSHScript\CSVDEMO\" and placing it in the $logloc variable.
+   2. You then continue to generate the date timestamp by using the Get-Date cmdlet with the -format parameter set to the "yyyyMMddhhmmss" argument.
+   3. You store this value in the $date variable.
+   4. You build the merge file string by placing "$logloc" + "Merged_$date.csv" into the $mergefile variable.
+   5. You then create the merge file by leveraging the new-item cmdlet with the $mergefile argument, and the -ItemType parameter set to File.
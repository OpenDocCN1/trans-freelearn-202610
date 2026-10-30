# Resources required

Data sources that are available in your TCRM environment will be used for the purpose of this exercise. Please use the datasets found under the My First Analytics App app so that the results will be consistent with the following steps:

+   - Opportunities
+   - Cases
+   - Activities
+   - Accounts
+   - Users

# Steps to follow

Build a new recipe in your demo TCRM environment and follow these steps:

+   1. Select Opportunities, Accounts, Activities, Cases, and Users for your recipe input data. The panel on the right allows the selection of columns, and although you can proceed by selecting all columns, ideally you would only want to work with the columns required.
+   2. Add filters that are required as per the exercise objectives.
+   3. Add some aggregation nodes as you know that Opportunities, Cases, and Activities have a lower granularity than Accounts. As such, you will need to aggregate all of them to the same granularity (Accounts) so that you are able to perform joins in the later steps to meet the objectives.
+   4. Your aggregation on the Opportunities data input will split the opportunity and expected amounts into two columns—with line items and without, as that was a requirement. However, this also means that you do not have a field that has the total amount and total expected amount. Create a Transform node that helps you bring the totals back into the dataset build.
+   5. Make the necessary joins so that you can work toward your final dataset.
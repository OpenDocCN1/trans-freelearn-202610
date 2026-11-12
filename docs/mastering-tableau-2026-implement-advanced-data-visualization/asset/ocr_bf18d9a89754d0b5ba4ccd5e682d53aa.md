| Customer Name | Day of Order Date | %Total Sales along Table (Down) | Sales |
|---|---|---|---|
| Chuck Clark | 30 December 2025 | 0.25% | 209.3 |
| Erica Bern | 30 December 2025 | 0.04% | 34.6 |
| Harry Olson | 30 December 2025 | 0.00% | 3.0 |
| Jill Matthias | 30 December 2025 | 0.00% | 3.0 |
| Patrick O'Donnell | 30 December 2025 | 0.55% | 466.8 |

Figure 5.2: Total Sales

Let's consider how the preceding worksheet functions:

+   - The filter on **Order Date** ensures that the dataset returned to Tableau only includes data from the month of December 2025.
+   - The % **Total Sales** SUM([Sales])/TOTAL(SUM([Sales]))) includes the TOTAL(SUM([Sales])) calculated field, which returns the total sales for the entire dataset. Dividing SUM([Sales]) by this total returns the percentage of the total.
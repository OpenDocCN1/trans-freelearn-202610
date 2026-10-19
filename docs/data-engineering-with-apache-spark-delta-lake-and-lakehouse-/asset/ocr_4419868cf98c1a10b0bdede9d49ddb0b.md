Note the country data in e-commerce transactions now exists in an abbreviated form:

# Verify country in ecommerce transactions

```
%sql
select customer_name,country from esalesns
```

(2) Spark Jobs

|  | customer_name | country |
|---|---|---|
| 1 | Vera Russell | UK |
| 2 | Brynn Nelson | USA |
| 3 | Ignacia Price | UK |
| 4 | Beatrice Tucker | UK |
| 5 | Kenyon Daugherty | USA |
| 6 | Linda Pugh | CAN |
| 7 | Brenna Robles | CAN |

Figure 7.49 – Verifying the uniform country data in e-commerce transactions

Previously, we also noticed that transactions in store_orders compared to e-commerce are done in varying currencies – USD, CAD, EUR, and INR. Electroniz had expressed a desire to covert all transactions to a common currency – USD.

In the cell titled Define Functions, notice the function to perform the appropriate currency conversion. Notice this function is using a currency conversion factor that converts the sale from the native currency to USD. You may recall that we fetch the currency conversion factory every hour as part of the Electroniz ingestion pipeline and store it in the currency table.

```
def curate_sales_price(currency, currency_value, sales_price):
    if (currency != 'USD'):
        curated_value = float(sales_price)/float(currency_value)
        return float(curated_value)
    else:
        return float(sales_price)
```
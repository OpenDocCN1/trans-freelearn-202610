

## 验证重复数据

你可能还记得 `electroniz_batch_ingestion_pipeline` 不仅摄取了源中新增的数据，还摄取了随时间发生变化的数据。在上一章 *理解 Delta Lake* 中，我们学习了如何将新数据和变更数据合并到一个公共的 delta 表中，以建立单一事实来源。

即使数据会随时间演变和变化，最终用户只关心查询最新版本。我们需要非常清楚地理解这个概念，所以让我用以下客户姓名变更的事件序列来演示：

- 1. 一个新客户 `Prescott Barnes` 于 2021 年 4 月 4 日 17:56 在 `stores` 数据库中创建。
- 2. 该客户记录在 2021 年 4 月 4 日 18:00 运行的 Electroniz lakehouse 中被摄取。
- 3. 该客户于 2021 年 4 月 4 日 18:30 请求将姓名更改为 `Prescott Barnes Jr.`。
- 4. 该客户记录在 2021 年 4 月 4 日 19:00 运行的 Electroniz lakehouse 中被重新摄取。

现在我们在 lakehouse 的 bronze 层中为同一客户有两个记录 – *重复项*。嗯，这是一个问题，不是吗？

幸运的是，Delta Lake 的魔力使我们能够轻松处理此类问题并对数据进行去重。在标题为 **Curate Sales Data - standardize, mask and merge** 的单元格中，注意以下 Delta Lake 的 merge 子句：

```
deltaTable.alias("store_customers").merge(
df_table_curated.alias("store_customers_new"),
"store_customers.email = store_customers_new.email") \
.whenMatchedUpdate(set = {"customer_id": "store_customers_new.customer_id", \
"customer_name": "store_customers_new.customer_name", \
"address": "store_customers_new.address", \
"city": "store_customers_new.city", \
"postalcode": "store_customers_new.postalcode", \
"country": "store_customers_new.country", \
"phone": "store_customers_new.phone", \
"email": "store_customers_new.email", \
"credit_card": "store_customers_new.credit_card", \
"updated_at": "store_customers_new.updated_at" } )
.whenNotMatchedInsert(values = {
"customer_id": "store_customers_new.customer_id", \
"customer_name": "store_customers_new.customer_name", \
"address": "store_customers_new.address",
```

Figure 7.52 – Delta Lake merge of store_customers

这意味着如果客户记录在键上匹配（email），只需通过用新版本替换来更新现有客户数据。因此，如果 Delta Lake merge 过程工作正确，我们应该在 silver 层中看到该客户最新的姓名变更：

### 验证客户姓名合并

```
%sql
SELECT customer_name FROM store_customers WHERE email = 'phasellus.vitae@vitae.co.uk'
```

(2) Spark Jobs

|  | customer_name |
|---|---|
| 1 | Prescott Barnes Jr. |

Figure 7.53 – Result of the Delta Lake merge of store_customers

看起来客户姓名合并过程工作完美。

让我们用另一个示例验证相同的过程。你可能还记得有五笔订单的价格被调整。我们应该检查 silver 层中 delta 表的最新版本是否反映了调整后的价格。在标题为 Curate Sales Data - standardize, mask and merge 的单元格中，注意以下 Delta Lake 的 merge 子句：

```
deltaTable.alias("store_orders").merge(
df_table_curated.alias("store_orders_new"),
"store_orders.order_number = store_orders_new.order_number"
whenMatchedUpdate(set = {"order_number": "store_orders_new.order_number", "customer_id": "store_orders_new.customer_id", "product_id": "store_orders_new.product_id", "order_date": "store_orders_new.order_date", "units": "store_orders_new.units", "sale_price": "store_orders_new.sale_price", "sale_price_usd": "store_orders_new.sale_price_usd", "currency": "store_orders_new.currency", "order_mode": "store_orders_new.order_mode", "updated_at": "store_orders_new.updated_at" })
whenNotMatchedInsert(values = {
"order_number": "store_orders_new.order_number",
"customer_id": "store_orders_new.customer_id",
"product_id": "store_orders_new.product_id",
```

Figure 7.54 – Delta Lake merge of store_orders

如果 Delta Lake merge 过程工作正确，我们现在应该在 silver 层中看到这些订单的最新价格调整：

### 验证 store orders 价格调整

```
%sql
SELECT order_number, order_date,sale_price FROM store_orders WHERE order_number in (500, 1254, 1501, 2234, 2345)
```

|  | order_number | order_date | sale_price |
|---|---|---|---|
| 1 | 1254 | 2016-07-26 | 49.78 |
| 2 | 2234 | 2016-03-29 | 34.4 |
| 3 | 500 | 2016-02-16 | 93.77 |

Figure 7.55 – Result of the Delta Lake merge of store_orders

确实，价格已按预期进行了调整。但为什么表格中缺少几笔订单？这是因为这些订单已从 stores 数据库中删除。我们在 electroniz_batch_ingestion_pipeline 的 hour=21 运行中摄取了这两条已删除的记录。以下代码用于删除这些订单：

```
deltaTable.delete("order_mode = 'DELETE'")
```

Figure 7.56 – Delta Lake deletion of store_orders

### 重要说明

你可以通过提出一个简单问题来进一步理解因变更数据而产生的重复问题：最终用户想要查询什么？毫无疑问，只有最新版本的数据。幸运的是，Delta Lake 允许你存储、检索和恢复数据之前的版本，但默认情况下，它只提供最新版本。

## 验证不安全数据

最后，我们想要验证 silver 层中的数据是否符合安全标准。在发现阶段，一个核心安全要求是对所有 PII 数据进行掩码。在标题为 Define functions 的单元格中，注意以下用于掩码 PII 列的函数：

```
def mask_value(column):
    mask_value = hashlib.sha256(column.encode()).hexdigest()
    return mask_value
```

重要说明
在前面的示例中，我们使用了哈希来掩码值。哈希是一种单向函数，这意味着你无法从掩码值中检索实际数据。

在标题为 Curate Sales Data - standardize, mask and merge 的单元格中，我们使用相同的函数对所有 PII 列进行掩码，如下所示：

```
if table=="store_customers":
  df_table_incremental = spark.read.csv(bronze_table_path, schema=schema_stores )
  #display(df_table_incremental)
  df_table_curated = df_table_incremental.withColumn('email_curated',curate_email_udf('email')).drop('email').withColumnRenamed('email_curated', 'email')
  df_table_curated = df_table_curated.withColumn('country_curated',curate_country_udf('country')).drop('country').withColumnRenamed('country_curated', 'country')
  df_table_curated = df_table_curated.withColumn('phone_masked',mask_udf('phone')).drop('phone').withColumnRenamed('phone_masked', 'phone')
  df_table_curated = df_table_curated.withColumn('credit_card_masked',mask_udf('credit_card')).drop('credit_card').withColumnRenamed('credit_card_masked', 'credit_card')
  df_table_curated = df_table_curated.withColumn('address_masked',mask_udf('address')).drop('address').withColumnRenamed('address_masked', 'address')
```

Figure 7.57 – Masking data for PII columns

如果掩码工作正确，所有 PII 列在 silver 层中都应被正确掩码：

Verify Data Masking

```
select customer_name, address, phone, credit_card from store_customers
```

| customer_name | address | phone | credit_card |
|---|---|---|---|
| 1 Hannah Ferrol | 658076... | 7k... | 049... |
| 7 May Bentley | 0500... | 000... | 0c6... |
| 3 Nellie Frost | 235... | 1e... | 51... |
| 4 Ayanne Morton | 0358... | 1e... | 5b... |
| 5 Faith Hooper | c411... | 727... | 627... |
| 6 Sage Bauer | a42... | fbc... | a0f... |
| 7 Lionel Dillon | 940... | 171... | 205... |

Figure 7.58 – Verifying the results of masking data for PII columns

重要说明
掩码数据的另一种方法是加密存储时 PII 列。使用这种方法，你可以根据访问数据的用户的<|message|>

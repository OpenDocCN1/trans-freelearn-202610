

这导致以下输出：

```
%sql
DESCRIBE HISTORY store_orders;
```

| version | timestamp | userid | userName | operation |
|---|---|---|---|---|
| 3 | 2021-05-04T15:09:30.000+0000 | 2160993539918818 |  | RESTORE |
| 2 | 2021-05-04T15:09:14.000+0000 | 2160993539918818 |  | DELETE |
| 1 | 2021-05-04T15:09:09.000+0000 | 2160993539918818 |  | UPDATE |
| 0 | 2021-05-04T15:09:03.000+0000 | 2160993539918818 |  | CREATE TABLE AS SELECT |

显示全部 4 行。

图 6.29 – 还原操作后 sales_orders 表的交易历史

请注意，表的版本 3 显示 operation=RESTORE，表明表已还原到之前的状态。

在计划的摄取过程中，数据会反复插入和更新。Delta Lake 提供了使用 upserts 在现有表中合并数据的一种便捷方式。

## 执行数据的 upsert

Delta Lake 还支持使用 MERGE 子句的 upsert 操作。upsert 的思想很简单，即将现有表中的数据与源产生的新数据合并。在湖仓中，将增量数据摄取并与 Delta Lake 中预先存在的表合并是很常见的。每次摄取新数据时都会重复 MERGE 过程，并且对于在湖仓的银层中创建数据的单一事实至关重要。

> 重要说明
> UPSERT 操作是同一操作中的更新和插入的混合。使用 MERGE 子句，如果记录在表中不存在则可以执行插入，或者如果记录已存在则简单地更新现有数据。

- 1. 在开始 MERGE 过程之前，让我们先了解 store_orders 表中当前的行数：

```
%sql
SELECT count(*) FROM store_orders;
```

这导致以下输出：

```
%sql
SELECT count(*) FROM store_orders;
```

图 6.30 – 检查 sales_orders 中的行数

- 2. 现在，我们将读取 electroniz_batch_ingestion_pipeline 运行的第二次迭代中增量摄取的数据。例如，当我们在第 5 章《数据收集阶段 – 青铜层》中运行管道时，2021/04/28/19 文件夹结构存储了第二次运行的增量数据。此文件夹包含 sales_orders 表中 200 行新数据，这些数据准备好要合并到 delta 表中：

```
ADLS_FOLDER="2021/04/28/19"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/\[dbo\].\["+TABLE+"\]/"+ADLS_FOLDER
df_read_data_hour2 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema )
display(df_read_data_hour2)
```

这导致以下输出：

```
ADLS_FOLDER="2021/04/28/19"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/\[dbo\].\["+TABLE+"\]/"+ADLS_FOLDER
df_read_data_hour2 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema )
display(df_read_data_hour2)
```

| order_number | customer_id | product_id | order_date | units | sale_price | currency | order_mode | updated_at |
|---|---|---|---|---|---|---|---|---|
| 2501 | 568 | 3 | 04/05/2020 | 10 | 68.07 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2502 | 825 | 4 | 03/29/2017 | 7 | 50.44 | INR | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2503 | 616 | 10 | 07/27/2019 | 6 | 82.61 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2504 | 606 | 10 | 12/05/2018 | 9 | 77.29 | EURO | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2505 | 550 | 2 | 06/30/2020 | 8 | 98.87 | INR | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2506 | 848 | 3 | 10/28/2018 | 2 | 41.32 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2507 | 810 | 8 | 11/08/2020 | 7 | 89.12 | EURO | EDIT | 2021-04-28T18:25:41.120+0000 |

显示全部 200 行。

图 6.31 – electroniz_batch_ingestion_pipeline 第二次迭代的增量数据

请注意，新的 DataFrame，df_read_data_hour2，其中包含 200 行。

3. 此时，我们准备使用以下代码将现有的 sales_orders 表与增量数据合并：

```
deltaTable = DeltaTable.forPath(spark, DELTA_TABLE_WRITE_PATH)
deltaTable.alias("store_orders").merge(
    df_read_data_hour2.alias("store_orders_new"),
    "store_orders.order_number = store_orders_new.order_number"
    .whenMatchedUpdate(set = {"order_number": "store_orders_new.order_number",
    "customer_id": "store_orders_new.customer_id",
    "product_id": "store_orders_new.product_id",
    "order_date": "store_orders_new.order_date",
    "units": "store_orders_new.units",
    "sale_price": "store_orders_new.sale_price",
    "currency": "store_orders_new.currency",
    "order_mode": "store_orders_new.order_mode",
    "updated_at": "store_orders_new.updated_at" } )
    .whenNotMatchedInsert(values =
    {
        "order_number": "store_orders_new.order_number",
        "customer_id": "store_orders_new.customer_id",
        "product_id": "store_orders_new.product_id",
        "order_date": "store_orders_new.order_date",
        "units": "store_orders_new.units",
        "sale_price": "store_orders_new.sale_price",
        "currency": "store_orders_new.currency",
        "order_mode": "store_orders_new.order_mode",
        "updated_at": "store_orders_new.updated_at"
    }
    ).execute()
```

(11) Spark Jobs

Command took 5.56 seconds -- by roop1kakukreja@gmail.com at 5/4/2021, 10:36:17 AM on trainingcluster

图 6.32 – 从 electroniz_batch_ingestion_pipeline 第二次迭代合并数据

注意 PySpark 命令为 upsert 操作所做的结构。

- sales_orders – 现有的 delta 表（2000 行）。
- sales_orders_new – 存储增量数据（200 行）的临时表。
- sales_orders 和 sales_orders_new 使用 order_number 列连接在一起。
- whenMatchedUpdate – 子句指示通过上述连接条件的行，这意味着它们已预先存在于 delta 表中，因此应使用增量数据集中的新数据简单更新。
- whenNotMatchedInsert – 子句指示未通过前述连接条件的行，这意味着它们是新的，因此应使用增量数据集插入。

4. 完成后，让我们了解合并后 store_orders 表中行数的变化：

```
%sql
SELECT count(*) FROM store_orders;
```

这导致以下输出：

图 6.33 – 合并第二次迭代后检查 sales_orders 中的行数

行数增加了 200。我相信你现在已经猜到了，增量行是通过 whenNotMatchedInsert 子句合并的。

5. 由于最近的合并，store_orders 表的历史也将推进：

```
%sql
DESCRIBE HISTORY store_orders;
```

这导致以下输出：

图 6.34 – 第二次迭代合并操作后 sales_orders 表的交易历史

请注意，store_orders 表的版本 4 显示 operation=MERGE。我相信到此时，你已经很好地理解了 delta 表中的 ACID 合规性。

6. 你可能还记得 electroniz_batch_ingestion_pipeline 每小时运行一次。第三次迭代后，又摄取了更多增量数据，这次我的文件夹结构在第三次迭代后看起来像 2021/04/28/20。此文件夹包含源数据库中编辑的五行数据。很可能这是由于价格调整造成的。

为了保持数据的一致性，我们希望商店数据库中调整后的价格能够正确反映在湖仓中。我们可以按如下方式开始读取增量数据：

```
ADLS_FOLDER="2021/04/28/20"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/[dbo].["+TABLE+" ]/"+ADLS_FOLDER
df_read_data_hour3 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema)
display(df_read_data_hour3)
```

这导致以下输出：

```
ADLS_FOLDER="2021/04/28/20"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/[dbo].["+TABLE+" ]/"+ADLS_FOLDER
df_read_data_hour3 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema)
display(df_read_data_hour3)
```

| order_number | customer_id | product_id | order_date | units | sale_price | currency | order_mode | updated_at |
|---|---|---|---|---|---|---|---|---|
| 500 | 856 | 4 | 02/16/2016 | 3 | 93.77 | CAD | EDIT | 2021-04-28T19:38:230+0000 |
| 1254 | 1707 | 8 | 07/26/2016 | 5 | 49.78 | USD | EDIT | 2021-04-28T19:38:217+0000 |
| 1501 | 883 | 0 | 06/23/2017 | 6 | 47.65 | CAD | EDIT | 2021-04-28T19:38:197+0000 |
| 2234 | 2429 | 6 | 03/29/2016 | 6 | 34.40 | USD | EDIT | 2021-04-28T19:38:223+0000 |
| 2345 | 317 | 1 | 10/03/2016 | 1 | 32.10 | USD | EDIT | 2021-04-28T19:38:207+0000 |

图 6.35 – electroniz_batch_ingestion_pipeline 第三次迭代的增量数据

请注意文件包含五行，其中 order_mode=EDIT。

7. 比较 store_orders 表中的相同行。请注意，这些行的 sale_price 值与前一个 Spark DataFrame df_read_data_hour3 中读取的增量数据相比有所不同：

```
%sql
SELECT * FROM store_orders WHERE order_number IN (500, 1254, 1501, 2234, 2345);
```


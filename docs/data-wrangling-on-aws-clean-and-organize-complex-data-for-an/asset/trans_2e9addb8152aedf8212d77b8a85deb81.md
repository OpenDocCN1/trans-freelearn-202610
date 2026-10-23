

5. 通过执行带 upsert 选项的命令来测试 upsert 情况：

```python
%time
wr.postgresql.to_sql(df_update, dbcon, schema="public", table="speed_test", mode="upsert",
    upsert_conflict_columns=['quadkey'],)
```

CPU times: user 4.25 ms, sys: 191 µs, total: 4.44 ms
Wall time: 7.69 ms

```python
%time
df_update1=wr.postgresql.read_sql_query("SELECT * FROM public.speed_test where quadkey in ('0022133222330013','0022133222330201')", dbcon)
```

CPU times: user 4.94 ms, sys: 17 µs, total: 4.96 ms
Wall time: 4.46 ms

df_update1

| | quadkey | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices |
|---|---------|------|------------|------------|------------|-------|---------|
| 0 | 0000000000000000 | POLYGON | 50 | 25 | 5 | 1 | 1 |
| 1 | 0022133222330013 | POLYGON((...)) | 135 | 128 | 72 | 1 | 2 |
| 2 | 0022133222330201 | POLYGON((...)) | 14997 | 20649 | 70 | 1 | 2 |

Figure 3.62: RDS integration – validating the upsert logic

这将帮助我们以简单有效的方式创建数据管道，用于在现有 Postgres 表中更新和插入数据。你也可以根据项目需求进一步自定义此代码。

在我们之前使用的 SageMaker notebook 环境中，pg8000 库已预安装，因此在安装 awswrangler 包后我们能够成功执行命令：

```python
!pip list | grep pg8000
pg8000    1.29.1
```

Figure 3.63: RDS integration – SageMaker database library availability verification

如果环境中没有 pg8000 或用于数据库引擎的另一个 Python 库，则需要在继续之前安装它。如果是 Lambda 函数，则可通过 Lambda 层添加。

## Redshift

awswrangler 模块具有连接到 Redshift 的额外功能；上述常见函数在 Redshift 中也存在。然而，我们还有一些特定于 Redshift 数据仓库的额外功能，如下所示：

- **连接到数据仓库**：我们可以使用用户名/密码像其他 RDS 数据库一样连接，或使用临时登录凭据连接到数据库用户名，前提是你拥有获取 Redshift 集群凭据的 IAM 权限（https://docs.aws.amazon.com/cli/latest/reference/redshift/get-cluster-credentials.html）。
- **向数据仓库写入/加载数据：**
    - **从 pandas DataFrame 加载：** 有几种选项可以将数据从 DataFrame 加载到 Redshift 表中。`to_sql` 函数，可用于 RDS，此前已使用，可用于此目的。另一个选项是使用 `copy` 方法，它使用 S3 中的 Parquet 文件作为暂存区并利用 Redshift COPY 命令加载数据。此方法在配置上开销更大，但在记录数量较多时更有效，因为 Redshift 并行线程加载数据。
    - **直接从 S3 文件加载：** 如果你想将数据直接从 S3 加载到 Redshift 而不加载到 pandas DataFrame，此方法更高效。此方法使用 COPY 命令将数据移入 Redshift。

以下步骤使用 `copy` 命令将数据写入 Redshift 数据仓库。

复制速度测试数据并将其加载到与 Redshift 集群相同区域的 S3 存储桶中。在以下示例中，我们将数据缩小到 100,000 条记录（仍是合理的数据量）以说明 Redshift 加载过程：

```python
speed_test=wr.s3.read_parquet("s3://oakla-open-data/parquet/performance/type=mobile/year=2021/quarter=4/")
df1=speed_test.head(100000)
wr.s3.to_parquet(df1,"s3://[redacted]/awswrangler/data/redshift_load_speed_test.parquet")
{'paths': ['s3://[redacted]/awswrangler/data/redshift_load_speed_test.parquet'],
 'partitions_values': {}}
```

Figure 3.64: Redshift integration – preparing data to be loaded into Redshift

**用例 1**：在不做任何修改的情况下将数据从 S3 迁移到 Redshift。按如下方式将数据加载到 Redshift 数据仓库：

```python
In [ ]:
%%time
#Method1 - Load S3 files directly into Redshift
wr.redshift.copy_from_files(path="s3://[redacted]/awswrangler/data/",
                           con=con, schema="public", table="speed_test", mode="overwrite"
                           ,iam_role="arn:aws:iam::473373155991:role/RedshiftImmersionRole" 
                           ,path_ignore_suffix=".txt")
CPU times: user 107 ms, sys: 11.1 ms, total: 118 ms
Wall time: 2.86 s
```

Figure 3.65: Redshift integration – loading data into Redshift

从 Redshift 查询数据并验证表是否按预期加载：

![](img/e28602b44ec813a47df57c96b204412c_143_0.png)

Figure 3.66: Redshift integration – querying data from Redshift

**用例 2**：通过 ETL 过程对数据进行一些修改后从 S3 迁移数据。将数据从 S3 文件加载到 pandas DataFrame 并使用 pandas 转换按需对数据执行 ETL 修改。

```python
#Usecase2 – Load S3 files into Pandas, make some changes and load it to Redshift using S3 copy command
speed_test=wr.s3.read_parquet("s3://ookla-open-data/parquet/performance/type=mobile/year=2021/quarter=4/")
```

```python
#Usecase2 – Perform ETL transformations
speed_test_load=speed_test.drop('tile',axis=1)
speed_test_load['newdevices']=speed_test_load['devices']+1
speed_test_load['avg_usedbandwidth']=speed_test_load['avg_d_kbps']+speed_test_load['avg_u_kbps']
```

```python
speed_test_load.head(2)
```

| quadkey | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices | newdevices | avg_usedbandwidth |
|---|---|---|---|---|---|---|---|
| 0 0022133222330013 | 85 | 103 | 72 | 1 | 1 | 2 | 188 |
| 1 0022133222330201 | 14947 | 20624 | 70 | 1 | 1 | 2 | 35571 |

Figure 3.67: Redshift integration – transforming data before loading it into Redshift

转换完成后，将数据加载到 Redshift 表中。记录集有 100,000 条记录，我们将使用 copy 函数并验证加载完成的总体时间。

```python
%time
#Path variable here is the S3 staging path for loading to Redshift
wr.redshift.copy(df=speed_test_load, path="s3://[redacted]/awswrangler/data-staging/",
    con=con,schema="public", table="speed_test_uc2",mode="overwrite",
    iam_role="arn:aws:iam::473373155991:role/RedshiftImmersionRole")
```

CPU times: user 3.31 s, sys: 318 ms, total: 3.63 s
Wall time: 8.48 s

Figure 3.68: Redshift integration – loading data into Redshift from S3 using the COPY command

我们可以验证在上述 `copy` 命令中提供的 S3 路径中是否创建了暂存文件。需要明确的是，我们运行了此语句两次，第一次运行耗时 15 秒，第二次运行耗时 8 秒。因此，加载时间取决于多个因素，包括带宽和当时的可用容量。

用户可以从 Redshift 查询数据并验证新表，以确保所做的更改已加载到其中。

| avg_lat_ms | tests | devices | newdevices | avg_usedbandwidth |
| :--- | :--- | :--- | :--- | :--- |
| 22 | 1 | 1 | 2 | 400783 |
| 29 | 5 | 3 | 4 | 69938 |
| 31 | 2 | 1 | 2 | 5879 |

Figure 3.69: Redshift integration – the query loaded data from Redshift

作为此用例的一部分，我们还将探索 `to_sql` 函数以将数据写入 Redshift，但这将使用 `INSERT` 语句加载数据，并且性能不如 `copy` 命令。然而，此方法不会有将数据存储在 S3 暂存路径中的开销。

```python
%%time
#Use case - Load S3 files into Pandas, make some changes and load it to Redshift using insert command directly
speed_test=wr.redshift.to_sql(df= speed_test_load,con=con,schema="public",table="speed_test_uc3")
```

CPU times: user 1min 28s, sys: 3.19 s, total: 1min 32s
Wall time: 18min 34s

Figure 3.70: Redshift integration – loading data into Redshift using the to_sql function

你可以看到这需要 18 分钟，远高于前两种方法。当然，此时间取决于多个因素，但此用例让你了解当数据较小时，例如引用表加载时发生的情况。

我们可以通过使用 `chunksize` 参数进一步减少加载时间。默认情况下，`chunksize` 配置为 200 条记录，因此记录以 200 条为一批加载。此因素可以根据运行它的机器的容量进行增加。以下屏幕截图显示了将 `chunksize` 参数设置为 2000 时的执行时间。尽管如此，与 `copy` 函数相比，此选项仍需要更长时间。

```python
%%time
#Use case - Load S3 files into Pandas, make some changes and load it to Redshift using insert command directly
speed_test=wr.redshift.to_sql(df=speed_test_load,con=con,schema="public",table="speed_test_uc3",chunksize=2000)
```

CPU times: user 1min 4s, sys: 1.63 s, total: 1min 6s
Wall time: 12min 29s

Figure 3.71: Redshift integration – loading data into Redshift using the to_sql function and the chunksize parameter


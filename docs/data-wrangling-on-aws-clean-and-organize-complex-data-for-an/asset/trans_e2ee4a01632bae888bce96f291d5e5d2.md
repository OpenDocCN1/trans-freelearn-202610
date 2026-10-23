

# 使用 Pandas 进行数据质量验证

对于熟悉 NFL 比赛的读者，赛季期间每周一场比赛，中间会有几周的比赛轮空。让我们首先验证为纽约巨人队准备的赛程，并检查实际数据是否符合这些规则。这是一个数据质量验证步骤，因为数据本身没有问题，但我们期望数据符合我们的规则，以匹配现实世界情况。请注意，这是由脚本生成的虚构数据，用于说明我们的用例。

我们首先会确定纽约巨人队是否有在同一时间段内安排的比赛，这将使这些比赛无法进行。我们将使用 pandas 的 `groupby` 功能对比赛开始时间进行分组，并获取比赛场次数量，如以下代码所示：

```python
#We need to identify schedule issues where the same team is playing multiple games on the same day across different stadiums
#We can highlight the issues as the same team can't play games at the same time
sport_schedule_NYG.groupby(['start_date_time'])['start_date_time'].count()
```

```text
start_date_time
2022-08-28 18:00:00    1
2022-09-04 12:00:00    1
2022-09-11 14:00:00    1
2022-09-18 14:00:00    1
2022-09-25 13:00:00    1
2022-10-02 15:00:00    1
2022-10-16 19:00:00    4
2022-11-13 14:00:00    4
2022-12-11 12:00:00    4
Name: start_date_time, dtype: int64
```

![](img/e28602b44ec813a47df57c96b204412c_334_0.png)

我们在 3 周的时间里安排了 4 场同时进行的比赛，这些比赛是不可能进行的。但我们是否在其他周也有问题，例如 9 月 25 日，当天不同时间安排了多场比赛？即使我们做出一个大胆的假设，即同一支队伍可以在一天内不同时间进行两场比赛，让我们理解比赛进行的地点背景。我们将对比赛开始时间和比赛所在城市执行 `groupby` 函数。

```python
#Go one level further, to identify issues that have same team playing games on same day at different locations
sport_schedule_NYG.groupby(['start_date_time','city'])['start_date_time'].count()
```

```text
start_date_time         city                         
2022-08-28 18:00:00    Arlington, Texas                 1
2022-09-04 12:00:00    East Rutherford, New Jersey      1
2022-09-11 14:00:00    East Rutherford, New Jersey      1
2022-09-18 14:00:00    East Rutherford, New Jersey      1
2022-09-25 13:00:00    Philadelphia, Pennsylvania       1
2022-09-25 15:00:00    Landover, Maryland                1
2022-10-16 19:00:00    East Rutherford, New Jersey      4
2022-11-13 14:00:00    East Rutherford, New Jersey      4
2022-12-11 12:00:00    East Rutherford, New Jersey      4
```

![](img/e28602b44ec813a47df57c96b204412c_334_1.png)

现在我们明白了，队伍无法在同一天内移动的这些城市之间进行多场比赛，因此我们还需要修改该周的赛程。上述验证旨在展示 pandas 在数据验证方面的能力。我们的业务规则是确保每支队伍每周只有一场比赛。让我们在我们的样本数据中验证该业务规则。我们将开始时间转换为日期字段，仅提取周的部分，并统计该队伍每周的比赛数量。

输出与我们之前的验证结果类似。现在我们将着手修正该队伍的赛程。

```python
#Practically, a team can play a match per week and let us identify teams having more than one game per week
sport_schedule_NYG.loc[:,"week_nbr"]=sport_schedule_NYG['start_date_time'].dt.isocalendar().week
sport_schedule_NYG.groupby(['week_nbr'])['week_nbr'].count()
```

```text
week_nbr
34      1
35      1
36      1
37      1
38      2
41      4
45      4
49      4
Name: week_nbr dtype: int64
```

图 9.60：验证 NFL 队伍是否每周只有一场比赛

我们将使用 numpy 库及其 `where` 子句函数，通过开始日期来识别记录，同时识别当开始日期数据重复时的对手队伍。以下更新语句仅用于本书的学习目的。我们手动更新了赛程，以确保纽约巨人队每周只有一场比赛，但我们没有比较其他队伍的赛程，实际上，创建一个反映这一点的赛程将是一个复杂的练习。因此，我们将只关注纽约巨人队，并假设其他队伍的赛程不会有任何冲突：

我们可以设置这些数据质量规则以执行并仅显示不符合规则的记录，然后向业务团队成员发送电子邮件，以便他们检查我们数据管道中的数据。

```python
sport_schedule_NYG.loc[:, "week_nbr"]=sport_schedule_NYG['start_date_time'].dt.isocalendar().week
sport_schedule_NYG.groupby(['week_nbr'])['week_nbr'].count()
```

```text
week_nbr
34      1
35      1
36      1
37      1
38      1
39      1
40      1
41      1
42      1
43      1
44      1
45      1
46      1
47      1
48      1
49      1
50      1
51      1
```

图 9.63：验证更新后的纽约巨人队赛程

我们也可以执行其他验证规则。我们可以针对 `ticket_purchase_hist` 表运行查询，并查看其为空。在实际生产环境中，可能是因为本应填充购票数据的过程损坏了。然而，在我们当前的用例中，本应插入购票数据的存储过程在初始设置期间未执行。我们将登录到*将示例数据加载到源数据库*章节中之前使用的 EC2 实例。一旦连接到 RDS MySQL 数据库，使用 `use` 命令选择 `dms_sample` 数据库并执行存储过程以插入 1,000 张门票，调用 `generateTicketActivity(1000, 0);`。

```mysql
MySQL [(none)]> use dms_sample;
Reading table information for completion of table and column names
You can turn off this feature by adding --skip-auto-rehash
Database changed

MySQL [dms_sample]> call generateTicketActivity(1000,0);
+--------------------------------------------------------------------------------------------+
| CONCAT('Congratulations! You've just purchased ',p_quantity,' tickets.') |
+--------------------------------------------------------------------------------------------+
| Congratulations! You've just purchased 6 tickets.                                         |
+--------------------------------------------------------------------------------------------+
1 row in set (0.22 sec)

+--------------------------------------------------------------------------------------------+
| CONCAT('Congratulations! You've just purchased ',p_quantity,' tickets.') |
+--------------------------------------------------------------------------------------------+
| Congratulations! You've just purchased 4 tickets.                                         |
+--------------------------------------------------------------------------------------------+
```

图 9.64：生成购票活动的存储过程

现在我们查询 `ticket_purchase_hist` 表，查看记录正在被填充。这将确保我们在数据管道中所有数据都按预期填充。

```python
# After executing the insert tickets stored procedure. This SP is created but not called during initial run
# We can insert 1000 tickets and validate the ticketing activity now. This ensures the data is complete
ticket_purchase_hist=wr.mysql.read_sql_query("SELECT * FROM dms_sample.ticket_purchase_hist", con=con)
```

|  | sporting_event_ticket_id | purchased_by_id | transaction_date_time | transferred_from_id | purchase_price |
|:---|:---:|:---:|:---:|:---:|:---:|
| 0 | 65536 | 410959 | 2022-12-07 03:23:29 | None | 43.13 |
| 1 | 65537 | 410959 | 2022-12-07 03:23:29 | None | 43.13 |
| 2 | 131071 | 4232646 | 2022-12-07 03:22:48 | None | 44.59 |
| 3 | 131072 | 4232646 | 2022-12-07 03:22:48 | None | 44.59 |
| 4 | 131090 | 4232646 | 2022-12-07 03:22:48 | None | 44.59 |
| ... |  |  |  |  |  |

![](img/e28602b44ec813a47df57c96b204412c_338_0.png)

我们将查询查看纽约巨人队与费城老鹰队（event_id – 934）之间比赛的购票情况。现在我们将看到一些记录的 `ticketholder_id` 字段已填充，标识了购买门票的人（这是从我们之前执行的存储过程中填充的）：

```python
sporting_event_ticket=wr.mysql.read_sql_query("SELECT * FROM dms_sample.sporting_event_ticket where sporting_event_id=934")
sporting_event_ticket.loc[sporting_event_ticket["ticketholder_id"].notnull()]
```

|  | id | sporting_event_id | sport_location_id | seat_level | seat_section | seat_row | seat | ticketholder_id | ticket_price |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 82496 | 63339564 | 934 | 32 | 2 | 10 | A | 1 | 2051116 | 29.44 |
| 82497 | 63339565 | 934 | 32 | 2 | 10 | A | 10 | 2051116 | 29.44 |
| 82498 | 63339582 | 934 | 32 | 2 | 10 | A | 8 | 2051116 | 29.44 |
| 82499 | 63339583 | 934 | 32 | 2 | 10 | A | 9 | 2051116 | 29.44 |

![](img/e28602b44ec813a47df57c96b204412c_338_1.png)

现在我们可以在哪些比赛门票售罄以及哪些比赛门票销售不佳方面进行分析。

我们使用几条规则验证了样本数据集的数据质量：

- 每支 NFL 队伍在整个赛季中每周需要安排一场比赛
- 应填充购票历史数据，以了解每场活动还有多少空座

# 数据质量验证与数据管道的集成

我们可以根据每个项目的特定要求对数据集执行不同的验证。可以通过以下更新进一步提高数据质量验证：


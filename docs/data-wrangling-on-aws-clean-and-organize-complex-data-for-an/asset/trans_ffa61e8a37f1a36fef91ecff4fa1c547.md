
将原始格式的数据转换为每个运动场馆各类别座位的聚合计数。这些数据将用于仪表板中，以可视化不同地点可用的座位类型。

透视函数提供了实现类似聚合的能力。我们将使用合并操作连接 sport_location 表数据和 seat 表数据（这些数据已加载到 output_data DataFrame 中）。我们将在本章的数据结构部分进一步探索如何连接 DataFrame。透视表将根据座位类型和体育场名称汇总座位数量。

```python
import awswrangler as wr
import pandas as pd
import numpy as np
con=wr.mysql.connect("wranglerbook")
sport_loc=wr.mysql.read_sql_query("SELECT * FROM dms_sample.sport_location", con=con)
pivot_data=pd.merge(output_data,sport_loc,left_on="sport_loc_id",right_on="id",how='inner')
final_data=pivot_data.pivot_table(values="seat_nbr", index="name", margins="True", columns="seat_type", aggfunc="count")
final_data
```

| 座位类型 | 豪华 | 视线受阻 | 优质 | 标准 | 低于标准 | 总计 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **名称** | | | | | | |
| AT&T Park | 1000.0 | NaN | 13000.0 | 23915.0 | 4000.0 | 41915 |
| AT&T Stadium | 1000.0 | 1000.0 | 22999.0 | 42000.0 | 13001.0 | 80000 |
| Angel Stadium | NaN | NaN | 17001.0 | 26482.0 | 2000.0 | 45483 |
| Arrowhead Stadium | 3415.0 | 1000.0 | 16000.0 | 50001.0 | 6000.0 | 76416 |
| Bank of America Stadium | 8000.0 | NaN | 23001.0 | 33418.0 | 11000.0 | 75419 |
| ... | ... | ... | ... | ... | ... | ... |
| US Cellular Field | 5000.0 | 1000.0 | 12614.0 | 18001.0 | 4000.0 | 40615 |

图 9.28：使用透视表将数据集聚合并转换为所需的结构

在上面的示例中，我们可以看到 AT&T Park 没有视线受阻的座位（请记住这是虚构数据 😊）。然而，座位计数是小数类型，这具有误导性，因为我们不可能有部分的座位。让我们通过以下方式使用 format 函数转换该列的数据类型来修复这个问题：

```python
final_data.head(5).style.format("{:.0f}")
```

| 座位类型 | 豪华 | 视线受阻 | 优质 | 标准 | 低于标准 | 总计 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **名称** | | | | | | |
| AT&T Park | 1000 | nan | 13000 | 23915 | 4000 | 41915 |
| AT&T Stadium | 1000 | 1000 | 22999 | 42000 | 13001 | 80000 |
| Angel Stadium | nan | nan | 17001 | 26482 | 2000 | 45483 |
| Arrowhead Stadium | 3415 | 1000 | 16000 | 50001 | 6000 | 76416 |
| Bank of America Stadium | 8000 | nan | 23001 | 33418 | 11000 | 75419 |

图 9.29：在 pandas 中使用数据格式化和样式选项

## 使用 AWS SDK for Pandas 构建端到端数据整理流水线

现在我们拥有了一种不同的数据表示方式，这是原始数据源中不可用的。类似地，你可以根据项目消费需求使用 pandas 操作（如透视、非透视等）来转换数据。

Pandas 提供了结构化成数据的能力，例如删除不需要的列、重命名列以及根据需要更改索引。这有助于将数据格式更改为更适合每种用例的更标准格式。此外，Pandas 中有一些函数可用于展平单列内的嵌套数据，以便更好地分析数据。

为了展示我们的工作，让我们考虑一个示例体育场数据，其中包含体育场的地址和餐饮选项。我们的示例数据集中没有这些数据，因此将使用一个示例值来展示 JSON 数据展平是如何工作的。我们将使用 `json_normalize` ([https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html](https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html)) 函数将嵌套的 JSON 数据展平为列以供分析。现在我们可以更高效地查询这些列的数据。你可以通过筛选 Location.State 列来查询加利福尼亚州有多少个体育场。

```python
stadium_address = {'Name': 'Angel Stadium', 'Location': {'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}, 'Dining': ['The Sweet shop', 'Big A Burger']}
```

```python
from pandas import json_normalize
flat1 = json_normalize(stadium_address)
```

| | Name | Dining | Location.City | Location.Zipcode | Location.State | Location.Address.Street Name | Location.Address.Number |
|:---|:---|:---|:---|:---|:---|:---|:---|
| 0 | Angel Stadium | [The Sweet shop, Big A Burger] | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

图 9.30：使用 json_normalize 函数展平层次结构 JSON 数据

然而，我们在 Dining 列中仍有列表形式的数据。如果我们希望展开该列，它将有助于我们进一步分析。为此，我们需要创建一个函数，该函数将展平来自 JSON 字符串的字典和列表数据。

```python
# 函数参考自 https://towardsdatascience.com/flattening-json-objects-in-python-f5343c794b10
def flatten_json(y):
    out = {}
    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x
    flatten(y)
    return out
```

图 9.31：展平包含 JSON 数据的列表和字典列的函数

现在，你可以看到输出显示了从 JSON 字符串中展平的字典和列表数据。我们使用了在前一步骤中创建的 `flatten_json` 函数来实现所需输出：

```python
flat = flatten_json(stadium_address)
json_normalize(flat)
```

图 9.32：展平包含列表和字典列的 JSON 数据

| | Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street_Name | Location_Address_Number | Dining_0 | Dining_1 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 0 | Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 | The Sweet shop | Big A Burger |

让我们花点时间来理解该函数是如何工作的，以便你可以进一步开发它。我们将向函数添加注释，并展示 `flatten` 函数的工作方式。该函数将被递归调用以展平 JSON 字符串：

```python
# 用于学习函数的工作方式。
def flatten(x, name='', out={}):
    print(x)
    a=""
    if type(x) is dict:
        for a in x:
            print("")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_')
    elif type(x) is list:
        i = 0
        for a in x:
            print("********************")
            print("List route " + name + str(i) + '_')
            flatten(a, name + str(i) + '_')
            i += 1
    else:
        print("===> Found a value for target column --> " + x)
        out[name[:-1]] = x
```

图 9.33：通过添加注释来理解 flatten 函数的工作方式

我们在函数中立即添加了 print 语句，以理解传入的输入字符串。我们在每个部分中添加了 print 语句，执行以下功能：

- 展平字典对象
- 展平列表对象
- 识别要存储在目标列中的原子值

### 该函数是如何工作的？

该函数将遍历提供的数据，并识别当前对象是字典还是列表：

- 字典对象具有花括号，并采用键值对结构，例如：`{ "name":"Angel stadium", "state":"CA" }`。字典内部可以包含字典或列表对象。
- 列表对象包含重复多次的类似数据项。它们存储在中括号结构中。它们可用于存储多值属性。在上面的示例中，我们将 Angel stadium 内多家餐厅的数据存储在列表中 – 例如：`[ "The sweet shop", "Big A burger" ]`。

一旦识别出对象，如果它是嵌套在另一个对象中的字典或列表，该函数将递归调用自身。

以下截图提供了该函数在提供最终输出之前如何解析数据的说明。遵循每个步骤，你可以更好地理解该函数的工作原理：


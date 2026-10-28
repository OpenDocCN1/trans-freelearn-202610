

+   - 为确保其属于我们公司，电子邮件地址必须属于我们公司的域名（就我们而言，我们只需使用 `example.com`）
+   - 为了与其他电子邮件地址保持一致，地址应转换为小写

这听起来像很多规则，但使用 Django，如果我们逐一处理，就会很简单。我们将继续使用你在 *Chapter 6, Forms* 中开始的 `form_project` 应用。如果你尚未完成 *Chapter 6, Forms*，你可以从 `https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter06/final/form_project` 下载代码。

+   1. 在 PyCharm 中，打开 `form_example` 应用的 `forms.py` 文件。

> **Note**
> 
> 请确保 Django 开发服务器未在运行；否则，在你修改此文件时它可能会崩溃，导致 PyCharm 跳入调试器。

由于我们对 `ExampleForm` 的工作已完成，你可以将其从此文件中删除。

+   2. 创建一个名为 `OrderForm` 的新类，它继承自 `forms.Form`：

```
```python
class OrderForm(forms.Form):
```
```

+   3. 按如下方式向该类添加四个字段：
+   
+   - `magazine_count`，IntegerField，`min_value` 为 0，`max_value` 为 80
+   - `book_count`，IntegerField，`min_value` 为 0，`max_value` 为 50
+   - `send_confirmation`，BooleanField，非必填
+   - `email`，EmailField，也是非必填

该类应如下所示：

```
```python
class OrderForm(forms.Form):
    magazine_count = forms.IntegerField(min_value=0,
                                        max_value=80)
    book_count = forms.IntegerField(min_value=0,
                                    max_value=50)
    send_confirmation = 
        forms.BooleanField(required=False)
    email = forms.EmailField(required=False)
```
```

4. 添加一个验证函数来检查用户的电子邮件地址是否在正确的域名上。首先，需要导入 ValidationError；将此行添加到文件顶部：

```
from django.core.exceptions import ValidationError
```

然后，在导入行之后编写此函数（在 OrderForm 类实现之前）：

```
def validate_email_domain(value):
    if value.split("@")[-1].lower() != "example.com":
        raise ValidationError("The email address must
            be on the domain example.com.")
```

该函数在 @ 符号上分割电子邮件地址，然后检查其后的部分是否等于 example.com。此函数单独使用时将验证非电子邮件地址。例如，not-valid@some other domain@example.com 字符串不会在此函数中引发 ValidationError。这在我们的情况下是可以接受的，因为我们使用的是 EmailField，其他标准字段验证器将检查电子邮件地址的有效性。

5. 将 validate_email_domain 函数作为验证器添加到 OrderForm 上的 email 字段。更新 EmailField 构造函数调用以添加 validators 参数，并传入包含验证函数的列表：

```
class OrderForm(forms.Form):
    ...
    email = forms.EmailField(required=False,
        validators=[validate_email_domain])
```

6. 添加 clean_email 方法到表单以确保电子邮件地址为小写：

```
class OrderForm(forms.Form):
    # truncated for brevity

    def clean_email(self):
        return self.cleaned_data['email'].lower()
```

7. 现在，添加 clean 方法以执行所有跨字段验证。首先，我们将仅添加确保只有在请求订单确认时才输入电子邮件地址的逻辑：

```
class OrderForm(forms.Form):
    # truncated for brevity
    def clean(self):
        cleaned_data = super().clean()
        if cleaned_data["send_confirmation"] and not
        cleaned_data.get("email"):
```

```
self.add_error("email", "Please enter an email address to receive the confirmation message.")
elif cleaned_data.get("email") and not cleaned_data["send_confirmation"]:
    self.add_error("send_confirmation", "Please check this if you want to receive a confirmation email.")
```

如果选中了 Send confirmation 但未添加电子邮件地址，这将向 Email 字段添加错误：

![](img/4840a43efd335abd10b93708225692d6_352_0.png)

Figure 7.8 – Error if Send confirmation is checked but no email address is added

同样，如果输入了电子邮件地址但未选中 Send confirmation，也会向 Email 添加错误：

![](img/4840a43efd335abd10b93708225692d6_353_0.png)

Figure 7.9 – Error because an email has been entered but the user has not chosen to receive confirmation

8. 在 clean 方法中添加最后的检查。项目总数不应超过 100。如果 magazine_count 和 book_count 的总和大于 100，我们将添加一个非字段错误：

```
class OrderForm(forms.Form):
    ...
    def clean(self):
        ...
        item_total = cleaned_data.get("magazine_count", 0) + cleaned_data.get("book_count", 0)

        if item_total > 100:
            self.add_error(None, "The total number of items must be 100 or less.")
```

这将通过将 None 作为第一个参数传递给 add_error 调用来添加非字段错误。

Note
请参阅 https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter07/Exercise7.01/form_project/form_example/forms.py 获取完整代码。

保存 forms.py。

9. 打开 reviews 应用的 views.py 文件。我们将更改表单的导入，以便导入 OrderForm 而不是 ExampleForm。请考虑以下导入行：

```
from .forms import ExampleForm
```

按如下方式更改它：

```
from .forms import OrderForm
```

10. 在 form_example 视图中，将使用 ExampleForm 的两行更改为使用 OrderForm。请考虑以下代码行：

```
form = ExampleForm(request.POST)
```

按如下方式更改：

```
form = OrderForm(request.POST)
```

同样，请考虑以下代码行：

```
form = ExampleForm()
```

按如下方式更改：

```
form = OrderForm()
```

其余函数可以保持不变。

我们不需要对模板进行更改。启动 Django 开发服务器并在浏览器中导航到 http://127.0.0.1:8000/form-example/。你应该会看到表单的渲染方式与图 7.10 所示类似：

![](img/4840a43efd335abd10b93708225692d6_355_0.png)

Figure 7.10 – OrderForm in the browser

+   11. 尝试使用 **Magazine count** 为 80 和 **Book count** 为 50 提交表单。浏览器会允许此操作，但由于它们的总和超过 100，表单中的 `clean` 方法会触发错误并在页面上显示：

![](img/4840a43efd335abd10b93708225692d6_355_1.png)

Figure 7.11 – A non-field error displayed on the form when the maximum number of allowed items is exceeded

12. 尝试选中 **Send confirmation** 但将 **Email** 字段留空。然后，填写 **Email** 文本框但取消选中 **Send confirmation**。任一组合都会导致错误，指出两者必须同时存在。错误会根据缺少的字段而有所不同：

![](img/4840a43efd335abd10b93708225692d6_356_0.png)

Figure 7.12 – Error message if no email address is present

13. 现在，尝试选中 **Send confirmation**，并输入位于 `example.com` 域上的电子邮件地址。你应该会收到一条消息，指出你的电子邮件地址必须具有 `example.com` 域名。你还应该收到一条消息，指出必须设置电子邮件——这是因为电子邮件没有进入 cleaned_data 字典。毕竟，它是无效的：

![](img/4840a43efd335abd10b93708225692d6_356_1.png)

Figure 7.13 – The error message that’s shown when the email domain is not example.com

14. 最后，为 **Magazine count** 和 **Book count** 输入有效值（例如 20 和 20）。选中 **Send confirmation** 并输入 UserName@Example.Com 作为电子邮件（确保匹配字母大小写，包括混合的大写和小写字符）：

![](img/4840a43efd335abd10b93708225692d6_357_0.png)

15. 切换到 PyCharm 并查看调试控制台。你会看到电子邮件在被我们的调试代码打印时已转换为小写：

```
magazine_count: (<class 'int'>) 20
book_count: (<class 'int'>) 20
send_confirmation: (<class 'bool'>) True
email: (<class 'str'>) username@example.com
```

Figure 7.15 – Email in lowercase, as well as other fields

这是我们的 `clean_email` 方法在起作用——即使我们输入了大写和小写混合的数据，它也已被转换为全小写。

在此练习中，我们创建了一个新的 `OrderForm`，实现了表单和字段 clean 方法。我们使用自定义验证器确保 **Email** 字段符合我们的特定验证规则——只允许特定域名。我们使用自定义字段清理方法（`clean_email`）将电子邮件地址转换为小写。然后我们实现了 `clean` 方法来验证相互依赖的表单。在此方法中，我们添加了字段和非字段错误。


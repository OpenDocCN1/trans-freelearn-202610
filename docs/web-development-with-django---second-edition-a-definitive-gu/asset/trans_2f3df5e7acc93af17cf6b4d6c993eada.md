

## 使用 plotly 生成图表

在尝试可视化应用程序所维护数据中的模式时，图表会非常有用。有许多 Python 库可以支持开发者生成静态或交互式图表。

在本书中，我们将使用 plotly，这是一个社区支持的 Python 库，可以生成图表并将其渲染到网页上。plotly 特别吸引我们的一点是它易于与 Django 集成。

要在你的系统上安装它，可以运行以下命令：

```
pip install plotly
```

现在你已经完成了安装，让我们看看如何使用 plotly 生成一个图表可视化。

## 设置图形对象

在开始生成图表之前，我们需要初始化一个 plotly Figure 对象，它本质上充当图表的容器。plotly Figure 对象非常容易初始化；可以通过以下代码片段完成：

```
from plotly.graph_objs import graphs
figure = graphs.Figure()
```

plotly 库的 `graph_objs` 模块中的 `Figure()` 构造函数返回一个 Figure 图形容器的实例，图表可以在其中生成。一旦 Figure 对象就绪，我们必须生成一个绘图。

## 生成绘图

绘图是数据集的可视化表示。这个绘图可以是散点图、折线图、图表等等。例如，要生成一个散点图，可以使用以下代码片段：

```
scatter_plot = graphs.Scatter(x_axis_values, y_axis_values)
```

`Scatter` 构造函数接收 X 轴和 Y 轴的值，并返回一个可用于构建散点图的对象。一旦生成了 `scatter_plot` 对象，下一步就是将此绘图添加到我们的 Figure 中。可以按如下方式完成：

```
figure.add_trace(scatter_plot)
```

`add_trace()` 方法负责将绘图对象添加到图形中，并在图形内部生成其可视化。

## 在网页上渲染绘图

一旦绘图被添加到图形中，就可以通过调用 plotly 库的 `offline` 绘图模块中的 `plot()` 方法将其渲染到网页上。如下面的代码片段所示：

```
from plotly.offline import plot
visualization_html = plot(figure, output_type='div')
```

`plot()` 方法接受两个主要参数：第一个是需要渲染的图形，第二个是容器 HTML 标签，图形 HTML 将在此容器内部生成。`plot` 方法返回完全集成的 HTML，可以嵌入到任何网页中，或作为模板的一部分来渲染图表。

现在，了解了图表绘图的工作原理后，让我们尝试一个动手练习，为我们示例数据集生成一个图表。

## 练习 13.05 – 在 Python 中生成图表

在本练习中，我们将使用 Python 生成一个图表绘图。它将是一个表示二维数据的散点图：

- 1. 在本练习中，我们将使用 `plotly` 库。要使用此库，我们需要通过运行以下命令将其安装到系统上：
  
      ```bash
      pip install plotly
      ```

- 2. 现在库已安装，创建一个名为 `scatter_plot_demo.py` 的新文件，并向其中添加以下导入语句：
  
      ```python
      from plotly.offline import plot
      import plotly.graph_objs as graphs
      ```

- 3. 导入完成后，创建一个名为 `generate_scatter_plot()` 的方法，该方法接受两个参数——X 轴的值和 Y 轴的值：
  
      ```python
      def generate_scatter_plot(x_axis, y_axis):
          """Generate a scatter plot for the provided x and
             y-axis values."""
      ```

- 4. 在此方法内部，首先创建一个对象作为图表的容器：
  
      ```python
      figure = graphs.Figure()
      ```

5. 图表容器设置完成后，使用 X 轴和 Y 轴的值创建一个新的 Scatter 对象，并将其添加到图表的 Figure 容器中：

```
scatter = graphs.Scatter(x=x_axis, y=y_axis)
figure.add_trace(scatter)
```

6. 散点图就绪并已添加到图形后，最后一步是生成 HTML，该 HTML 可用于在网页内渲染此绘图。为此，调用 `plot()` 方法并将图形容器对象传递给它。然后，在 HTML div 标签内渲染 HTML：

```
return plot(figure, output_type='div')
```

完整的 `generate_scatter_plot()` 方法现在应如下所示：

```
def generate_scatter_plot(x_axis, y_axis):
    figure = graphs.Figure()
    scatter = graphs.Scatter(x=x_axis, y=y_axis)
    figure.add_trace(scatter)
    return plot(figure, output_type='div')
```

7. 一旦生成了绘图的 HTML，就需要将其渲染到某个地方。为此，创建一个名为 `generate_html()` 的新方法，该方法将绘图 HTML 作为参数，并渲染一个包含该绘图的 HTML 文件：

```
def generate_html(plot_html):
    """Generate an HTML page for the provided plot."""
    html_content = "<html><head><title>Plot Demo</title></head><body>{}</body></html>"
    .format(plot_html)
    try:
        with open('plot_demo.html', 'w') as plot_file:
            plot_file.write(html_content)
    except (IOError, OSError) as file_io_error:
        print("Unable to generate plot file. Exception: {}".format(file_io_error))
```

8. 方法设置完成后，最后一步是调用它。为此，创建一个脚本入口点，设置 X 轴列表和 Y 轴列表的值，然后调用 `generate_scatter_plot()` 方法。使用该方法返回的值，调用 `generate_html()` 方法，该方法将创建一个包含散点图的 HTML 页面：

```
if __name__ == '__main__':
    x = [1,2,3,4,5]
    y = [3,8,7,9,2]
    plot_html = generate_scatter_plot(x, y)
    generate_html(plot_html)
```

9. 代码就绪后，运行该文件并查看生成的输出。要运行代码，请执行以下命令：

```
python3 scatter_plot_demo.py
```

执行完成后，将在脚本执行的同一目录中生成一个新的 `plot_demo.html` 文件。打开该文件后，你应该会看到以下内容：

![](img/4840a43efd335abd10b93708225692d6_604_0.png)

![](img/4840a43efd335abd10b93708225692d6_604_1.png)

图 13.8：使用 plotly 在浏览器中生成的图表

至此，我们已经生成了第一个散点图，其中不同的点通过一条线连接起来。

在本练习中，我们使用 plotly 库生成了一个可以在浏览器中渲染的图表，以便读者可视化数据。

现在，你了解了如何在 Python 中处理图表，以及如何从它们生成 HTML 页面。

但作为 Web 开发者，你如何在 Django 中使用这些图表呢？让我们来了解一下。

## 将 plotly 与 Django 集成

plotly 生成的图表非常容易嵌入到 Django 模板中。由于 `plot()` 方法返回完全包含的 HTML，可用于渲染图表，我们可以将返回的 HTML 作为 Django 中的模板变量并按原样传递。然后，Django 模板引擎将在最终模板显示在浏览器中之前，将该生成的 HTML 添加到最终模板中。

以下是一些示例代码：

```
def user_profile(request):
    username = request.user.get_username()
    scatter_plot_html = scatter_plot_books_read(username)
    return render(request, 'user_profile.html',
                  context={'plt_div': scatter_plot_html})
```

前面的代码将使模板中使用的 `{{ plt_div }}` 内容被存储在 `scatter_plot_demo` 变量中的 HTML 替换，最终模板将渲染每周阅读书籍数量的散点图。

现在，让我们深入探讨如何处理向 Django 添加可视化，以及通过绘图和图表增强应用程序体验。

## 将可视化与 Django 集成

在前几节中，你学习了如何以不同格式读写数据，以满足用户的不同需求。但是我们如何将所学内容与 Django 集成呢？

例如，在 Bookr 中，我们可能希望允许用户导出他们阅读过的书籍列表，或者可视化他们一年中的阅读活动。这该如何实现？下一个练习将聚焦于这一方面。你将学习目前所见的组件如何集成到 Django Web 应用程序中。

## 练习 13.06 – 在用户个人资料页面上可视化用户的阅读历史

在本练习中，我们将修改用户的个人资料页面，以便用户在访问 Bookr 上的个人资料页面时，能够可视化其书籍阅读历史。

让我们看看如何实现：

- 1. 要开始集成可视化用户阅读历史的功能，你需要安装 plotly 库。为此，请在终端中运行以下命令：
  
  ```
  pip install plotly
  ```


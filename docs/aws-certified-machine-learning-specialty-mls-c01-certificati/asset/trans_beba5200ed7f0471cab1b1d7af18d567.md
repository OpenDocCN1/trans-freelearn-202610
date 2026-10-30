

现在，我们已经对 Amazon SageMaker 有了总体的了解。让我们在下一节中将知识付诸实践。

> **重要提示**
> Amazon SageMaker 控制台一直在变化。当你阅读本书时，控制台的外观可能与本书中显示的不同。

## 动手使用 Amazon SageMaker 笔记本实例

在本节的第一个步骤是创建一个 Jupyter Notebook，这需要一个笔记本实例。让我们按以下步骤创建笔记本实例：

- 1. 登录你的 AWS 账户。
- 2. 导航到 **Services > Amazon SageMaker**。
- 3. 在左侧导航窗格中，单击 **Notebook instances**，然后单击 **Create notebook instance** 按钮。
- 4. 提供一个 **Notebook instance name**，例如 `notebookinstance`，并将 **Notebook instance type** 保持在默认的 `ml.t2.medium` 设置。在 **Permissions and encryption** 部分，在 **IAM role** 中选择 `Create a new role`。系统会要求你指定存储桶名称。为本示例的目的，它被选为任意存储桶。
- 5. 在成功创建角色后，你应该会看到类似 *Figure 9.3* 的内容：

![](img/209e007237d34506e7ba16180f2e905c_292_0.png)

Figure 9.3 – Amazon SageMaker role creation

- 6. 将其他所有内容保持默认设置，然后单击 **Create notebook instance** 按钮。

7. 一旦实例状态变为 InService，选择该实例。单击 Actions 下拉菜单并选择 Open Jupyter。这将打开你的 Jupyter Notebook。

8. 现在，我们已经准备好在新创建的实例上运行 Jupyter Notebook。我们将执行探索性数据分析（EDA）并绘制不同类型的图表来可视化数据。一旦熟悉 Jupyter Notebook，我们将构建一些模型来预测波士顿房价。我们将应用在前几章中学到的算法并进行比较，以找到根据我们的数据提供最佳预测的最佳模型。让我们开始吧。

9. 在 Jupyter Notebook 中，单击 New 并选择 Terminal。在命令提示符中运行以下命令以将代码下载到实例：

```
sh-4.2$ cd ~/SageMaker/
sh-4.2$ git clone https://github.com/PacktPublishing/
AWS-Certified-Machine-Learning-Specialty-MLS-C01-
Certification-Guide.git
```

10. 一旦 Git 仓库克隆到 SageMaker 笔记本实例，在命令提示符中输入 exit 以退出。现在，你的代码已准备好执行。

11. 导航到 Jupyter Notebook 文件部分中的 Chapter-9，如 Figure 9.4 所示：

![](img/209e007237d34506e7ba16180f2e905c_293_0.png)

Figure 9.4 – Jupyter Notebook

12. 单击第一个笔记本 1.Boston-House-Price-SageMaker-Notebook-Instance-Example.ipynb。它会提示你为笔记本选择内核。请选择 conda_python3，如 Figure 9.5 所示：

![](img/209e007237d34506e7ba16180f2e905c_294_0.png)

Figure 9.5 – Jupyter Notebook kernel selection

13. 从笔记本中，导航到 Kernel > Restart & Clear Output。单击播放图标逐个运行单元格。请确保你已运行每个单独的单元格并检查每次执行/运行的输出。

14. 你可以通过添加单元格和删除单元格来熟悉 Jupyter Notebook 操作。在其中一段落中，有一个 bash 命令允许你从笔记本中安装 xgboost 库。

15. 最后一个单元格解释了我们如何比较各种建模技术的不同分数以从数学上得出结论。Figure 9.6 清楚地显示，预测波士顿房价的最佳模型是 XGBoost：

```
models = pd.DataFrame({
    'Model': ['Linear Regression', 'Random Forest', 'XGBoost', 'Support Vector Machines'],
    'R-squared Score': [acc_linreg*100, acc_rf*100, acc_xgb*100, acc_svm*100],
    'Adjusted R-squared Score': [adj_R2_linreg*100, adj_R2_rf*100, adj_R2_xgb*100, adj_R2_svm*100],
    'MAE': [mae_linreg, mae_rf, mae_xgb, mae_svm],
    'MSE': [mse_linreg, mse_rf, mse_xgb, mse_svm],
    'RMSE': [rmse_linreg, rmse_rf, rmse_xgb, rmse_svm]})
models.sort_values(by='R-squared Score', ascending=False)
```

|  | Model | R-squared Score | Adjusted R-squared Score | MAE | MSE | RMSE |
|---|---|---|---|---|---|---|
| 2 | XGBoost | 85.799520 | 84.461793 | 2.530958 | 14.828152 | 3.850734 |
| 1 | Random Forest | 82.181317 | 80.502745 | 2.500316 | 18.606282 | 4.313500 |
| 0 | Linear Regression | 71.218184 | 68.506853 | 3.859006 | 30.053993 | 5.482152 |
| 3 | Support Vector Machines | 59.001585 | 55.139415 | 3.756145 | 42.810575 | 6.542979 |

Figure 9.6 – Comparing the models

16. 一旦你完成了此笔记本的执行，请随时关闭内核并从 SageMaker 控制台停止你的笔记本实例。这是节省成本的最佳实践。

在下一个动手部分，我们将熟悉 Amazon SageMaker 的训练和推理实例。我们还将使用 Amazon SageMaker API 使此过程更轻松。我们将使用与前一个示例中相同的笔记本实例。

## 动手使用 Amazon SageMaker 的训练和推理实例

在本节中，我们将学习如何训练模型并托管模型以生成预测结果。让我们通过使用前一个示例中的笔记本实例开始：

- 1. 登录你的 AWS 账户 https://console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances。

2. 单击我们在前一个示例中创建的实例 notebookinstance 旁边的 Start。一旦状态移动到 InService，在新标签页中打开它，如 Figure 9.7 所示：

![](img/209e007237d34506e7ba16180f2e905c_296_0.png)

Figure 9.7 – The InService instance

3. 导航到 Jupyter Notebook 主页中名为 SageMaker Examples 的标签。

4. 选择 k_nearest_neighbors_covtype.ipynb 笔记本。单击 Use and create a copy。

5. 当你运行以下段落，如 Figure 9.8 所示时，你也可以在 SageMaker 主页的 Training > Training jobs 中检查训练作业：

```
hyperparams = {"feature_dim": 54, "K": 10, "sample_size": 200000, "predictor_type": "classifier"}
output_path = f"s3://{bucket}/(prefix)/default_example/output"
knn_estimator = trained_estimator_from_hyperparams(
    s3_train_data, hyperparams, output_path, s3_test_data=s3_test_data
)
The method get_image_uri has been renamed in sagemaker>=2.
See: https://sagemaker.readthedocs.io/en/stable/v2.html for details.
Defaulting to the only supported framework/algorithm version: 1. Ignoring framework/algorithm version: 1.
2021-02-03 22:00:05 Starting - Starting the training job...
2021-02-03 22:00:30 Starting - Launching requested ML instancesProfilerReport-1612389605: InProgress
........
2021-02-03 22:01:51 Starting - Preparing the instances for training...
2021-02-03 22:02:32 Downloading - Downloading input data...
2021-02-03 22:02:52 Training - Downloading the training image...
2021-02-03 22:02:33 Training - Training image download completed. Training in progress..Docker entrypoint called with argument(s): train
Running default environment configuration script
[02/03/2021 22:03:35 INFO 140457495148352] Reading default configuration from /opt/amazon/lib/python2.7/site-package s/algorithm/resources/default-conf.json: {'u'index_metric': u'L2', 'u' tuning objective metric': u' ', 'u' num_gpus': u'a uto', 'u'log_level': u'info', 'u'feature_dim': u'auto', 'u'faiss_index_ivf_nlists': u'auto', 'u'epochs': u'1', 'u'index_t ype': u'faiss.Flat', 'u'faiss_index_nprobe': u'5', 'u'kvstore': u'dist_async', 'u'num_kv_servers': u'1', 'u'mini_batch _size': u'5000'}
[02/03/2021 22:03:35 INFO 140457495148352] Merging with provided configuration from /opt/ml/input/config/hyperparamet
```

Figure 9.8 – The SageMaker fit API call

6. 训练作业看起来类似于 Figure 9.9。它在后端启动一个 ECS 容器并使用在前一个示例中创建的 IAM 执行角色来运行此请求的训练作业：

![](img/209e007237d34506e7ba16180f2e905c_297_0.png)

Figure 9.9 – Training jobs

7. 如果你进入并在 CloudWatch 中检查日志，它会为你提供有关容器及其执行步骤的更多详细信息。作为机器学习工程师，值得进入并检查 Algorithm 的 CloudWatch 指标。

8. 现在，如果你在笔记本中运行以下段落，如 Figure 9.10 所示，那么它将创建一个端点配置和一个端点，在该端点上部署来自早期训练作业的模型。

9. 我已将实例类型更改为节省成本。它是托管模型的主机或机器。请明智地选择你的实例。我们将在下一节学习如何选择实例类型。我还更改了 endpoint_name 以便更容易识别：

```
import time

instance_type = "ml.t2.medium"
model_name = "knn_%s" % instance_type
endpoint_name = "knn-baba-test-%s" % (str(time.time()).replace(".", "-"))
print("setting up the endpoint..")
predictor = predictor_from_estimator(
    knn_estimator, model_name, instance_type, endpoint_name=endpoint_name
)
```

setting up the endpoint..
-

Figure 9.10 – Creating the predictor object with endpoint details


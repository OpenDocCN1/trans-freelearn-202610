

10. 导航到 **Inference > Endpoints**。这将显示在上一段执行后创建的端点。该端点具有配置，可以通过 **Inference > Endpoint Configurations** 进行导航和追踪。

11. 如果你在 notebook 中查看 **Inference** 部分，你会注意到它使用测试数据来预测结果。它使用来自 SageMaker API 的 predictor 对象来进行预测。predictor 对象包含端点详情、模型名称和实例类型。

12. 对端点的 API 调用发生在 **Inference** 部分中，并且通过创建 notebook 实例时使用的 IAM 角色进行身份验证。相同的 API 调用可以通过 CloudWatch 调用指标进行追踪。

13. 最后，在 notebook 中运行 `delete_endpoint` 方法将删除端点。要删除端点配置，请导航到 **Inference > Endpoint Configurations** 并在屏幕上选择该配置。点击 **Actions > Delete > Delete**。

14. 现在，请随意关闭内核并从 SageMaker 控制台停止你的 notebook 实例。这是节省成本的最佳实践。

在本节中，我们学习了如何使用 notebook 实例、训练实例、推理端点和端点配置来清理数据、训练模型并从中生成预测结果。在下一节中，我们将学习模型调优。

## Model tuning

在 *Chapter 8, Evaluating and Optimizing Models* 中，你学习了关于模型调优的许多重要概念。现在让我们从实践角度探索这个主题。

为了在 SageMaker 上调优模型，我们必须调用 `create_hyper_parameter_tuning_job` 并传递以下主要参数：

- `HyperParameterTuningJobName`：这是调优作业的名称。它有助于追踪代表你的调优作业而启动的训练作业。
- `HyperParameterTuningJobConfig`：在这里，你可以配置你的调优选项。例如，你想要调优的参数、它们的值范围、优化类型（如随机搜索或贝叶斯搜索）、你想要启动的最大训练作业数等。
- `TrainingJobDefinition`：在这里，你可以配置你的训练作业。例如，数据通道、输出位置、资源配置、评估指标和停止条件。

在 SageMaker 中，我们主要用来评估模型以选择最佳模型的一个指标被称为 **objective metric**。

在下面的示例中，我们正在为基于决策树的算法配置 `HyperParameterTuningJobConfig`。我们想要检查 **max_depth** 超参数的最佳配置，该超参数负责控制树的深度。

在 `IntegerParameterRanges` 中，我们必须指定以下内容：

- 超参数名称
- 我们想要测试的最小值
- 我们想要测试的最大值

**Important note**
每种类型的超参数必须适合于参数范围区段之一，例如分类、连续或整数参数。

在 `ResourceLimits` 中，我们指定了训练作业的数量以及我们想要并行运行的作业数量。请记住，调优过程的目标是以不同的超参数设置执行许多训练作业。这样做是为了选择最佳的一个作为最终模型。因此，我们必须指定这些训练作业执行规则。

我们然后在 `Strategy` 中设置搜索策略，最后在 `HyperParameterTuningJobObjective` 中设置目标函数：

```
tuning_job_config = {
    "ParameterRanges": {
        "CategoricalParameterRanges": [],
        "ContinuousParameterRanges": [],
        "IntegerParameterRanges": [
            {
                "MaxValue": "10",
                "MinValue": "1",
                "Name": "max_depth"
            }
        ]
    }
}
```

```
]
},
"ResourceLimits": {
    "MaxNumberOfTrainingJobs": 10,
    "MaxParallelTrainingJobs": 2
},
"Strategy": "Bayesian",
"HyperparameterTuningJobObjective": {
    "MetricName": "validation:auc",
    "Type": "Maximize"
}
}
```

我们需要设置的第二个重要配置是 TrainingJobDefinition。在这里，我们必须指定将要执行的训练作业的所有细节。其中最重要的设置之一是 TrainingImage 设置，它指的是将启动以执行训练过程的容器。该容器如预期必须实现了你的训练算法。

在这里，我们展示了一个内置算法 eXtreme Gradient Boosting 的示例，以便你可以将训练镜像设置为如下：

```
training_image = sagemaker.image_uris.retrieve('xgboost',
region, '1.0-1')
```

然后，你可以继续设置你的训练定义：

```
training_job_definition = {
    "AlgorithmSpecification": {
        "TrainingImage": training_image,
        "TrainingInputMode": "File"
    },
```

接下来，我们必须指定数据输入配置，也称为数据通道。在以下代码段中，我们设置了两个数据通道 – train 和 validation：

```
"InputDataConfig": [
    {
        "ChannelName": "train",
```

```
"CompressionType": "None",
"ContentType": "csv",
"DataSource": {
"S3DataSource": {
"S3DataDistributionType": "FullyReplicated",
"S3DataType": "S3Prefix",
"S3Uri": s3_input_train
}
}
},
{
"ChannelName": "validation",
"CompressionType": "None",
"ContentType": "csv",
"DataSource": {
"S3DataSource": {
"S3DataDistributionType": "FullyReplicated",
"S3DataType": "S3Prefix",
"S3Uri": s3_input_validation
}
}
}
],
```

我们还需要指定结果将存储的位置：

```
"OutputDataConfig": {
"S3OutputPath": "s3://{}/{}/output".format(bucket,prefix)
},
```

最后，我们设置资源配置、角色、静态参数和停止条件。在以下代码段中，我们想要使用两台类型为 ml.c4.2xlarge 的实例，每台 10 GB 存储：

```
"ResourceConfig": {
"InstanceCount": 2,
```

```
"InstanceType": "ml.c4.2xlarge",
"VolumeSizeInGB": 10
},
"RoleArn": <<your_role_name>>,
"StaticHyperParameters": {
"eval_metric": "auc",
"num_round": "100",
"objective": "binary:logistic",
"rate_drop": "0.3",
"tweedie_variance_power": "1.4"
},
"StoppingCondition": {
"MaxRuntimeInSeconds": 43200
}
}
```

> **Important note**
> 请注意，我们在此配置文件中使用了其他变量 bucket 和 prefix，它们应分别替换为你的存储桶名称和前缀键（如需要）。我们还引用了 s3_input_train 和 s3_input_validation，这两个变量指向 S3 中的训练和验证数据集。

一旦你设置了配置，你就可以启动调优过程：

```
smclient.create_hyper_parameter_tuning_job(
    HyperparameterTuningJobName = "my-tuning-example",
    HyperparameterTuningJobConfig = tuning_job_config,
    TrainingJobDefinition = training_job_definition
)
```

接下来，让我们了解一下如何追踪此过程的执行。

## Tracking your training jobs and selecting the best model

一旦你启动了调优过程，你可能还想检查另外两个步骤：追踪调优过程以及选择获胜模型（即具有最佳超参数集的模型）。

为了找到你的训练作业，你应该进入 SageMaker 控制台并导航到 Hyperparameter training jobs。你将找到已执行的调优作业列表，包括你的作业：

![](img/209e007237d34506e7ba16180f2e905c_303_0.png)

Figure 9.11 – Finding your tuning job

如果你通过点击其名称访问你的调优作业，你将找到一个摘要页面，其中包含有关调优过程的最相关信息。在 Training jobs 选项卡下，你将看到已执行的所有训练作业：

![](img/209e007237d34506e7ba16180f2e905c_303_1.png)

Figure 9.12 – Summary of the training jobs in the tuning process



具体来说，这个自定义训练器重写了 `build_evaluator` 方法，返回一个 COCOEvaluator 对象，用于评估测试集。之所以选择 COCOEvaluator 类，是因为测试数据集采用 COCO 格式。在指定评估器后，以下代码片段指定了该评估器的执行频率：

```
cfg.TEST.EVAL_PERIOD = cfg.SOLVER.CHECKPOINT_PERIOD
```

这段代码将评估周期设置为与检查点周期相同，因此每次保存检查点时都会进行评估。接下来的代码片段创建了一个自定义训练器，并像默认训练器一样执行训练：

```
trainer = BrainTumorTrainer(cfg)
trainer.train()
```

你可能会好奇，为什么在训练过程中执行评估器比在训练完成后评估训练好的模型更有价值。这种用法有几个原因。其一，这种方法可以在训练期间监控 COCO 评估指标的结果，这些指标可用于选择最佳模型并执行早停（当连续多次迭代后不再看到改进时，提前停止训练）。

另一个例子是，评估指标与训练指标（如分类损失和边界框损失）一起存储，并提供了使用 TensorBoard 等工具在训练期间评估偏差与方差权衡的机会。下一节将介绍一个名为**钩子系统**的新概念，并结合本节提出的思路来演示第一种场景。*第 6 章*将利用 TensorBoard 来演示第二种场景。

## 利用钩子系统

钩子系统允许将类合并到训练事件中执行若干任务。自定义钩子基于继承 Detectron2 中名为 `detectron2.engine.HookBase` 的基类构建。钩子允许开发人员通过重写以下方法在四个事件上执行任务：

- `before_training()` 用于在第一次训练迭代之前执行任务
- `after_training()` 用于在训练完成后执行任务
- `before_step()` 用于在每次训练迭代之前执行任务
- `after_step()` 用于在每次训练迭代之后执行任务

以下代码片段创建了一个钩子，用于从先前构建的自定义训练器中读取 COCOEvaluator 生成的评估指标，跟踪具有最高 mAP@0.5 值的最佳模型，并将模型保存为 model_best.pth：

```
# 为节省篇幅，省略了一些导入语句
class BestModelHook(HookBase):
    def __init__(self, cfg,
                metric = "bbox/AP50",
                min_max = "max"):
        # 后续说明
    def _take_latest_metrics(self):
        # 后续说明
    def after_step(self):
        # 后续说明
```

该类重写了 __init__ 方法，以传入 metric 和另一个参数（min_max），用于指示如何监控指标值（最小或最大）：

```
self._period = cfg.TEST.EVAL_PERIOD
self.metric = metric
self.min_max = min_max
self.best_value = float("-inf") if min_max == "max" else float("inf")
logger = logging.getLogger("detectron2")
logger.setLevel(logging.DEBUG)
logger.propagate = False
self._logger = logger
```

这个 BestModelHook 类还创建了一个方法（_take_latest_metrics），用于获取训练过程中存储的所有评估指标：

```
with torch.no_grad():
    latest_metrics = self.trainer.storage.latest()
    return latest_metrics
```

然后，after_step 方法遍历所有评估指标，获取监控值，并在评估结果优于当前最佳值时保存训练模型：

```
next_iter = self.trainer.iter + 1
is_final = next_iter == self.trainer.max_iter
if is_final or (self._period > 0 and next_iter % self._period == 0):
    latest_metrics = self._take_latest_metrics()
    for (key, (value, iter)) in latest_metrics.items():
        if key == self.metric:
            if (self.min_max == "min" and value < self.best_value) or (self.min_max == "max" and value > self.best_value):
                self._logger.info("Updating best model at iteration {} with {} = {}".format(iter, self.metric, value))
                self.best_value = value
                self.trainer.checkpointer.save("model_best")
```

值得注意的是，该钩子是通过重写 after_step() 方法来构建的。然而，它只在每个 `cfg.TEST.EVAL_PERIOD` 迭代且 COCOEvaluator 产生结果时执行其任务。

以下代码片段利用自定义训练器和钩子在训练期间执行模型评估和选择：

```
trainer = BrainTumorTrainer(cfg)
bm_hook = BestModelHook(cfg, metric="bbox/AP50", min_max="max")
trainer.register_hooks(hooks=[bm_hook])
trainer.train()
```

显然，将钩子注册到训练器中相对简单。此外，latest_metrics 变量包含许多训练和评估指标，我们只提取测试集上的 mAP@0.5（bbox/AP50）。最后，训练过程与默认训练器保持一致。最终，除了默认训练器生成的模型之外，还会有一个名为 model_best.pth 的模型，它保存了在测试数据集上评估具有最高 mAP@0.5 的模型权重。

恭喜！到此为止，你应该已经掌握了在自定义数据集上执行数据处理和训练自定义模型的步骤。此外，你还应该了解如何在训练中融入更多自定义处理。

## 小结

本章讨论了使用 Detectron2 训练目标检测模型时，探索、处理和准备自定义数据集的步骤。处理数据集后，将训练、测试和评估数据（如果有）注册到 Detectron2 并开始使用默认训练器训练目标检测模型是相对容易的。训练过程可能会产生许多模型。因此，本章提供了标准的评估指标和选择最佳模型的方法。默认训练器可能满足最常见的训练需求。然而，在某些情况下，可能需要自定义训练器来在训练过程中加入更多自定义功能。本章提供了构建自定义训练器的代码片段，使训练过程中能够包含对测试集的评估。同时，还提供了一个自定义钩子的代码片段，用于提取评估指标并在训练期间保存最佳模型。

下一章，即第 6 章，将使用 TensorBoard 分析默认训练器和本章构建的自定义训练器生成的训练与评估指标。分析这些指标应能提供调整目标检测模型以实现更高性能的方法。

## 检查训练结果并微调 Detectron2 的求解器

本章介绍如何使用 TensorBoard 检查训练历史。它利用代码和可视化方法解释 Detectron2 求解器背后的概念及其超参数。相关概念包括梯度下降、随机梯度下降、动量和可变学习率优化器。本章还提供代码，帮助你找到 Detectron2 求解器的标准超参数。

学完本章后，你将能够使用 TensorBoard 分析训练结果并获得洞见。你还会深入理解 Detectron2 求解器的关键超参数。此外，你将能够使用代码为自定义数据集生成这些超参数的合适值。具体而言，本章涵盖以下主题：

- 使用 TensorBoard 检查训练历史
- 理解 Detectron2 的求解器
- 微调学习率和批大小

### 技术要求

你必须已完成*第 1 章*，才能拥有适合 Detectron2 的开发环境。所有代码、数据集和结果均可在本书的 GitHub 仓库中获取：`https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2`。强烈建议你下载代码并跟随操作。

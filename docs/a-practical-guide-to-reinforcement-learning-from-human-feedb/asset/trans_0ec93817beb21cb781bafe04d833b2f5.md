

# 策略进度可视化与迁移学习对比

上文已获得使用 Q-learning 训练的策略进度，记为 `policy_progress_6x6`，可按 *图 1.5* 进行可视化，其中绘制了智能体在三个代表性回合（第 6、20 和 100 回合）中的轨迹。在第 6 和 20 回合中，智能体仍在学习策略，但到第 100 回合时，智能体已学会到达目标的最优路径。

![Figure 1.5 Progress in learning shown at various episodes for the 6 x 6 grid with trajectories taken by the agent to reach the goal.](media/file8.png)

**图 1.5** 显示了 6 x 6 网格中智能体为到达目标所采取的轨迹，在不同回合中的学习进度。

## 迁移学习至 8x8 网格

对于迁移学习（TL），将 **6x6** 环境中的 Q 值迁移到一个更复杂但相似的环境中，该环境包含 **8 x 8** 个单元格和 **5** 个障碍物（而非 3 个）。此处我们通过对比策略进度，比较从头训练智能体与使用迁移 Q 值训练智能体的效果。注意，我们不将 TL 的策略进度与 **6x6** 环境下从头开始的 Q-learning 进行比较，而是再次使用 `q_learning_scratch` 方法训练包含 **5** 个障碍物的 **8 x 8** 环境。执行代码如下：

```
obstacle_positions_5 = [(2, 3), (3, 1), (4, 3), (5, 2), (2, 5)]
new_grid_world = GridWorld(grid_size=(8,8), obstacle_positions=obstacle_positions_5)
rewards_transfer, steps_per_episode_transfer, policy_progress_transfer = new_grid_world.q_learning_transfer(grid_world.Q_values)
new_grid_world.plot_policy_progress(policy_progress_transfer)
rewards_scratch, steps_per_episode, policy_progress = new_grid_world.q_learning_scratch()
new_grid_world.plot_policy_progress(policy_progress)
```

基于 TL 的智能体训练策略进度使用 `plot_policy_progress` 方法绘制。如下所示的 *图 1.6* 中，你可以看到当 Q 值从包含 3 个障碍物的 6 x 6 网格训练中迁移而来时，8 x 8 网格中智能体为到达目标所采取的轨迹，在不同回合中的学习进度。在第 6 回合，智能体仍在学习策略；但到第 20 回合，智能体已学会到达目标的最优路径；在第 100 回合，它只是探索另一条最优路径。

![Figure 1.6 Agent trajectory at various episodes for the 8 x 8 grid when Q-values are transferred from training on 6 x 6 grid with 3 obstacles.](media/file9.png)

**图 1.6** 当 Q 值从包含 3 个障碍物的 6 x 6 网格训练中迁移而来时，8 x 8 网格中智能体在不同回合的轨迹。

同样，在 *图 1.7* 中绘制了智能体从头开始在包含 **5** 个障碍物的 **8x8** 网格环境中训练的策略进度。对比两种情况下的第 6、20 和 100 回合。在第 6 回合，智能体仍在学习策略；在第 20 回合，情况有所改善；但直到第 100 回合，智能体才学会到达目标的最优路径。

![Figure 1.7 Agent trajectories at various episodes for the 8 x 8 grid.](media/file10.png)

**图 1.7** 8 x 8 网格中智能体在不同回合的轨迹。

## 对比指标

为进一步对比基于迁移学习的训练进度与从头训练的进度，我们执行对比指标函数，如下所示：

```
grid_world.plot_rewards_comparison(rewards_transfer, rewards_scratch)
grid_world.plot_comparison_metrics(rewards_transfer, rewards_scratch, steps_per_episode_transfer, steps_per_episode)
```

在 *图 1.8* 中，通过绘制总奖励（在 10 个回合的窗口上平均/平滑）随回合变化的曲线，对比了在 8x8 网格上使用迁移 Q 值训练的智能体与从头训练的智能体的 Q-learning 性能，结果显示迁移学习加速了训练收敛。使用 TL 训练的奖励收敛在 **7** 个回合内完成，而从头训练需要 **45** 个回合。总奖励也更高（109 > -1446）。注意，第 0 回合的初始策略未绘制，且智能体此处仍遵循探索与利用策略（epsilon-greedy 方法）。

![Figure 1.8 Total reward per episodes averaged, comparing TL (in blue/darker) vs. from scratch.](media/file11.png)

**图 1.8** 每回合平均总奖励，对比 TL（蓝色/深色）与从头训练。

我们通过查看原始回合与总回合奖励的收敛情况，以及对比智能体在每个回合中到达目标所需的步数（每回合步数），进一步分析此问题，如 *图 1.9* 所示。该图显示迁移学习收敛更快，且在训练过程中更早以更少步数到达目标。

![Figure 1.9 Raw total reward per episode (left) and steps taken by agent to reach the goal for every episode (right) for transfer learning vs. Q-learning from scratch.](media/file12.png)

**图 1.9** 迁移学习与从头开始的 Q-learning 对比：原始每回合总奖励（左）和智能体每回合到达目标所走步数（右）。

## 示例输出

运行执行代码后，这些指标每 5 步打印一次。下面显示了示例输出。此处为简洁起见，使用 `...` 表示省略的行。

```
Q learning from Scratch
Episode 5/200, Total Reward: -116, Steps: 127...
Episode 15/200, Total Reward: -142, Steps: 153
Episode 20/200, Total Reward: 0, Steps: 11
Episode 100/200, Total Reward: 5, Steps: 6
Q learning with Transferred Q-values
Episode 5/200, Total Reward: -66, Steps: 77
Episode 10/200, Total Reward: 5, Steps: 6
Episode 15/200, Total Reward: 1, Steps: 10
Episode 20/200, Total Reward: 5, Steps: 6
```

从记录的示例输出中也可以看出，使用迁移 Q 值训练的智能体在 10 个回合内即达到正奖励，而从头训练需要超过双倍的回合数才能达到正奖励。在看到迁移学习带来的显著改进后，我们对使用人类反馈也持乐观态度，这将在下一节中实现。

# 基于人类反馈的强化学习

在前一节中，我们看到，通过将在源环境（**6 x 6** 网格含 **3** 个障碍物）上预训练的 Q 值迁移至目标环境（**8 x 8** 网格含 **5** 个障碍物）来训练智能体，能有效加快学习进度。这里有一个重要的注意事项：环境应具有合理的相似性，才能从迁移的策略中受益。如果环境差异显著，甚至目标（或到达目标的奖励激励）相互冲突，迁移学习可能会失效。我建议你尝试使用与获取待迁移策略 Q 值时截然不同的障碍物和目标进行实验。在这种情况下如何加速训练？一种方法是将人类反馈纳入策略训练中。人类反馈可以采取多种形式，例如显式奖励、评估或期望行为的演示。

- **显式奖励：** 人类反馈根据智能体的动作提供显式的奖励或惩罚。例如，在 `gridworld` 场景中，人类可能会为朝目标移动分配正奖励，为撞击障碍物分配负奖励。

- **评估：** 人类评估智能体的动作或策略，并根据其表现提供反馈。例如，他们可能会给出“这是个好动作”或“你本该向左而不是向右”的反馈，客观的点赞或点踩，“是”或“否”。

- **演示：** 人类通过展示智能体如何在环境中解决任务来演示期望行为。这可以是通向成功结果的轨迹或动作序列形式。


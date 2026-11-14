
# 可视化策略进度

为了清晰地可视化智能体朝目标迈进的轨迹，直到到达目标，我们实现了 `plot_policy_progress` 方法。在这里，网格世界被绘制出来，并标记了智能体走过的完整轨迹。这种轨迹可以为每个回合可视化。但只需少数几个回合的快照就足以展示进度。下面的函数定义和输入通过分配 `num_checkpoints`，设置了包含子图的图形，每个子图对应 `policy_progress` 列表中的一个检查点。

```
def plot_policy_progress(self, policy_progress):
    num_checkpoints = len(policy_progress)
    fig, axes = plt.subplots(1, num_checkpoints, figsize=(6*num_checkpoints, 6))
```

## 遍历检查点

然后，它开始一个循环来处理 `policy_progress` 中的每个检查点。它设置每个子图的标题以指示回合编号。

```
for i, (policy, visited_states, episode) in enumerate(policy_progress):
    ax = axes[i]
    ax.set_title(f'Policy at Episode {episode}')
```

## 状态的可视化表示

该函数接着遍历策略网格中的每个状态。它为目标位置、障碍物、起始位置和其他状态设置可视化表示，用适当的符号、颜色或样式标记它们。

```
for y in range(policy.shape[0]):
    for x in range(policy.shape[1]):
        state = (y, x)
        if state == self.goal_position:
            ax.text(x, self.grid_size[0] - y - 1, 'G', va='center', ha='center', fontsize=20)
            ax.add_patch(plt.Rectangle((x - 0.5, self.grid_size[0] - y - 1 - 0.5), 1, 1, color='lightgreen'))
        elif state in self.obstacle_positions:
            ax.text(x, self.grid_size[0] - y - 1, 'X', va='center', ha='center', fontsize=20)
            ax.add_patch(plt.Rectangle((x - 0.5, self.grid_size[0] - y - 1 - 0.5), 1, 1, color='lightgray'))
        elif state == self.start_position:
            ax.text(x, self.grid_size[0] - y - 1, 'S', va='center', ha='center', fontsize=20, color='red')
            ax.add_patch(plt.Rectangle((x - 0.5, self.grid_size[0] - y - 1 - 0.5), 1, 1, color='lightblue'))
        else:
            action = policy[y, x]
            ax.text(x, self.grid_size[0] - y - 1, self.action_symbols[action], va='center', ha='center', fontsize=20)
```

## 绘制智能体路径并配置网格

该函数接着为每个检查点绘制智能体走过的路径，用线条连接连续的状态。它还为子图配置网格线和标签，使绘图更易读。

```
for j in range(len(visited_states) - 1):
    x1, y1 = visited_states[j][1], self.grid_size[0] - visited_states[j][0] - 1
    x2, y2 = visited_states[j + 1][1], self.grid_size[0] - visited_states[j + 1][0] - 1
    ax.plot([x1, x2], [y1, y2], color='blue', linewidth=3)
    ax.set_xticks(np.arange(-0.5, policy.shape[1], 1))
    ax.set_yticks(np.arange(-0.5, policy.shape[0], 1))
    ax.grid(color='black', linestyle='-', linewidth=1)
    ax.set_xticklabels([])
    ax.set_yticklabels([])
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
```

图 1.4 展示了策略进度，即智能体在逐步回合中采取的轨迹，执行 1000 个回合的训练后，记录了第 1、10、20 和 999 回合的快照。智能体必须游荡并重走一些路径，在第 1 回合中走更长的轨迹到达目标。到第 10 回合时已有一些进展，到第 20 回合时智能体采取的轨迹有显著改善。在第 999 回合时，智能体已走最优路径。作为练习，将 q-learning 的回合数修改为仅 200 或 500 个，并更改检查点的数量。

![](media/file4.png)![](media/file5.png)![](media/file6.png)![图 1.4. 训练期间四个代表性回合的策略进展。](media/file7.png)

图 1.4. 训练期间四个代表性回合的策略进展。

实例化 `GridWorld` 类并运行 q-learning 训练的执行代码如下。

```
# 调整超参数以进行实验
obstacle_positions = [(2, 3), (3, 1), (4, 3)]
grid_world = GridWorld(grid_size=(6, 6), obstacle_positions=obstacle_positions)
rewards, steps_per_episode, policy_progress = grid_world.q_learning(num_episodes=1000, learning_rate=0.1, discount_factor=0.95, epsilon=0.1)
```

在上述 q-learning 实现中，智能体能够在有限的回合数内学会如何到达目标。所示的可视化方法有助于跟踪学习进度。本例中使用了三个障碍物和 6 x 6 的网格大小。该问题收敛所需的训练回合数约为 50，但对于更大的网格尺寸或更多障碍物，这个数字可能会增加。请注意，`GridWorld` 类允许参数化网格大小和障碍物位置，方便修改环境。我们将在后续章节中利用这种参数化来探索改进学习进度的策略。

## 设计奖励函数与迁移学习

为了让智能体学习到理想的策略，在前一节讨论的 Q-learning 算法中，对理想行为给予智能体正奖励，对非理想行为给予负奖励。在网格世界问题中，智能体每走一步都会获得 `-1` 的负奖励，这会阻止它走更多步骤。同样，智能体每走一步如果与障碍物重叠，会获得 `-5` 的较大负奖励。较大的负奖励有助于学习避开障碍物。最后，到达目标获得 `10` 的较大正奖励，鼓励到达目标状态。奖励分配本身就是奖励函数，重述如下：

```
reward = -1
if next_state == self.goal_position:
    reward = 10
elif next_state in self.obstacle_positions:
    reward = -5
```

请注意，奖励函数是离散的，且非常适合网格世界问题。在某些情况下，奖励函数可能需要是连续的。例如，如果网格有许多单元格，如 *1000 x 1000*，只有少数障碍物，随着智能体越来越接近目标，逐渐增加的连续奖励将有助于提供有意义的结果。如果除目标处外所有奖励都是 `-1`，智能体会游荡更久。在这种情况下，我们可以重新设计奖励分配。下面给出一个例子：

```
reward = -1 + np.exp(-0.1 * np.linalg.norm(np.array(state) - np.array(self.goal_position)))
if next_state in self.obstacle_positions:
    reward = reward - 5
```

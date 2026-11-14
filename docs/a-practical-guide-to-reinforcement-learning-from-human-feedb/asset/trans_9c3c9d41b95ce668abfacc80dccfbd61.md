
`reward = -1 + np.exp(-0.1 * np.linalg.norm(np.array(state) - np.array(self.goal_position)))` 这行代码计算当前状态的奖励。它以 `-1` 作为基础奖励，然后根据当前状态与目标位置之间的距离调整该基础奖励。当前状态与目标位置之间的距离使用 NumPy 中的欧几里得距离公式实现计算：`np.linalg.norm(np.array(state) - np.array(self.goal_position))`。该距离随后乘以 `-0.1` 缩放因子，并通过指数函数 `np.exp` 处理。负缩放因子确保奖励随距离增加而减小，指数函数则确保这种减小是渐进的而非线性的。接着，如果下一状态（智能体执行动作后将转移到的状态）在障碍物位置列表 `self.obstacle_positions` 中，奖励将进一步减少 5。这种惩罚鼓励智能体避开障碍物，因为转移到障碍物状态会导致更大的负奖励。

## 迁移学习

**迁移学习**（**TL**）在强化学习语境下，指利用解决某一任务获得的知识，来提升在相关但不同任务上的学习效果或性能。为演示网格世界问题中的迁移学习，我们修改了上一节讨论的 `q_learning` 函数代码。以下是新增的类方法：

`q_learning_transfer` 方法用于迁移学习。它接收来自较小网格世界（如 **6x6**）的预训练 Q 值，并将其迁移至较大的网格世界（如 **8x8**）。迁移的 Q 值随后在新环境中通过 Q-learning 进行微调。

```python
def q_learning_transfer(self, pre_learned_Q_values, epsilon=0.01):
    # Initialize Q-values with pre-learned values
    self.Q_values[:pre_learned_Q_values.shape[0], :pre_learned_Q_values.shape[1]] = pre_learned_Q_values
    print(f"Q learning with Transferred Q-values")
    # Perform Q-learning in the new environment
    rewards_transfer, steps_per_episode, policy_progress = self.q_learning(epsilon=0.01)
    return rewards_transfer, steps_per_episode, policy_progress
```

`q_learning_scratch` 方法在新环境中从头开始执行 Q-learning，首先初始化 Q 值。

```python
def q_learning_scratch(self):
    self.Q_values = np.zeros((self.num_states, self.num_actions))  # Or initialize with other initial values if needed
    print(f"Q learning from Scratch")
    rewards_scratch, steps_per_episode, policy_progress = self.q_learning()
    return rewards_scratch, steps_per_episode, policy_progress
```

最后，在完成迁移学习与从头训练两个模型的训练后，可以对两者进行对比。下面的 `plot_rewards_comparison` 方法对比它们的性能，以总奖励为指标，并使用移动平均平滑后绘图。我们展示该方法的部分代码片段。

下面的代码定义了函数，并计算迁移学习与从头训练两种场景的收敛速度。它找到奖励达到各自场景最大奖励 90% 的第一个回合。

```python
def plot_rewards_comparison(self, rewards_transfer, rewards_scratch):
    convergence_speed_transfer = next((i for i, reward in enumerate(rewards_transfer) if reward >= 0.9 * max(rewards_transfer)), len(rewards_transfer))
    convergence_speed_scratch = next((i for i, reward in enumerate(rewards_scratch) if reward >= 0.9 * max(rewards_scratch)), len(rewards_scratch))
```

随后代码对每个回合的奖励求和，计算迁移学习与从头训练的总奖励。

```python
    total_reward_transfer = sum(rewards_transfer)
    total_reward_scratch = sum(rewards_scratch)
```

为获得更好的可视化效果，使用移动平均平滑奖励曲线。移动平均的窗口大小设为 10。

```python
    window_size = 10
    rewards_transfer_smoothed = np.convolve(rewards_transfer, np.ones(window_size)/window_size, mode='valid')
    rewards_scratch_smoothed = np.convolve(rewards_scratch, np.ones(window_size)/window_size, mode='valid')
```

下面的代码绘制两种场景的平滑奖励曲线，使用不同颜色和样式以便区分，并添加适当标签。你可以根据喜好修改颜色和样式。

```python
    plt.plot(np.arange(window_size//2, len(rewards_transfer_smoothed) + window_size//2), rewards_transfer_smoothed, label='Transfer Learning', linestyle='--', color='blue', alpha=0.7)
    plt.plot(np.arange(window_size//2, len(rewards_scratch_smoothed) + window_size//2), rewards_scratch_smoothed, label='Training from Scratch', linestyle='--', color='orange', alpha=0.7)
```

然后在图上用垂直线高亮显示与迁移学习相关的特定回合。

```python
    transfer_epochs = [episode for _, _, episode in policy_progress]
    for epoch in transfer_epochs:
        plt.axvline(x=epoch, color='gray', linestyle='--', linewidth=0.8)
```

为使图表更丰富，我们添加文本标注以显示两种场景的收敛速度和总奖励。

```python
    plt.text(0.5, 0.6, f"Convergence Speed (Transfer): {convergence_speed_transfer}", transform=plt.gca().transAxes)
    plt.text(0.5, 0.55, f"Total Reward (Transfer): {total_reward_transfer}", transform=plt.gca().transAxes)
    plt.text(0.5, 0.5, f"Convergence Speed (Scratch): {convergence_speed_scratch}", transform=plt.gca().transAxes)
    plt.text(0.5, 0.45, f"Total Reward (Scratch): {total_reward_scratch}", transform=plt.gca().transAxes)
```

`plot_rewards_comparison` 函数的最后片段设置图表的标签、标题和图例，并显示图表。

```python
    plt.xlabel('Episode')
    plt.ylabel('Total Reward')
    plt.title('Q-learning Performance Comparison')
    plt.legend()
    plt.show()
```

下面的 `plot_comparison_metrics` 方法对比带有迁移学习（预训练 Q 值迁移）的 Q-learning 与从头训练的 Q-learning 的奖励指标。

```python
def plot_comparison_metrics(self, rewards_transfer, rewards_scratch, steps_transfer, steps_scratch):
    fig, axes = plt.subplots(1, 2, figsize=(12, 6))
    ax1 = axes[0]
    ax1.plot(rewards_transfer, label='Transfer Learning')
    ax1.plot(rewards_scratch, label='Training from Scratch')
    ax1.set_xlabel('Episode')
    ax1.set_ylabel('Total Reward')
    ax1.set_title('Total Reward Comparison')
    ax1.legend()
    ax2 = axes[1]
    ax2.plot(steps_transfer, label='Transfer Learning')
    ax2.plot(steps_scratch, label='Training from Scratch')
    ax2.set_xlabel('Episode')
    ax2.set_ylabel('Steps per Episode')
    ax2.set_title('Steps per Episode Comparison')
    ax2.legend()
    plt.tight_layout()
    plt.show()
```

下面展示策略训练与迁移学习步骤的示例执行。为演示迁移学习的工作原理，我们首先实例化修改后的 `GridWorld` 类，创建一个包含三个障碍物的 **6x6** 网格世界以获得训练好的策略。运行 `q_learning()` 方法得出 **6x6** 环境的最优策略和 Q 值。我们将使用这些 Q 值进行迁移学习。代码如下：

```python
obstacle_positions = [(2, 3), (3, 1), (4, 3)]
grid_world = GridWorld(grid_size=(6, 6), obstacle_positions=obstacle_positions)
rewards, steps_per_episode_6x6, policy_progress_6x6 = grid_world.q_learning()
grid_world.plot_policy_progress(policy_progress_6x6)
```


# GridWorld 状态索引转换与 Q-Learning 实现

## 状态到索引的转换：`state_to_index(state)`

方法 `state_to_index(state)` 用于将状态（表示网格中位置的元组）转换为其在 Q 值数组中对应的索引。下面分解表达式 `state[0] * self.grid_size[1] + state[1]`：

*   `state[0]`：表示状态在网格中的行索引。
*   `self.grid_size[1]`：表示网格的列数。因为 `grid_size` 是元组 `(rows, columns)`，所以 `self.grid_size[1]` 给出列数。
*   `state[1]`：表示状态在网格中的列索引。

将 `state[0]` 乘以 `self.grid_size[1]`，本质上是将行索引转换为跨越所有行的线性索引。例如，若网格大小为 `(6, 6)` 且位置为 `(2, 3)`，计算为 `2 * 6 + 3 = 15`。这将行索引 2 转换为网格扁平化表示中的线性等价值。随后加上 `state[1]`（列索引），即可将二维坐标映射为一维数组中的单个索引，便于访问 Q 值数组元素。这种方法确保每个状态唯一映射到 Q 值数组的一个索引，从而在 Q-Learning 过程中高效存取 Q 值。

## 索引到状态的转换：`index_to_state(self, index)`

`GridWorld` 类中的方法 `index_to_state(self, index)` 负责将 Q 值数组中的索引转换回网格中的二维状态坐标 `(i, j)`。

表达式 `index // self.grid_size[1]` 计算状态的行索引：

*   `index // self.grid_size[1]` 通过整除求出索引之前完整的行数，即行索引。

表达式 `index % self.grid_size[1]` 计算状态的列索引：

*   `index % self.grid_size[1]` 计算索引除以列数（`self.grid_size[1]`）的余数，即列索引。

该方法返回 `tuple (row_index, column_index)`，表示网格中状态的二维坐标。例如：

*   假设 `index = 20`，网格大小为 `(6, 6)`。
*   `row_index = 20 // 6 = 3`（整除）
*   `column_index = 20 % 6 = 2`（取余）

对应的网格状态即为 `(3, 2)`。当你拥有 Q 值数组中的索引并需获取对应的网格坐标时，此方法可将线性索引映射回原始二维位置，便于 Q 值与策略的解读与可视化。

## 状态有效性检查：`is_valid_state`

考虑网格世界中的障碍物，智能体可存在的状态首先必须在网格边界内，且不与障碍物重合。为保证状态有效，新增方法 `is_valid_state`，检查状态是否在边界内且非障碍物。

```
def is_valid_state(self, state):
    return 0 <= state[0] < self.grid_size[0] and 0 <= state[1] < self.grid_size[1] \
           and state not in self.obstacle_positions
```

## 获取下一状态：`get_next_state`

为推进至下一状态，新增方法 `get_next_state()`：给定状态与动作，计算下一状态。若动作导致无效状态，则停留在当前状态。

*   表达式 `(state[0] + action[0], state[1] + action[1])` 通过将动作指定的变化量加到当前状态上，计算下一状态。
*   `state[0] + action[0]` 通过加上 `action[0]` 指定的行方向变化量，计算新行索引。
*   `state[1] + action[1]` 通过加上 `action[1]` 指定的列方向变化量，计算新列索引。

随后调用 `is_valid_state` 检查下一状态是否有效（在边界内且非障碍物）。若有效则返回下一状态，否则返回当前状态，表明智能体无法移动而需原地停留。

```
def get_next_state(self, state, action):
    next_state = (state[0] + action[0], state[1] + action[1])
    if self.is_valid_state(next_state):
        return next_state
    return state
```

该方法在 Q-Learning 算法中用于根据当前状态与智能体选择的动作确定下一状态，确保移动遵守网格边界并避开障碍物。

## Q-Learning 算法

上述方法定义了网格世界环境、状态、动作及状态更新机制，涵盖了环境部分及智能体交互的媒介。智能体利用状态生成策略以选取动作，`q_learning` 方法实现了 Q-Learning 算法。

首先，算法迭代多个回合以探索各种策略，开启循环。将状态初始化为起始位置，累计奖励与步数归零，开始回合循环。检查状态是否为目标状态，每个回合迭代智能体改变状态所经历的步骤。智能体每次迭代执行一步动作以改变状态。为学习环境，算法采用 epsilon-greedy 策略平衡探索与利用：以较小的 `epsilon` 概率随机选取动作，其余时间选择使 Q 值最大化的动作。每一步给予 `-1` 奖励；若下一步到达目标状态，给予 `10` 奖励；若 `next_step` 与障碍物重合，给予 `-5` 奖励以抑制此类移动。接着计算目标值 `Target = r + gamma * Qmax`，即即时奖励与下一状态最大 Q 值折扣和。此目标值用于更新当前状态-动作对的 Q 值。Q 值表示在状态 `s` 执行动作 `a` 的预期累计未来奖励，更新公式为 `Q_values = Q_t + learning_rate * (target – Q_t)`。每次迭代更新累计奖励、`next_state` 与步数，如 `q_learning` 函数所示。`Q_learning` 方法基于接收到的奖励更新 Q 值，迭代指定回合数，每回合包含状态转移直至到达目标状态。此处采用 epsilon-greedy 探索选择动作，故有时会采取次优动作，但 Q 值存储长期前景。下面代码展示用 Python 实现 Q-Learning 的步骤。

### 函数定义与初始化

此片段定义 `q_learning` 函数并初始化必要变量：`rewards` 存储每回合总奖励，`steps_per_episode` 存储每回合步数，`policy_progress` 在指定检查点记录策略。若未提供参数则设置默认值。

```
def q_learning(self, num_episodes=1000, learning_rate=0.1, discount_factor=0.9, epsilon=0.1, policy_checkpoints=None):
    rewards = []
    steps_per_episode = []
    policy_progress = []  # Store optimal policy at different checkpoints
    if policy_checkpoints is None:
        policy_checkpoints = [num_episodes // 1000, num_episodes // 100, num_episodes // 50, num_episodes - 1]
```

### 回合循环

此片段启动回合主循环。每回合将状态初始化为起始位置，总奖励与步数归零，并创建列表记录访问过的状态。

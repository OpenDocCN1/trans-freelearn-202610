
# DQN Training Implementation

## Optimizer and Loss Function

These two moments are used to adapt the learning rate for each parameter, making `Adam` well-suited for dealing with sparse gradients, noisy data, and large datasets. Based on the learning rate, the weights of the model are updated with respect to the gradient. A small learning rate (like `0.001`) ensures that the model converges slowly, making it less likely to overshoot the minimum of the loss function. We initialize the `Adam` optimizer with a learning rate of `0.001` and applies it to the parameters of the Q-network (`q_net`) as follows:

`optimizer = optim.Adam(q_net.parameters(), lr=0.001)`

Generally, the metric to track the quality of model training is the loss function, which is the difference between target value and model predicted value. In context of the DQN algorithm, and specifically here, the loss function is the mean squared error loss, which is the mean squared error (MSE) between the Q-values predicted by the Q-network and the target values computed by the Bellman equation. We initialize the `MSELoss` function as:

`loss_fn = nn.MSELoss()`

## Step 7: Replay Memory

A `ReplayMemory` class is implemented to store and manage experiences (transitions) encountered by the agent. This memory allows the agent to sample and reuse past experiences during training, which helps break the correlation between consecutive experiences and stabilize learning.

```
class ReplayMemory:
    def __init__(self, capacity):
        self.memory = deque(maxlen=capacity)
    def push(self, transition):
        self.memory.append(transition)
    def sample(self, batch_size):
        return random.sample(self.memory, batch_size)
    def __len__(self):
        return len(self.memory)
```

The memory is implemented as a `deque` (double-ended queue) with a fixed capacity. The `push(transition)` function adds a new experience to the memory. The `sample(batch_size)` function randomly samples a batch of experiences for training, and `__len__` method returns the current size of the memory.

## Step 8: Environment Reset

At the start of each episode, we reset the environment, initializing the state, total reward, and done flag. The states can be randomized or have some gradual changing values depending on the training strategy.

```
def initialize_environment(env):
    state, _ = env.reset()
    total_reward = 0
    done = False
    return state, total_reward, done
```

## Step 9: Select Action

In previous chapters, we discussed importance of using an exploration-exploitation strategy. We will continue to use epsilon greedy strategy, where for a small probability, we will randomly pick an action within the allowed range, and for the rest of the times we will pick the action that has maximum Q value. The implementation of the `select_action` function is given below.

```
def select_action(q_net, state, epsilon, env):
    if random.random() > epsilon:
        with torch.no_grad():
            action = q_net(state).max(1)[1].item()
    else:
        action = env.action_space.sample()
    return action
```

## Step 10: Store Experience

During training based on DQN algorithm, we store experiences to a replay buffer and sample them as a batch from there for updating Q-network. The `store_experience` function stores data at each iteration consistently to the replay buffer.

```
def store_experience(memory, state, action, reward, next_state, done):
    memory.push((state, action, reward, next_state, done))
```

## Step 11: Update Q-Network

This function samples a batch of experiences from replay memory and computes the loss between the current Q-values and the expected Q-values. `current_q_values` are the predicted Q-values for the actions taken. `max_next_q_values` are the maximum predicted Q-value for the next state from the target network, and the `expected_q_values` are calculated using the Bellman equation. The loss is backpropagated to update the Q-network’s weights.

```
def update_q_network(q_net, target_net, memory, optimizer, loss_fn, gamma, batch_size):
    if len(memory) < batch_size:
        return
    transitions = memory.sample(batch_size)
    batch_state, batch_action, batch_reward, batch_next_state, batch_done = zip(*transitions)
    batch_state_tensor = torch.FloatTensor(batch_state)
    batch_action_tensor = torch.LongTensor(batch_action).unsqueeze(1)
    batch_reward_tensor = torch.FloatTensor(batch_reward)
    batch_next_state_tensor = torch.FloatTensor(batch_next_state)
    batch_done_tensor = torch.FloatTensor(batch_done)
    current_q_values = q_net(batch_state_tensor).gather(1, batch_action_tensor)
    max_next_q_values = target_net(batch_next_state_tensor).max(1)[0].detach()
    expected_q_values = batch_reward_tensor + (gamma * max_next_q_values * (1 - batch_done_tensor))
    loss = loss_fn(current_q_values.squeeze(), expected_q_values)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```

If the replay memory has accumulated enough transitions (at least `batch_size`), a batch of experiences is sampled from the memory. The Q-network is then updated by:

- Computing the predicted Q-values for the current states and actions.
- Calculating the expected q values from predicted current q values from `q_net` and the predicted Q-values from the `target_net` for the next states.
- Computing the loss between the predicted and target Q-values using the specified loss function.
- Backpropagating the loss and updating the Q-network’s weights using the optimizer.

## Step 12: Update Target Network

To keep up with the progress made by the predictor, the target network needs to be periodically updated. We implement an update function for the target network also.

```
def update_target_network(q_net, target_net):
    target_net.load_state_dict(q_net.state_dict())
```

Note that by update, here we are simply updating the weights of the network.

## Step 13: Decay Epsilon

As the network gathers experience based on training episodes, and makes progress in its policies, we schedule a reduction in exploration and let the agent do more exploitation. The `decay_epsilon` function decays the value of epsilon over time to reduce exploration as the agent becomes more confident in its actions.

```
def decay_epsilon(epsilon, epsilon_end, epsilon_start, episode, epsilon_decay):
    return max(epsilon_end, epsilon_start - episode / epsilon_decay)
```

Now that we got all modularized steps written as functions, we can use them in the training loop. The next section discusses the main training loop.

## Training Loop

The main training loop in the DQN algorithm is where the actual learning happens. The loop iteratively interacts with the environment, collects experiences, and uses these experiences to update the Q-network. Here are the steps within the training loop, using the above functions and classes:

1. **Inputs and Configurations**: We first assign or input all the training configuration parameters to the function.

```
def train_dqn(q_net, target_net, env, optimizer, loss_fn, num_episodes=1000, gamma=0.99,
              epsilon_start=1.0, epsilon_end=0.1, epsilon_decay=500, batch_size=32, target_update_freq=10):
```

2. **Initialize Replay Memory and Epsilon**: A `ReplayMemory` instance is initialized to store experiences `(state, action, reward, next_state, done)` that the agent collects as it interacts with the environment. `epsilon_start` sets the initial value for epsilon, which controls the exploration-exploitation trade-off. Higher epsilon means more exploration (random actions), while lower epsilon leads to more exploitation (actions based on the Q-network’s knowledge).

```
    memory = ReplayMemory(capacity=10000)
    epsilon = epsilon_start
    rewards = []
```

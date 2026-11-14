
## DQN Training Loop

1.  **Loop Over Episodes**: The loop runs for `num_episodes`, where each episode is an attempt by the agent to solve the task or environment (e.g., move the car to the top of the hill in the MountainCar-v0 environment).

```python
for episode in range(num_episodes):
```

1.  **Initialize the environment and start episode loop**: At the start of each episode, the environment is initialized by resetting to its initial state using `env.reset()`, and the agent's total reward for the episode is set to 0. The `done` flag is initialized to `False` to indicate that the episode is ongoing. The episode runs in a `while` loop until conditions are met, that is `done = True`. The state output from gym environment initialization is converted to a tensor type.

```python
state, total_reward, done = initialize_environment(env)
while not done:
    state_tensor = torch.FloatTensor(state).unsqueeze(0)
```

1.  **Action selection and assignment**: The agent selects an action based on the current state. This decision is made using the epsilon-greedy strategy: With probability `epsilon`, the agent explores by choosing a random action. For the rest of the cases with probability `1-epsilon`, the agent exploits what it learned by choosing the action that maximizes the Q-value predicted by the `q_net` for the current state.

```python
action = select_action(q_net, state_tensor, epsilon, env)
```

1.  **Execute action and observe states**: The selected action is executed in the environment using `env.step(action)`, which returns:

    *   `next_state`: The state the environment transitions to.
    *   `reward`: The reward received for taking the action.
    *   `done`: A boolean flag indicating if the episode has ended.
    *   `trunc`: A flag for episode truncation (can be treated as done).
    *   `info`: Additional information provided by the environment (often ignored).

```python
next_state, reward, done, trunc, info = env.step(action)
done = done or trunc
```

1.  **Store Transition in Replay Memory**: The transition `(state, action, reward, next_state, done)` is stored in the replay memory. This experience will be used later for training the Q-network.

```python
store_experience(memory, state, action, reward, next_state, done)
```

1.  **Optimize and update**: The state is first updated with next state, similarly total reward is updated, and then the q network is optimized and updated.

```python
state = next_state
total_reward += reward
update_q_network(q_net, target_net, memory, optimizer, loss_fn, gamma, batch_size)
```

1.  **Update the Target Network**: Every periodic increase of the number of episodes by target update frequency, `target_update_freq`, the weights of the `q_net` are copied to the `target_net`. This step ensures stability in learning by decoupling the network used for estimating future rewards from the network being updated.

```python
if episode % target_update_freq == 0:
    update_target_network(q_net, target_net)
```

1.  **Decay Epsilon**: `epsilon` is decayed after each episode to gradually shift the agent's behavior from exploration to exploitation. This decay is crucial for allowing the agent to learn from its experiences and rely more on the Q-network as it improves:

```python
epsilon = decay_epsilon(epsilon, epsilon_end, epsilon_start, episode, epsilon_decay)
```

1.  **Log and Store Results**: The total reward for each episode is stored in the `rewards` list. Additionally, progress is logged every 100 episodes, showing the episode number, total reward, and current `epsilon` value.

```python
rewards.append(total_reward)
if episode % 100 == 0:
    print(f'Episode {episode}, Total Reward: {total_reward}, Epsilon: {epsilon:.2f}')
```

At this point the rewards are returned as output:

```python
return rewards
```

## Additional Model Training Utilities

In addition to the training function, a few more functions are necessary for re-using the training models. For complex environments, training can be a time-consuming process, and saving the model, plotting the training progress, and logging outputs might be necessary. Similarly, before deploying trained models, loading the trained model and running inference on trained models might be necessary. We will cover those additional functions next.

### Saving the Model

The `save_model` function is used to save the trained model's parameters (weights and biases) to a file. This allows you to save the state of the model after training, so you can load it later without retraining from scratch. `model.state_dict()` method returns a dictionary containing the model's parameters. The state dictionary includes all the learnable parameters (e.g., weights and biases) of the model's layers.

```python
def save_model(model, path):
    torch.save(model.state_dict(), path)
```

The `torch.save` function takes the state dictionary and saves it to a file specified by `path`. This file is typically saved with a `.pth` or `.pt` extension, which is a common convention in PyTorch.

### Load Model

The `load_model` function is used to load the model's parameters from a file, restoring the model to its previously saved state for further use, such as inference or additional training. `torch.load(path)` loads the state dictionary (i.e., the model's parameters) from the file specified by `path`.

```python
def load_model(model, path):
    model.load_state_dict(torch.load(path))
    model.eval()
```

`model.load_state_dict(...)` method loads the state dictionary into the model, setting the model's parameters to the values that were saved. `model.eval()` method sets the model to evaluation mode. In evaluation mode, certain layers like dropout and batch normalization behave differently compared to training mode, ensuring the model operates correctly when making predictions.

### Plot Rewards

The `plot_rewards` function is used to visualize the total rewards accumulated by the agent over the episodes during training. This plot helps to assess the agent's learning progress.

```python
def plot_rewards(rewards):
    plt.plot(range(len(rewards)), rewards)
    plt.xlabel('Episode')
    plt.ylabel('Total Reward')
    plt.title('Training Rewards over Episodes')
    plt.show()
```

### Logging

To keep track of model quality and progress, some type of analytics and visualization beyond plotting reward progress can be useful. For this, we add a logging function.

```python
import logging

def log_training_info(episode, action, reward, state, next_state, done, trunc):
    logging.info(f"Episode: {episode}, Action: {action}, Reward: {reward}, "
                 f"State: {state}, Next State: {next_state}, Done: {done}, Trunc: {trunc}")
```

This logging function can be added within the training loop right after the `env.step()` method. These utilities enhance the overall workflow by providing tools to manage and analyze the model effectively. Training the DQN script `python torch_dqn.py --train`, using the execution code as below, will train the agent, and save the q network model:

```python
if __name__ == "__main__":
    print(args.train)
    if args.train:
        rewards = train_dqn(q_net, target_net, env, optimizer, loss_fn)
        # # Saving the model
        save_model(q_net, 'q_network_tgt_test.pth')
        # # Plotting training rewards
        plot_rewards(rewards)
    else:
        load_model(q_net, 'q_network_tgt_test.pth')
        inference(env, q_net, episodes=10)
```

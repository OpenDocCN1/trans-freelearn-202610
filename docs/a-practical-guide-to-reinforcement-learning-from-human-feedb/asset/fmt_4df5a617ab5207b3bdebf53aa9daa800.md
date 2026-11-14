
# PPO Agent Implementation and Training

## get_action_and_value Method

```
def get_action_and_value(self, x, action=None):
    action_mean = self.actor_mean(x)
    if self.is_discrete:
        # For discrete action spaces, use a softmax layer
        action_probs = nn.functional.softmax(action_mean, dim=-1)
        if action is None:
            action = action_probs.multinomial(num_samples=1).squeeze(1)
        log_prob = action_probs.log()[range(action.size(0)), action]
        entropy = -(action_probs * action_probs.log()).sum(dim=-1)
    else:
        # For continuous action spaces
        action_logstd = self.actor_logstd.expand_as(action_mean)
        action_std = torch.exp(action_logstd)
        probs = Normal(action_mean, action_std)
        if action is None:
            action = probs.sample()
        log_prob = probs.log_prob(action).sum(1)
        entropy = probs.entropy().sum(1)
    return action, log_prob, entropy, self.critic(x)
```

For discrete actions, the `Softmax Layer` computes probabilities for each action, the action sampling samples an action or uses the provided action, and then calculates `log_prob` and `entropy` for training. For continuous actions, the normal distribution uses the `action_mean` and `action_std` to define a Gaussian distribution for actions, action sampling samples from this distribution if no action is provided, and then calculates `log_prob` and `entropy`.

## layer_init Method

```
@staticmethod
def layer_init(layer, std=np.sqrt(2), bias_const=0.0):
    nn.init.orthogonal_(layer.weight, std)
    nn.init.constant_(layer.bias, bias_const)
    return layer
```

The `Layer Initialization` (`layer_init` method) method initializes the neural network layers with specific configurations. It uses orthogonal initialization for weights to stabilize training and sets a constant value for biases, ensuring that the layers are prepared for effective learning.

## Agent Class Overview

The `Agent` class effectively combines actor-critic methods, adapting to both discrete and continuous action spaces. Each component—from initialization to action selection—plays a crucial role in how the agent learns and interacts with the environment. This structured approach allows the agent to efficiently learn policies that maximize rewards through reinforcement learning. Note that here we are adopting the `cleanRL` architecture, rather than experimenting with a new one. But, for a problem such as continuous mountain car with sparse rewards, a model architecture involving memory such as `LSTM` (long short term memory) or `GRU` (gated recurrent units) can help retain knowledge from earlier states of the episode. For now, we will continue with the standard architecture used in the libraries.

## Training Parameters (args.yaml)

PPO algorithm has several training parameters that range from `env_id` and `num_envs` related to environment, to number of steps for training `num_steps`, to learning rate `learning_rate`. All these are added in a separate configuration file called `args.yaml`. This helps organize the code and these can be used for experimenting with. Here, we are using the parameters adopted from `cleanRL` repository, and are tuned already.

## PPO Training (training.py)

The detailed training loop is provided in the `training.py` file in the github repository. Here we explain key algorithmic steps of the training loop for PPO.

### Training Setup and Initialization

PPO training requires preparing the parameters for training progress. Considering that we are using vectorized environments, this involves defining the following parameters:

*   `Batch Size`, total number of time steps collected across all environments.
*   `Minibatch Size`, for each update of the network, this is the size of the minibatch sampled from the collected batch.
*   `Iterations`, total number of iterations depends on how many time steps we plan to take across training.

```
batch_size = int(args['num_envs'] * args['num_steps'])
minibatch_size = batch_size // args['num_minibatches']
num_iterations = args['total_timesteps'] // batch_size
```

The goal is to prepare the optimizer and define how training progresses.

### Checkpointing and Restarting

For long training sessions, it is common that training might need to be stopped, evaluated using inference, and continued further or resumed from where it was stopped. This helps with not having to restart training from scratch.

```
latest_checkpoint = get_latest_checkpoint(checkpoint_dir)
if latest_checkpoint:
    checkpoint = load_model(agent, latest_checkpoint)
    global_step = checkpoint.get('global_step', 0)
    start_iteration = checkpoint.get('start_iteration', 0)
else:
    global_step = 0
    start_iteration = 0
```

Checkpoints of the model are saved at regular intervals so that training progress is saved and further training can continue from the last saved model (checkpoint).

### Annealing Learning Rate

Since our goal is to have stable learning, it is often beneficial to reduce the learning at later stages of training.

```
if args['anneal_lr']:
    frac = 1.0 - (iteration - 1.0) / num_iterations
    optimizer.param_groups[0]["lr"] = frac * args['learning_rate']
```

The learning rate is gradually decreased over time, encouraging stable convergence. This is typical in PPO to avoid excessive exploration in later stages of training.

### Collecting Rollouts

Rollouts involve letting the environment step through a number of steps, generating data (`obs`, `actions`, `logprobs`, `rewards`, `values`), which can be collected for training.

```
for step in range(args['num_steps']):
    with torch.no_grad():
        action, logprob, _, value = agent.get_action_and_value(next_obs)
    next_obs, reward, terminations, truncations, infos = envs.step(action.cpu().numpy())
    rewards[step] = torch.tensor(reward).to(device).view(-1)
```

Note that `torch.no_grad()` ensures that we are not calculating gradients here, as we are only collecting data. Another point to note is that some environments might terminate sooner than others. For example, some successful episodes might climb the mountain and reach the flag and terminate. Since we are using vectorized environments, the training of environments does not end. But we are keeping a log of `terminations` and `truncations`, and using them in calculating advantages and returns.

### Compute Advantages and Returns

As discussed in previous section, for stability of learning, we need to calculate advantages and returns. Here, `GAE` helps compute a more stable advantage by reducing variance in the reward signal.

```
next_value = agent.get_value(next_obs).reshape(1, -1)
advantages, returns = compute_advantages_and_returns(rewards, values, next_value, next_done, dones, args)
```

# Report
---
This project used the DDPG (Deep Deterministic Policy Gradient) implementation from [DDPG-Bipedal Udacity project repo](https://github.com/udacity/deep-reinforcement-learning/tree/master/ddpg-bipedal).

## State and Action Spaces
In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector must be a number between -1 and 1.

## Learning Algorithm

The agent training is done in the `ddpg` function in the `DDPG_Continuous_Control` notebook.

It continues episodical training via the the ddpg agent until `n_episodes` is reached or until the environment is solved. The  environment is considered solved when the average reward (over the last 100 episodes) is at least +33.0. Note that if the number of agents is >1 then the average of all agents' reward at that time was used.

Each episode continues until `max_step` timesteps is reached or if there's any done returned.

As above, a reward of +0.1 is provided for each step that the agent's hand is in the goal location.

The DDPG agent is defined in [`ddpg_multiple_agents.py`](https://github.com/HyunTruth/drlnd-p2/blob/master/ddpg_multiple_agents.py)

For each time step and agent the Agent acts upon the state utilising a shared (at class level) `replay_buffer`, `actor_local`, `actor_target`, `actor_optimizer`, `critic_local`, `criticl_target` and `critic_optimizer` networks.

### DDPG Hyper Parameters
- n_episodes (int): maximum number of training episodes
- max_step (int): maximum number of timesteps per episode

### DDPG Agent Hyper Parameters

BUFFER_SIZE = int(1e6)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 5e-3              # for soft update of target parameters
LR_ACTOR = 1e-3         # learning rate of the actor
LR_CRITIC = 1e-3        # learning rate of the critic
WEIGHT_DECAY = 1e-6     # L2 weight decay

For the major hyperparameters I tried out different values from ShangtongZhang's implementation and above bipedal implementation, in different combinations.

### Neural Networks

Actor and Critic network models were defined in [`model.py`](https://github.com/HyunTruth/drlnd-p2/blob/master/model.py).

The Actor network architecture is three fully connected layers with 128 and 128 units, going through batch normalization & relu activation. Tanh activation was used to derive the action from action space. The network has an initial dimension the same as the state size.

The Critic network architecture is three fully connected layers with 128 and 128 units with relu activation. Batch normalization was done only for the 1st layer.  The critic network has  an initial dimension the size of the state size plus action size.

## Plot of rewards

This training was performed gradually, changing manually some hyper parameters mentioned above. 

Checkout the reward plot of my best run here:
 ![Reward Plot](https://github.com/HyunTruth/drlnd-p2/blob/master/result.PNG)


## Ideas for Future Work
The performance of the agents might be improved by considering the following:

- Various architecture optimization

Many of the architecture related improvements such as the network architectures (number of layers and neurons), fc layer size, batch norm, and the activation function could be further tuned to improve performance.

- Alternative learning algorithms

Alternative methods include PPO, A2C, A3C and D4PG algorithm. We could try them and compare.

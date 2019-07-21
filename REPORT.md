# Project 2: Continuous control

## Objective
The objective of this project is to implement a Deep Deterministic Policy Gradients (DDPG) algorithm to maintain the position of a double-jointed at the target location for as many time steps as possible.

In order to solve the environment, your agent must get an average score of +30 over 100 consecutive episodes.

## Environment description 
The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

## Agent Implementation

The "Reacher" environment is a continuous environment compared to the "Banana" environment that we used in project 1. A continuous space indicates that there are infinite range of action values to control the arm.

Therefore, a policy-based method needs to be used to solve the environment instead of a value-based method.

The agent in the project was built on the DDPG agent in the Actor-Critic Methods tutorial.

### Actor-Critic Method

The Actor-Critic method contains two networks:

1. Actor network that learns how to react by estimating the best policy and maximum the reward
2. Critic network that learns how to state-action pair values

The Actor-Critic method combines the two networks to accelerate the learning process.

#### Actor Model

`Layers:`
-       self.fc1 = nn.Linear(33, 256)
        self.fc2 = nn.Linear(256, 256)
        self.fc3 = nn.Linear(256, 4)
        self.bn = nn.BatchNorm1d(fc1_units)
`Forward:`
-       def forward(self, state):
            if state.dim() == 1:
                state = torch.unsqueeze(state,0)
            x = F.relu(self.fc1(state))
            x = self.bn(x)
            x = F.relu(self.fc2(x))
            return F.tanh(self.fc3(x))


#### Critic Model
`Layers:`
-       self.fc1 = nn.Linear(33, 256)
        self.fc2 = nn.Linear(256+4, 256)
        self.fc3 = nn.Linear(256, 1)
        self.bn = nn.BatchNorm1d(fc1_units)
`Forward:`
-       def forward(self, state, action):
            if state.dim() == 1:
                state = torch.unsqueeze(state,0)
            x = F.relu(self.fcs1(state))
            x = self.bn(x)
            x = torch.cat((x, action), dim=1)
            x = F.relu(self.fc2(x))
            return self.fc3(x)


#### Experience Reply

The agent utilize a reply buffer to obtain experience tuples wish state, action, reward, next state, and done to learn from the past experience.

The experiences are sampled randomly to avoid over-fitting and bias.

## Result

The agent was able to archive the an average reward of 30 over the last 100 episode with 544 episodes

The scores diagram can be found in Continuous_Control.ipynb notebook

## Future work
- Experiment with dropout layers on the agent
- Experience replay prioritization, select experience based on priority value instead of random samepling
- Hyper-parameters fine tuning
- Experiment with other methods, such as PPO, A3C, and D4PG




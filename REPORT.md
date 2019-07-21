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
-       self.fc1 = nn.Linear(33, 128)
        self.fc2 = nn.Linear(128, 128)
        self.fc3 = nn.Linear(128, 128)
`Forward:`
-       def forward(self, state):
            x = F.relu(self.fc1(state))
            x = F.relu(self.fc2(x))
            return F.tanh(self.fc3(x)) 


#### Critic Model
`Layers:`
-       self.fc1 = nn.Linear(33, 128)
        self.fc2 = nn.Linear(128+4, 128)
        self.fc3 = nn.Linear(128, 1)
`Forward:`
-       def forward(self, state, action):
            x = F.relu(self.fcs1(state))
            x = torch.cat((x, action), dim=1)
            x = F.relu(self.fc2(x))
            return self.fc3(x)


#### Experience Reply

The agent utilize a reply buffer to obtain experience tuples wish state, action, reward, next state, and done to learn from the past experience.

The experiences are sampled randomly to avoid over-fitting and bias.

## Result

The agent was able to archive the goal with 96 and 119 episodes, however, without the reset_parameters method to initialize the weights, the agent was not able to reach the goal even with 200 episodes.

It seems like weights initialization plays great part in the training.

    def reset_parameters(self):
        self.fc1.weight.data.uniform_(*hidden_init(self.fc1))
        self.fc2.weight.data.uniform_(*hidden_init(self.fc2))
        self.fc3.weight.data.uniform_(-3e-3, 3e-3)


## Future work
- Experiment with dropout layers on the agent
- Experience replay prioritization, select experience based on priority value instead of random samepling
- Hyper-parameters fine tuning
- Experiment with other methods, such as PPO, A3C, and D4PG




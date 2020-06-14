# Project 2: Control Navigation
### Author: Angelo Antonio Manzatto 
------------------------------
### Introduction

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of the agent is to maintain its position at the target location for as many time steps as possible.

### Space State
------------------------------------------------------

The state space has 33 dimensions and contains the agent's  to position, rotation, velocity, and angular velocities of the arm.

### Action State
------------------------------------------------------

Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1

### Acceptance Criteria
------------------------------------------------------
The task is episodic, and in order to solve the environment, your agent must get an average score of +30 over 100 consecutive episodes.

### Algorithm 
------------------------------------------------------
This project uses a **Deep Deterministic Policy Gradients (DDPG) ** algorithm to train an agent' arm on reaching the target destination.

DDPG is an algorithm which concurrently learns q Q-funciona and a policy. It uses two models, one called Actor and the other called critic that directly maps states to actions intead of outputting the probability distribution across a discrete action space. 

Also it has two target networks that are time-delayed copies of their original networks that slowly track the learned networks in order to improve stability while learning.

The steps that the algorithm performs can be seeing as below:

![Plot](/images/ddpg-algorithm.png)

Also we use s replay buffer to sample past experiences for learning to remove the strong correlation between sequence of action that results in a deterministic state helping to learng how to act in different situations that are not frequent.

The parameter from the target network are updated using a process called "soft update" that uses part of the learned network in order to slowly update its own weights.

Since we are learning a continuous space instead of a discrete one the exploration is done by adding noise to the action itself using a method called Ornsterin-Uhlenbeck Process.

### Model 
------------------------------------------------------
Two Aritficial Neural Network models (Actor and Critic) were used on this project contains just fully connected layers using an output with Tanh activation in order to constraint the arm movement between -1 and 1

Actor Model:

* FC Layer 1 (states, 256)
* Batch Normalization 1D (256)
* FC Layer 2 (256, 256)
* FC Layer 3 (128, actions)
* Critic Model

Critic Model:

* FC Layer 1 (states, 256)
* Batch Normalization 1D (256)
* Concatenate(states + actions)
* FC Layer 2 (states + actions, 128)
* FC Layer 3 (128, actions)

The number of states are 33 while for the is a vector with 4 movements that need to be converted from discrete space to continuous space with a range that goes from -1 to +1 using the Critic Model.

###  Hyperparameters
------------------------------------------------------
The parameters used during training were:

| Hyperparameter | Value     | Description |
| ----------- | ----------- | ----------- |
|BUFFER_SIZE | 1e6|replay buffer size|
|BATCH_SIZE | 128 |minibatch size|
|GAMMA | 0.95 |discount factor|
|TAU | 1e-3|for soft update of target parameters|
|LR_ACTOR| 1e-4| learning rate|
|LR_LR_CRITIC| 1e-3| learning rate|
|WEIGHT_DECAY| 0 |how the learning rate should decay over time|

### Evalutation
-----------------------------------------------------
The problem was solved after 863 episodes.

Below we can see  a plot of rewards per episode.

![Plot](/images/results.png)

### Ideas for the future
-----------------------------------------------------
The next step towards a more rewarding challenge should be trainning the 20 agents simultaneously.

Use a framework like Robot Operating System (ROS) and try to teach an arm over there in a simulated environment.

There is also other more advanced Deep Learning Algorithms that could be put on test for this challenge like:
* PPO
* A3C, A2C
* D4PG

The ultimate challenge would be to build a C++ solution instead of a Python, but first I am buing a simple hpp file header that implements deep learning that can be found at: https://github.com/AngeloManzatto/ANN

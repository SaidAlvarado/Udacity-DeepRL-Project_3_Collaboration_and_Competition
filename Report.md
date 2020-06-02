# Udacity's Deep RL Nanodegree - Project 2: Continuous Control

## Technical Report



In this report we will talk in detail about the algorithms and techniques used to solve this Reinforcement Learning scenario.


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365478-9c47b600-a3a8-11ea-92b0-a750598a70e7.gif" alt="Trained Agent"/>
</p>



### Problem Statement



#### Environment

In this scenario an RL-agent is tasked with controlling a two-jointed arm as it moves to track the position of a target circling around it. The environment was made with Unity's ML-Agents framework and there are two variations: One environment has 20 agents operating individually for parallel learning, and the other one only has a single agent. The task looks roughly as follows:


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365528-f2b4f480-a3a8-11ea-9f84-b5ed18bcd9b7.png"/>
  
</p>




#### Rewards
This environment provides the following rewards:
A reward of **`+0.1`** is provided for each step that the agent's hand is in the goal location.



#### Actions

This is a **Continuous action** scenario. As such, each action is a vector with four numbers corresponding tothe torque applicable to the two joints. Every entry in the action vector should be a number between **`-1`** and **`1`**.



#### State Space

The state space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. 

---

### Deep Deterministic Policy Gradient (DDPG)



This exercise was solved using as a base the Deep Deterministic Policy Gradient techniques from the following [paper](https://arxiv.org/abs/1509.02971). 

> TP Lillicrap, JJ Hunt, A Pritzel, *et al.* Continuous control with deep reinforcement learning (2015). 



Which implements the following algorithm:



<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365536-19732b00-a3a9-11ea-9dd1-1d03e7dc7d53.png" width="70%" height="70%" alt="Algorithm description"/>
</p>

This works particularly well for the current environment given that both its **State Space** and **Action Space** are continuous.



#### Neural Network.

Since this is arguably an Actor-Critic method, we require 2 Neural Networks. One to estimate the best action for a particular state and another one to estimate the Value Function. Each of these must have a duplicate network which will serve as the _Target_ during training. Given that the input is not an image, there is no need to use a Convolutional Architecture. Instead, it is sufficient to have networks with two fully connected RELU internal layers ending with a Tanh function and linear function for the actor and the critic respectively.

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365580-71aa2d00-a3a9-11ea-8929-dc6b7357150c.png" alt="Neural Network"/>
</p>



#### Experience Replay

All steps' **`(State, Action, Reward, Next State)`**   tuples are saved in to a queue in memory. Every **`20`** time steps **`10`** learning passes are performed, in which a mini-batch of **`256`** tuples are selected to update the Neural Network weights. 

#### Ornstein-Uhlenbeck Noise

Since this is as scenario with a continuous action space, it is not possible to use the Epsilon-greedy method of adding randomness to the actions in order to encourage exploration of the state-action space. To substitute this we can use the Ornstein-Uhlenbeck process to add some variance to the decisions of the algorithm.

**IMPORTANT NOTE:** If the environment with multiple agents is going to be used, it is imperative that the output dimension of the noise process is adjusted to generate the correct shape of noise for all the agents simultaneously, namely, **`(20,4)`**. **Otherwise, the agent's training will not converge** irrespective of how you adjust all other hyper-parameters.


#### Target Network Soft Updates

Unlike other methods which update the target network by directly copying the parameters of the local network, this algorithm slowly mixes the weights of the target network and the local network by **`0.1%`** each timestep.




#### Selected Hyper-parameters

The code uses the following Hyper-parameters:

- **`Number of Hidden Layers`**  =  2
- **`Neurons in 1° layer`**  =  300 
- **`Neurons in 2° layer`**  =  200 
- **`Gamma`**  =  0.99
- **`TAU`**  =  1e-3
- **`Actor Learning Rate`**  =  2e-4
- **`Critic Learning Rate`**  =  1e-4
- **`Steps per update`**  =  20
- **`Updates performed per step`**  =  10
- **`Batch Size`**  =  256

---

### Results

When simulated, we receive the following plot of score over episodes.


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365607-9bfbea80-a3a9-11ea-87d5-afb993146a08.png"/>
</p>



As it can be seen, the algorithm achieves an average score of 30 in about 124 episodes, effectively solving the Task.


---
### Future work

There are several ways to improve this algorithm. As it was currently implemented, in the update process of the Critic network the Expected returns are calculated using a 1-step Bootstraping TD estimation. It would be interesting enhance the algorithm with a Generalized Advantage Estimation, such as Lambda Return.


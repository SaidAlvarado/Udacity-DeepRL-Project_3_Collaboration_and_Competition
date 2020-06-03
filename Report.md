# Udacity's Deep RL Nanodegree - Project 2: Continuous Control

## Technical Report



In this report we will talk in detail about the algorithms and techniques used to solve this Reinforcement Learning scenario.


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365478-9c47b600-a3a8-11ea-92b0-a750598a70e7.gif" alt="Trained Agent"/>
</p>



### Problem Statement



#### Environment

In this scenario an RL-agent controls two rackets to bounce a ball over a net, somewhat like playing Ping-Pong. The environment was made with Unity's ML-Agents framework. The task looks roughly as follows:


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365528-f2b4f480-a3a8-11ea-9f84-b5ed18bcd9b7.png"/>
  
</p>




#### Rewards
This environment provides the following rewards:

- If an agent hits the ball over the net, it receives a reward of  **+0.1**.
- If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of **-0.01**.
- No rewards are provided in a per-time-step basis.



#### Actions

Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. it looks the following.

**`[Racket Movement, Racket Jump]`**

- **`Racket Movement`**: **Positive** values move the racket at a constant speed towards the net, **Negative** values move it away from the net.
- **`Racket Jump`**: Values larger than **0.5** trigger a Jump, Values lower or equal than **0.5** do nothing.

Every entry in the action vector should be a number between **`-1`** and **`+1`**.



#### State Space

The observation space consists of **`8`** variables corresponding to the position and velocity of the ball and racket, the environment returns 3 stacked observation spaces at each timestep, son the returned variable has dimension **`24`**.
The vector has the following variables:
<br>
**`[Racket Pos X, Racket Pos Y, Racket Vel X, Racket Vel Y, Ball Pos X, Ball Pos Y, Racket Vel X, Racket Vel y]`**

(For some reason it seems that the last two elements are a repeat of the third and fourth elements.)

---

### Deep Deterministic Policy Gradient (DDPG)



This exercise was solved using as a base the Deep Deterministic Policy Gradient techniques from the following [paper](https://arxiv.org/abs/1509.02971). And adapting it for two simulateously cooperating agents.

> TP Lillicrap, JJ Hunt, A Pritzel, *et al.* Continuous control with deep reinforcement learning (2015). 



Which implements the following algorithm:



<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365536-19732b00-a3a9-11ea-9dd1-1d03e7dc7d53.png" width="70%" height="70%" alt="Algorithm description"/>
</p>

This works particularly well for the current environment given that both its **State Space** and **Action Space** are continuous.



#### Environment's frame of reference

The environment is programmed in such a way that the observations are provided a pair at a time one for each racket, and the data is always presented from the perspective of the local frame of each respective racket.

This means that the data is flipped so as to that X positive axis is alwas pointing towards the net.

This becomes useful as it is very easy to separatedly add the experience of each of the rackets to the experience buffer and use to train a single neural network that can control them both independently. 


(Small diagrama of where the zero is)


#### Experience Replay

All steps' **`(State, Action, Reward, Next State)`**   tuples  from each one of the rackets are saved in to a queue in memory. At each time steps **`2`** learning passes are performed, in which a mini-batch of **`256`** tuples are selected to update the Neural Network weights.


#### Neural Network.

Since this is arguably an Actor-Critic method, we require 2 Neural Networks. One to estimate the best action for a particular state (One racket at a time) and another one to estimate the Value Function. Each of these must have a duplicate network which will serve as the _Target_ during training. Given that the input is not an image, there is no need to use a Convolutional Architecture. Instead, it is sufficient to have networks with two fully connected RELU internal layers ending with a Tanh function and linear function for the actor and the critic respectively.

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365580-71aa2d00-a3a9-11ea-8929-dc6b7357150c.png" alt="Neural Network"/>
</p>



#### Ornstein-Uhlenbeck Noise

Since this is as scenario with a continuous action space, it is not possible to use the Epsilon-greedy method of adding randomness to the actions in order to encourage exploration of the state-action space. To substitute this we can use the Ornstein-Uhlenbeck process to add some variance to the decisions of the algorithm.

**IMPORTANT NOTE:** It's important to adjust the scale of the noise, if it is too large the movement of the rackets may become erratic and jittery. THis may prevent the training from converging. 


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



As it can be seen, the algorithm achieves an average score of 0.5 in about 804 episodes, effectively solving the Task.


---
### Future work

There are several ways to improve this algorithm. As it was currently implemented, in the update process of the Critic network the Expected returns are calculated using a 1-step Bootstraping TD estimation. It would be interesting enhance the algorithm with a Generalized Advantage Estimation, such as Lambda Return.




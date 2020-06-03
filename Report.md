# Udacity's Deep RL Nanodegree - Project 3: Collaboration and Competition

## Technical Report



In this report we will talk in detail about the algorithms and techniques used to solve this Reinforcement Learning scenario.


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83697644-d52d9800-a5ff-11ea-9f21-bbc9b0b93e9a.gif" alt="Trained Agent"/>
</p>



### Problem Statement



#### Environment

In this scenario an RL-agent controls two rackets to bounce a ball over a net, somewhat like playing Ping-Pong. The environment was made with Unity's ML-Agents framework. The task looks roughly as follows:


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83697685-f42c2a00-a5ff-11ea-9a09-07a3fde04525.png"  width="70%" height="70%"/>
  
</p>




#### Rewards
This environment provides the following rewards:

- If an agent hits the ball over the net, it receives a reward of  **+0.1**.
- If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of **-0.01**.
- No rewards are provided in a per-time-step basis.

<br>

#### Actions

Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. it looks like the following. **`[Racket Movement, Racket Jump]`**, where:

- **`Racket Movement`**: **Positive** values move the racket at a constant speed towards the net, **Negative** values move it away from the net.
- **`Racket Jump`**: Values larger than **0.5** trigger a Jump, Values lower or equal than **0.5** do nothing.

Every entry in the action vector should be a number between **`-1`** and **`+1`**.

<br>

#### State Space

The observation space consists of **`8`** variables corresponding to the position and velocity of the ball and racket, the environment returns 3 stacked observation spaces at each timestep, so the returned variable has **`24`** dimensions.
The vector has the following variables:
<br>
**`[Racket Pos X, Racket Pos Y, Racket Vel X, Racket Vel Y, Ball Pos X, Ball Pos Y, Racket Vel X, Racket Vel y]`**

(For some reason it seems that the last two elements are a repeat of the third and fourth elements.)

---

### Deep Deterministic Policy Gradient (DDPG)



This exercise was solved using as a base the Deep Deterministic Policy Gradient techniques from the following [paper](https://arxiv.org/abs/1509.02971). And adapting it for two simultaneously cooperating agents.

> TP Lillicrap, JJ Hunt, A Pritzel, *et al.* Continuous control with deep reinforcement learning (2015). 



Which implements the following algorithm:



<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365536-19732b00-a3a9-11ea-9dd1-1d03e7dc7d53.png" width="70%" height="70%" alt="Algorithm description"/>
</p>

This works particularly well for the current environment given that both its **State Space** and **Action Space** are continuous.



#### Environment's frame of reference

The environment is programmed in such a way that the observations are provided a pair at a time one, for each racket. The data is always presented from the perspective of the local frame of reference of each respective racket.

This means that the data is flipped so as to that X positive axis is always pointing towards the net.

This is useful as it is very easy to add the experience of each of the rackets to the experience buffer separately and use them to train a single neural network that can control them both independently. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83697730-132abc00-a600-11ea-8a73-d96ac2b7acbd.png"  width="70%" height="70%" />
  
</p>
Notice that the orientation of the frame of reference is reversed from one racket to the other in such a way that the rackets are always in the negative side of the X-axis. The direction of the action "Forward" is also reversed, and the net serves as the central zero-point of the X-axis


#### Experience Replay

All steps' **`(State, Action, Reward, Next State)`**   tuples  from each one of the rackets are saved in to a queue in memory. At each time steps **`2`** learning passes are performed, in which a mini-batch of **`256`** tuples are selected to update the Neural Network weights.


#### Neural Network.

Since this is arguably an Actor-Critic method, we require 2 Neural Networks. One to estimate the best action for a particular state (One racket at a time) and another one to estimate the Value Function. Each of these must have a duplicate network which will serve as the _Target_ during training. Given that the input is not an image, there is no need to use a Convolutional Architecture. Instead, it is sufficient to have networks with two fully connected RELU internal layers ending with a Tanh function and linear function for the actor and the critic respectively.

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83698046-bed40c00-a600-11ea-9c1c-a14d67dcdb43.png" alt="Neural Network"  width="70%" height="70%" />
</p>



#### Ornstein-Uhlenbeck Noise

Since this is as scenario with a continuous action space, it is not possible to use the Epsilon-greedy method of adding randomness to the actions in order to encourage exploration of the state-action space. To substitute this we can use the Ornstein-Uhlenbeck process to add some variance to the decisions of the algorithm.

**IMPORTANT NOTE:** It's important to adjust the scale of the noise, if it is too large the movement of the rackets may become erratic and jittery. This may prevent the training from converging. 


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
- **`Steps per update`**  =  1
- **`Updates performed per step`**  =  1
- **`Batch Size`**  =  256

---

### Results

When simulated, we receive the following plot of score over episodes.


<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83698095-e1662500-a600-11ea-889e-b4aab98b7329.png"/>
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83698130-f2169b00-a600-11ea-97c9-a68647b73ffe.png" width="70%" height="70%"/>
</p>


As it can be seen, the algorithm achieves an average score of 0.5 in about 804 episodes, effectively solving the Task.


---
### Future work

There are several ways to improve this algorithm. As it was currently implemented, in the update process of the Critic network the Expected returns are calculated using a 1-step Bootstraping TD estimation. It would be interesting enhance the algorithm with a Generalized Advantage Estimation, such as Lambda Return.




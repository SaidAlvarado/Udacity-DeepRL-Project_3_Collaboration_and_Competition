# Udacity's Deep RL Nanodegree - Project 3: Collaboration and Competition

[//]: # (Image References)

[image2]: https://user-images.githubusercontent.com/10624937/42386929-76f671f0-8106-11e8-9376-f17da2ae852e.png "Kernel"


## Introduction

This repository contains my solution for the third project of the Deep Reinforcement Learning Nanodegree from Udacity. In this exercise an RL-agent controls two rackets to bounce a ball over a net, somewhat like playing Ping-Pong. The environment was made with Unity's ML-Agents framework.

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365478-9c47b600-a3a8-11ea-92b0-a750598a70e7.gif" alt="Trained Agent"/>
</p>


## Environment Definition

#### Rewards

- If an agent hits the ball over the net, it receives a reward of  **+0.1**.
- If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of **-0.01**.
- No rewards are provided in a per-time-step basis.

#### State Space

The observation space consists of **`8`** variables corresponding to the position and velocity of the ball and racket, the environment returns 3 stacked observation spaces at each timestep, son the returned variable has dimension **`24`**.
The vector has the following variables:
<br>
**`[Racket Pos X, Racket Pos Y, Racket Vel X, Racket Vel Y, Ball Pos X, Ball Pos Y, Racket Vel X, Racket Vel y]`**

(For some reason it seems that the last two elements are a repeat of the third and fourth elements.)

#### Action Space

Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. it looks the following.

**`[Racket Movement, Racket Jump]`**

- **`Racket Movement`**: **Positive** values move the racket at a constant speed towards the net, **Negative** values move it away from the net.
- **`Racket Jump`**: Values larger than **0.5** trigger a Jump, Values lower or equal than **0.5** do nothing.

Every entry in the action vector should be a number between **`-1`** and **`+1`**.

#### Task Goal

The goal of each agent is to keep the ball in play for as many time steps as possible. In order to consider the environment solved, any of the agents must get an average score of +0.5 over 100 consecutive episodes. The score of a particular episode is the maximum score between the two agents.



## Getting Started

Most of these instructions were borrowed from the installation instructions in Udacity's [Deep Reinforcement Learning Repository](https://github.com/udacity/deep-reinforcement-learning).


### Prepare the Anaconda Environment


1. Create (and activate) a new environment with Python 3.6.

	- __Linux__ or __Mac__: 
	```bash
	conda create --name drlnd python=3.6
	source activate drlnd
	```
	- __Windows__: 
	```bash
	conda create --name drlnd python=3.6 
	activate drlnd
	```

2. Clone the repository (if you haven't already!), and navigate to the `python/` folder.  Then, install several dependencies.
```bash
git clone https://github.com/SaidAlvarado/https://github.com/SaidAlvarado/Udacity-DeepRL-Project_3_Collaboration_and_Competition.git
cd Udacity-DeepRL-Project_3_Collaboration_and_Competition/python
pip install .
```

3. Create an [IPython kernel](http://ipython.readthedocs.io/en/stable/install/kernel_install.html) for the `drlnd` environment.  
```bash
python -m ipykernel install --user --name drlnd --display-name "drlnd"
```

4. Before running code in the notebook, change the kernel to match the `drlnd` environment by using the drop-down `Kernel` menu. 

![Kernel][image2]

### Download the Unity Environment


5. Download the environment from one of the links below.  You need only select the environment that matches your operating system:

    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)

6. Decompress the file into the root directory of the repository.

### Run the code.

7. Follow the instructions in `DDPG_Collaboration_and_Competition.ipynb` to train and see the agent in action.


## Understanding the Algorithm

For more information regarding the algorithm used to solve this environment, please refer the the technical report `Report.md` included in the repository.


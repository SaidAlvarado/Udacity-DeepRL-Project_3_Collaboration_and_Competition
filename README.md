# Udacity's Deep RL Nanodegree - Project 2: Continuous Control

[//]: # (Image References)

[image2]: https://user-images.githubusercontent.com/10624937/42386929-76f671f0-8106-11e8-9376-f17da2ae852e.png "Kernel"


## Introduction

This repository contains my solution for the second project of the Deep Reinforcement Learning Nanodegree from Udacity. In this exercise an RL-agent is tasked with controlling a two-jointed arm as it moves to track the position of a target circling around it. The environment was made with Unity's ML-Agents framework and there are two variations one environment has 20 agents operating individually for parallel learning, and the other one only has a single agent.

<p align="center">
  <img src="https://user-images.githubusercontent.com/11748427/83365478-9c47b600-a3a8-11ea-92b0-a750598a70e7.gif" alt="Trained Agent"/>
</p>


## Environment Definition

#### Rewards

A reward of **`+0.1`** is provided for each step that the agent's hand is in the goal location.

#### State Space

The state space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. 

#### Action Space

Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between **`-1`** and **`1`**.

#### Task Goal

Such as the goal of the agent is to maintain its position at the target locations for as many time steps as possible. In order to consider the environment solved, the agent must get an average score of +30 over 100 consecutive episodes. In the case of the environment with 20 agents, the score of an episode is calculated as the average of the scores of all agents.



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
git clone https://github.com/SaidAlvarado/Udacity-DeepRL-Project_2_Continuous_Control.git
cd Udacity-DeepRL-Project_2_Continuous_Control/python
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

    - **_Version 1: One (1) Agent_**
        - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Linux.zip)
        - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher.app.zip)
        - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Windows_x86.zip)
        - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Windows_x86_64.zip)

    - **_Version 2: Twenty (20) Agents_**
        - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux.zip)
        - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher.app.zip)
        - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86.zip)
        - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Windows_x86_64.zip)

6. Decompress the file into the root directory of the repository.

### Run the code.

7. Follow the instructions in `DDPG_Continuous_Control.ipynb` to train and see the agent in action.


## Understanding the Algorithm

For more information regarding the algorithm used to solve this environment, please refer the the technical report `Report.md` included in the repository.


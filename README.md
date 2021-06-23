# Reinforcement learning in preventive maintenance policy
Our final project contains two ways to solve this problem, one is reinforcement learning and the other one is robust optimization. In order to introduce clearly, we will split our tutorial into two parts.
## Table of content
- Introducing markov decision process
- Q-learning in reinforcement learning
- What is preventive maintenance?
    - preventive maintenance
    - corrective maintenance
- Problem definition
- Markov decision process in the problem
- Programming example
## Introducing markov decision process
When we discuss the reinforcement learning, it will mention to the markov decision process and we can say that markov decision process is the base of reinforcement learning. So in this tutorial, we will start from MDP's introduction.

Before we start, let's take a look about the classical picture about the reinforcement learning. The picture depict the structure of reinforcement leaning, we can find that there are two component which represent the Agent and Environment, and the information iteracted is state, action, and reward respectively.
![](https://i.imgur.com/z3nbrmL.png)


### What is the purpose of the MDP?
Markov decision process problems assume a finite number of states and actions. At each time the agent observes a tate and executes an action, which incurs intermediate costs to be minimized. The cost and the successor state depend only on the current state and the chosen action. Successor generation may be probabilistic, based on the uncertainty we have on the environment in which the search takes place. 

For example, if we expect tommorrow will be a rainy day, so we decide to bring the umbrella, but in fact that tomorrow maybe just a sunny day. So, MDP formulate such problem into a mathematical model which can express as below.

$p(s', r| s, a) = Pr(S_{t+1}=s', R_{t+1}=r|S_t=s, A_t=a)$

By the way, in our problem we assume that the next state only have relation with the state now but not the states before and we call it markov property.

$























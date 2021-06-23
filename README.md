# Reinforcement learning in preventive maintenance policy
Our final project contains two ways to solve this problem, one is reinforcement learning and the other one is robust optimization. To introduce clearly, we will split our tutorial into two parts.
## Table of content
- Introducing Markov decision process
- Q-learning
- What is preventive maintenance?
    - preventive maintenance
    - corrective maintenance
- Problem definition
- Markov decision process in the problem
- Programming example
## Introducing Markov decision process
When we discuss reinforcement learning, it will mention the Markov decision process and we can say that the Markov decision process is the base of reinforcement learning. So in this tutorial, we will start with MDP's introduction.

Before we start, let's take a look at the classical picture of the reinforcement learning. The picture depicts the structure of reinforcement learning, we can find that there is two-component which represent the Agent and Environment, and the information interacted is state, action, and reward respectively.

![](https://i.imgur.com/f4cN3wp.png)


![](https://i.imgur.com/z3nbrmL.png)



### What is the purpose of the MDP?
Markov decision process problems assume a finite number of states and actions. At each time the agent observes a state and executes an action, which incurs intermediate costs to be minimized. The cost and the successor state depend only on the current state and the chosen action. Successor generation may be probabilistic, based on the uncertainty we have on the environment in which the search takes place. 

For example, if we expect tomorrow will be a rainy day, so we decide to bring an umbrella, but that tomorrow may be just a sunny day. So, MDP formulates such a problem into a mathematical model which can express as below.

![](https://i.imgur.com/XMEqAB8.png)

By the way, in our problem, we assume that the next state only has relation with the state now but not the states before. we call it Markov property.

![](https://i.imgur.com/Gjy9m28.png)

While we can formulate the stochastics problem we then can use the algorithms to solve the problem which will make the total reward maximum.

The algorithms for the MDP problem shows below:
- Value iteration
- Policy iteration
- Q-learning

In this tutorial, we will mainly introduce Q-learning.

## Q-learning
Q-learning is a model-free reinforcement learning algorithm to learn the value of an action in a particular state. Intuitively, Q-learning establishes the Q-table to record each state-action value which means if we do such action at such a state, how much reward will we gain. And we could use the following formula to learn the Q-table.


![](https://i.imgur.com/OXv8eMh.png)



Below is the algorithm about Q-learning:

![](https://i.imgur.com/JPUiW31.png)

Here we will introduce the algorithm step by step:

1. Step 1: initialize the Q-Table:
We will first build a n * m table, n means the number of states while m means the number of actions. And we can give each cell an arbitrarily value or zeros.
2. Step 2 and Step 3: Choose and perform an action
These two steps will do iteratively until it came to convergence. We choose an action in the state based on the Q-table which can contain the biggest Q-value. However, the beginning value is given arbitrarily (or zero), so we do not know which action may bring more value. To handle this problem, we can use the methods call the epsilon greedy strategy. These methods have a property that will exploration randomly in the beginning, it make the agent can try different actions when the prophase.
3. Step 4: Evaluate the action
Whenever we take action at a state, we can observe the environment and check the reward and update the Q-value by the formula below.
4. Step 5: Convergence
In this step, we may set some threshold to make the model terminate. For example, we often use the episode limit or the difference between the reward as the threshold.

## What is preventive maintenance?
Preventive maintenance (PM) is doing the maintenance before the machine breaks down, and making the machine  operating on schedule. On the other hand,  there is corrective maintenance (CM) which is doing the repair after the machine breaks down.  In the factory, we do not want to have corrective maintenance, because it will make the schedule out of control.

![](https://i.imgur.com/02q7Ffs.png)

Before introducing our methods, we will talk about the traditional PM policy, one is by time, and the other is by number. The policy by time is taking PM at a regular time, it is just like weekly PM and monthly PM. While the policy by numbers is taking PM when the number of production meets a certain level, maybe 1000 units or 10000 units. However, those policies may have a problem of lacking flexibility or stochastic factor-like order frequency and machine status.
![](https://i.imgur.com/9oFsiR8.png)

So, why do we need a preventive maintenance policy? Here is the profit and cost of PM. The profit of preventive maintenance has four points. First, PM can improve a machine's reliability and yield rate. At the same time, it can reduce the risk of unscheduled downtime and the cost of corrective maintenance. On the other hand, the cost of preventive maintenance is that it will generate production loss and backorder cost because when doing the PM, it has to turn off the machine and at this time no product will be produced. And at the same time, it will have some cost of preventive maintenance.


![](https://i.imgur.com/u6dhQpG.png)


















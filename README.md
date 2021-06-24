# Reinforcement learning in preventive maintenance policy
Our final project contains two ways to solve this problem, one is reinforcement learning and the other one is robust optimization. To introduce clearly, we will split our tutorial into two parts.

The other one part can see at below link:
Robust obtimization in preventive maintenance policy
## Table of content
- Introducing Markov decision process
- Q-learning
- What is preventive maintenance?
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

1. **Step 1:** initialize the Q-Table:
We will first build a n * m table, n means the number of states while m means the number of actions. And we can give each cell an arbitrarily value or zeros.
2. **Step 2 and Step 3:** Choose and perform an action
These two steps will do iteratively until it came to convergence. We choose an action in the state based on the Q-table which can contain the biggest Q-value. However, the beginning value is given arbitrarily (or zero), so we do not know which action may bring more value. To handle this problem, we can use the methods call the epsilon greedy strategy. These methods have a property that will exploration randomly in the beginning, it make the agent can try different actions when the prophase.
3. **Step 4:** Evaluate the action
Whenever we take action at a state, we can observe the environment and check the reward and update the Q-value by the formula below.
4. **Step 5:** Convergence
In this step, we may set some threshold to make the model terminate. For example, we often use the episode limit or the difference between the reward as the threshold.

## What is preventive maintenance?
Preventive maintenance (PM) is doing the maintenance before the machine breaks down, and making the machine  operating on schedule. On the other hand,  there is corrective maintenance (CM) which is doing the repair after the machine breaks down.  In the factory, we do not want to have corrective maintenance, because it will make the schedule out of control.

![](https://i.imgur.com/02q7Ffs.png)

Before introducing our methods, we will talk about the traditional PM policy, one is by time, and the other is by number. The policy by time is taking PM at a regular time, it is just like weekly PM and monthly PM. While the policy by numbers is taking PM when the number of production meets a certain level, maybe 1000 units or 10000 units. However, those policies may have a problem of lacking flexibility or stochastic factor-like order frequency and machine status.
![](https://i.imgur.com/9oFsiR8.png)

So, why do we need a preventive maintenance policy? Here is the profit and cost of PM. The profit of preventive maintenance has four points. First, PM can improve a machine's reliability and yield rate. At the same time, it can reduce the risk of unscheduled downtime and the cost of corrective maintenance. On the other hand, the cost of preventive maintenance is that it will generate production loss and backorder cost because when doing the PM, it has to turn off the machine and at this time no product will be produced. And at the same time, it will have some cost of preventive maintenance.


![](https://i.imgur.com/u6dhQpG.png)

## Problem definition
- Objective：Minimize sum of backorder cost and inventory cost
- Single machine preventive maintenance problem
- Order number will arrive at each step
- Discrete the machine status to numeral stage
- Machine have different wear out rate and yield rate at each stage
- Producing probaly makes machine wear out to next stage
- Rest will keep the stage at the same
- PM can recover the stage
- CM only can execute when at final stage and recover to the initial stage
- PM duration is shorter than CM duration
- Stochastic Factor
    - wear out rate (Exponential distribution)
    - demand fluctuation (Normal distribution)
![](https://i.imgur.com/zdCmnQy.png)

# Markov decision process in the problem
In the Markov decsion process, we have to define the state, action, and reward

## State
- machine age stage
    - machine wear out rate
    - machine yield rate
- Inventory level
- Backorder level
- Maintenance remaining duration
## Action
- Produce
- Rest
- Preventive maintenance
- Corrective maintenance
- Keep maintenance
## Reward
- Inventory cost
- Backorder cost

## Simple Transistion
Following picture is simple transistion, it seems like not showing the stochastic in the transistion. However, when at each stage, it has probaly transfer to different state, for example, if we keep producint, the state will effect by the order number which follow the normal distribution and may be wear out which follow by the exponetial distrubtion.

![](https://i.imgur.com/cLrkhm4.png)

# Programming example
### Give inital parameter
``` python
# machine 狀態
STAGE = 6
MACHINE_DETERIORATION_RATE = []
MACHINE_YIELD_RATE = [1.0, 0.8, 0.6, 0.4, 0.2, 0]
MACHINE_PRODUCTION_NUM = 10
# 成本相關
INVENTORY_LEVEL = 80
BUFFER_COST = -3
BACKORDER_COST = -5
# PM 相關
PM_DURATION = 1
CM_DURATION = 3
DURATION = 3

MU = 5
SIG= 1

lambd = 0.2
for i in range(1, 6):
    j = 1 - np.exp(-lambd*i)
    MACHINE_DETERIORATION_RATE.append(j)
MACHINE_DETERIORATION_RATE.append(0)
```
### Establish Q-table
Initial the Q-value, here we give a negative value for each cell because our rewards are all negative.
``` python
state, action_1, action_2, action_3, action_4, action_5 = [], [], [], [], [], []

for i in range(STAGE):
    for j in range((INVENTORY_LEVEL//2) * -1, (INVENTORY_LEVEL//2)+1):
            for k in range(DURATION+1):
                state.append((i,j,k))
                action_1.append(float("-1000"))
                action_2.append(float("-1000"))
                action_3.append(float("-1000"))
                action_4.append(float("-1000"))
                action_5.append(float("-1000"))
# 0: produce, 1:pm, 2:cm, 3:maintenance                    
q_table = pd.DataFrame({0:action_1, 1:action_2, 2:action_3,3:action_4, 4:action_5}, index=pd.MultiIndex.from_tuples(state))
initial_state = (0, 1, -4)
#q_table.loc[initial_state]
q_table
```
### Get environment
Giving the state and action, return the reward and next states.
Update the Q-table

```python
def get_enviroment(state, action):
    stage = state[0]
    inventory_level = state[1]
    duration = state[2]

    print("current_stage: ", stage)
    print("current_inventory_level: ", inventory_level)
    print("current_duration: ", duration)

    random_rate = random.random()
    order_num = int(random.gauss(mu=MU,sigma=SIG))
    print("order_num",order_num)

    if action == 0:
        print("keep producing")
        new_stage = stage + 1 if MACHINE_DETERIORATION_RATE[stage]>random_rate else stage
        new_inventory_level = inventory_level + int(MACHINE_YIELD_RATE[new_stage] * MACHINE_PRODUCTION_NUM) - order_num
        new_duration = 0
    elif action == 1:
        print("preventive maintenance")
        new_stage = stage - 2 if stage > 2 else 0
        new_inventory_level = inventory_level - order_num
        new_duration = PM_DURATION
    elif action == 2:
        print("corrective maintenance")
        new_stage = 0
        new_inventory_level = inventory_level - order_num
        new_duration = CM_DURATION
    elif action==3:
        print("under maintenance")
        new_stage = stage
        new_inventory_level = inventory_level - order_num
        new_duration = duration - 1
    else:
        print("rest")
        new_stage = stage
        new_inventory_level = inventory_level - order_num
        new_duration = 0


    # reward 並且檢查 new_inventory_level是否出界
    old_reward = inventory_level * BUFFER_COST if inventory_level >=0 else abs(inventory_level) * BACKORDER_COST
    if new_inventory_level >= INVENTORY_LEVEL/2:
        new_inventory_level = INVENTORY_LEVEL/2
    elif new_inventory_level <= -(INVENTORY_LEVEL/2):
        new_inventory_level = -(INVENTORY_LEVEL/2)

    reward = new_inventory_level * BUFFER_COST if new_inventory_level >=0 else abs(new_inventory_level) * BACKORDER_COST

    print("--after action--")
    print("next_stage: ", new_stage)
    print("next_inventory_level: ", new_inventory_level)
    print("next_duration: ", new_duration)
```
### Use Q-Table to choose action
```python
def choose_action(state, q_table, epsilon):
    stage = state[0]
    inventory_level = state[1]
    duration = state[2]
    
    random_prob = np.random.random_sample()
    if duration > 0: # 假設仍然在保養中，直接回傳 maintenance
        return 3
    elif stage == 5: # 假設 stage 已經到 5 了，直接選擇 CM 的動作
        return 2
    elif stage < 1: # 若 stage 為全新，則不會進行 PM，只會製造或停工
        if random_prob < epsilon:
            print("explore")
            rt = np.random.random_sample()
            if rt < 1/2:
                return 0
            else:
                return 4
        else:
            produce_value = q_table.loc[state, 0]
            rest_value = q_table.loc[state, 4]
            if produce_value >= rest_value:
                return 0
            else:
                return 4
    else: # 當 stage > 1 的時候，就能進行 PM、製造、停工
        if random_prob < epsilon:
            print("explore")
            rt = np.random.random_sample()
            if rt < 1/3:
                return 0
            elif 2/3 >= rt >= 1/3:
                return 1
            else:
                return 4
        else:
            produce_value = q_table.loc[state, 0]
            pm_value = q_table.loc[state, 1]
            rest_value = q_table.loc[state, 4]
            
            if produce_value >= pm_value and produce_value >= rest_value:
                return 0
            elif pm_value > produce_value and pm_value > rest_value:
                return 1
            else:
                return 4
            
```

### Result
There are 5 result plot for the solution
- Convergence plot: Sum of Q-table show the model convergence
![](https://i.imgur.com/42ylM1R.png)

- Average reward plot
According to the plot, we can find that the average reward increased from -100 to -20 which means it saves money on inventory cost and backorder cost.

![](https://i.imgur.com/drM9krI.png)

- Average inventory plot
According to the plot, we can find that the reinforcement learning's policy can reduce the inventory to a low level which closes to zero.

![](https://i.imgur.com/bNlkFxq.png)

- Inventory plot
The inventory plot shows that the inventory level and backorder level were controlled between a suitable interval by the policy.

![](https://i.imgur.com/rAqURsG.png)

- PM policy plot
Finally, we can see the PM's policy. According to the plot, we can see different colors: blue means produce, yellow means rest, and red means PM. So we can see that at different machine age stages and inventory levels, what action will the model do.
    - When staging 0 and  having enough inventory, RL will make machine rest
    - PM will be taken when stage 1-3 and having enough inventory
    - When stage 5, RL prefer to continue producing the product
    
![](https://i.imgur.com/mynWAbW.png)


### Comparison
However, we can see the scenario when the order number increases. the PM policy will change.
- Because of the loading increase, the policy generates by RL makes the machine keep producing most of the time
- Only when stage 3 and having lots of backorders, PM just be taken into consideration

![](https://i.imgur.com/lerRqze.png)


## Conclusion
In preventive maintenance, we consider the stochastic factor and try to solve the problem using the reinforcement learning method. 

To use reinforcement learning, we define the state, action, and reward to formulate the Markov decision process and then using Q-learning to solve the problem.

From the result, we can find that there have some interesting results. However, we think whenever making the PM decision, it still needs the human's experience and domain knowledge because there may have more factor should be considered just like WIP, loading rate.






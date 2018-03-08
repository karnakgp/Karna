### Title

Hindsight Experience Replay

### tl;dr

Standard reinforcement learning algorithms fail when rewards are sparse and binary (indicating whether or not the task has been completed) and so rewards need to be designed to guide policy optimization. The paper describes a technique that allows any off-policy RL algorithm to learn from sparse and binary rewards.

### Describe the method

The paper considers a actor-critic policy gradient method ([Continous Control with Deep Reinforcement Learning](https://arxiv.org/pdf/1509.02971.pdf)) in a setup where the start position and goal position are sampled at the start of each episode. The actor network uses current state and current goal to choose an action and the critic network uses current state, current goal and action to predict the Q-value.

Consider a task where a robotic arm has to grab a box and place it at a goal position. The state space consists of positions and velocities of robotic arm and the object. The goal states are states in which the object is at goal position with some fixed tolerance. The rewards are binary i.e. - [current_state == goal_state].

Standard RL algorithms will fail in this environment since the rewards are sparse and the agent wouldn't experience any reward other than -1. We may consider using a shaped reward function to guide the agent towards the goal e.g, reward = - (distance of gripper from object + distance of object from goal). It is difficult to design a reward function for any task and infact the policy gradient method can't successfully solve this task using this reward function. We also can't design a reward function when we don't know how the optimal behaviour looks like. In this example, the shaped reward captured our notion of progress towards completing the task but if we are using RL to learn the optimal way of play Go we may not understand when our agent is making progress.

Consider an episode with states s1, s2, ..., sT and a goal g != s1, s2, ..., sT which implies that the agent received a reward of −1 at every timestep. This sequence of states may not help us learn how to reach state g but it definitely tells us something about how to achieve the state sT. The idea is to add this episode to the replay buffer with goal state as g as well as sT. The replay with sT as goal state will contain reward different that -1 and make learning simpler. This approach combined with powerful function approximators (e.g, deep neural networks) allows the agent to learn how to achieve the goal even it has never observed it and only received -1 reward during training.

![algorithm](https://i.imgur.com/Vuu7Fin.png)

The authors did experiments on 3 robotic manipulation tasks and concluded that - 
1. The Policy gradient algorithm couldn't solve any the tasks but policy gradient with hindsight experience replay (HER) solves all tasks almost perfectly.

2. Even if we only care about one goal, training with multiple goals using HER performs better.

3. HER performs better than reward shaping because - 


      * Shaped rewards penalize for inappropriate behaviour (e.g. moving the box in a wrong direction) which may hinder exploration. It can cause the agent to learn not to touch the box at all if it can not manipulate it precisely and we noticed such behaviour in some of our experiments.
       
      * There is a huge discrepancy between what we optimize (i.e. a shaped reward function) and the success condition (i.e.: is the object within some radius from the goal at the end of the episode)

### Any further details

Example of reward shaping for robotic manipulation task - [Data-efficient Deep Reinforcement Learning
for Dexterous Manipulation](https://arxiv.org/pdf/1704.03073.pdf)

Effect of reward shaping on learned policy - [Policy invariance under reward transformations: Theory and application to reward shaping](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/NgHaradaRussell-shaping-ICML1999.pdf)

### My two cents

I really liked the idea that HER can be considered as a form of curriculum learning. The agent first learns how to go the the states it reached using the initial random policy and then further improves using experiences with harder goals and sparse rewards.
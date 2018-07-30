Intuitive RL: Intro to Advantage-Actor-Critic (A2C)

Reinforcement learning (RL) practitioners have produced a number of excellent tutorials. Most, however, describe RL in terms of mathematical equations and abstract diagrams. We like to think of the field from a different perspective. RL itself is inspired by how animals learn, so why not translate the underlying RL machinery back into the natural phenomena they’re designed to mimic? Humans learn best through stories.

This is a story about the Actor Advantage Critic (A2C) model. Actor-Critic models are a popular form of Policy Gradient model, which is itself a vanilla RL algorithm. If you understand the A2C, you understand deep RL.

##1
Intuitive RL: Intro to Advantage Actor Critic(A2C)
Written by Ruby Gilman
Art by Katherine Wang

##2
Agent: Cranberry Fox
In reinforcement learning, an agent moves through states in an environment by taking actions, trying to maximize rewards along the way

State
Action
Reward

##3
A2Cs take in a state - sensory inputs in Cranberry's case - and generate two outputs:

1) An estimate of how many rewards they expect to get from that point onwards, the state value.

2) A recommnedation of wath action to take, the policy

The "Critic"
Wow, What a wonderful glen! This will be a fruitful day of foraging. I bet I'll get 20 reward points before sunset today.

The "Actor"
Those flowers look pretty, I'm feeling drawn towards "A"...

##4

State
Policy

A deep RL model is an input-output mapping machine just like any other NN classification or regression model instead of mapping pictures or text to categories, a deep RL model mapes states to actions and/or states to state values. An A2C does both!

##5

##6
+1 score!

##7
This set of state-action-reward makes up a single observation. She'll record this row of data in her journal but she's not going to reflect on it just yet. 

State, V(s), Error, Action, Reward

She'll fill these out when she stops to reflect.

Some authors associate reward 1 with timestep 1, others associate it with timestep 2. All are referring to the same concept: A reward is associated with the state and action directly preceding it.

##8
Cranberry repeats the process again. First she takes in her surroundings and generates v(s) and an action recommendation.

This glen looks pretty standard.
v(s) = 19

Action choices all look pretty similar. I'll just go with "C" I guess.

##9
Then she takes an action

##10
Receives a reward +20!!

##11
And records the observation

##12
She repeats the process again

##13

##14

##15
State, V(s), V(s), error, action, reward

##16
After collecting three observations, Cranberry stops to reflect. 

Other families of model wait until the very end of the day before reflecting(Monte Carlo) while other reflect after every step (one-steps).

Before she can tune her inner critic, Cranberry needs to calculate how many points she would actually go on to receive from each given state.

But first!

##17
Let's consider how Cranberry's cousin, Monte Carlo Fox, would calculate the true value of each state.

Monte Carlo models don't reflect on their experiences until the end of the game. Since the value of the last state is zero, finding the true value of a given previous state is as simple as adding up the rewards received after that point.

No more rewards from here on out. V(s) = 0

From here i went on to receive six rewards, so the true value of the state is six. V(s) = 6

From here I went on to receive two rewards, so the true value of this state is two. V(s) = 2


##18
This is actually just a high-variance sample of V(s). The agent could easily have followed a different trajectory from the same state, thus receiving a different cumulative reward.

##19
But Cranberry is going to stop and reflect many times before the day is over. She won't know how many points she'll actually go on to recieve from each state until the end of the game because the end of the game is hours away!

This clearing looks decent, I'd say I'll gather 18 more fox points before the day is done.

This is where she does something really clever - Cranberry Fox estimates how many points she'll go on to get from the last state in the set. Luckily, she has a state-estimator built right in - her critic!

With this estimate in hand, Cranberry can calculate the "true" values of the preceding states exactly as Monte Carlo Fox did. 

Monte Carlo Fox estimated the target "labels" by Doing a rollout of a trajectory and adding up the rewards from each sate onwards. A2Cs truncate this trajectory and replace it with and estimate from their critic. Tis boostrapping down the variance of the estimate and allows A2Cs to operate continuosly, though at the cost of introducting a bit of bias.

##20
Rewards are often discounted to reflect the fact that a reward now is better than a rewardin the future. To keep thinks simple, Cranberry isn'y discounting her rewards.

##21
ss



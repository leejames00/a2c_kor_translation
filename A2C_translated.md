Intuitive RL: Intro to Advantage-Actor-Critic (A2C)

Reinforcement learning (RL) practitioners have produced a number of excellent tutorials. Most, however, describe RL in terms of mathematical equations and abstract diagrams. We like to think of the field from a different perspective. RL itself is inspired by how animals learn, so why not translate the underlying RL machinery back into the natural phenomena they’re designed to mimic? Humans learn best through stories.

This is a story about the Actor Advantage Critic (A2C) model. Actor-Critic models are a popular form of Policy Gradient model, which is itself a vanilla RL algorithm. If you understand the A2C, you understand deep RL.

## 1

Intuitive RL: Intro to Advantage Actor Critic(A2C)
Written by Ruby Gilman
Art by Katherine Wang

## 2

Agent: Cranberry Fox

In reinforcement learning, an agent moves through states in an environment by taking actions, trying to maximize rewards along the way

State
Action
Reward

## 3

A2Cs take in a state - sensory inputs in Cranberry's case - and generate two outputs:

1) An estimate of how many rewards they expect to get from that point onwards, the state value.

2) A recommnedation of wath action to take, the policy

The "Critic"

Wow, What a wonderful glen! This will be a fruitful day of foraging. I bet I'll get 20 reward points before sunset today.

The "Actor"

Those flowers look pretty, I'm feeling drawn towards "A"...

## 4

State
Policy

A deep RL model is an input-output mapping machine just like any other NN classification or regression model instead of mapping pictures or text to categories, a deep RL model mapes states to actions and/or states to state values. An A2C does both!

## 5

## 6

+1 score!

## 7

This set of state-action-reward makes up a single observation. She'll record this row of data in her journal but she's not going to reflect on it just yet. 

State, V(s), Error, Action, Reward

She'll fill these out when she stops to reflect.

Some authors associate reward 1 with timestep 1, others associate it with timestep 2. All are referring to the same concept: A reward is associated with the state and action directly preceding it.

## 8

Cranberry repeats the process again. First she takes in her surroundings and generates v(s) and an action recommendation.

This glen looks pretty standard.

v(s) = 19

Action choices all look pretty similar. I'll just go with "C" I guess.

## 9

Then she takes an action

## 10

Receives a reward +20!!

## 11

And records the observation

## 12

She repeats the process again

## 13

## 14

## 15

State, V(s), V(s), error, action, reward

## 16

After collecting three observations, Cranberry stops to reflect. 

Other families of model wait until the very end of the day before reflecting(Monte Carlo) while other reflect after every step (one-steps).

Before she can tune her inner critic, Cranberry needs to calculate how many points she would actually go on to receive from each given state.

But first!

## 17

Let's consider how Cranberry's cousin, Monte Carlo Fox, would calculate the true value of each state.

Monte Carlo models don't reflect on their experiences until the end of the game. Since the value of the last state is zero, finding the true value of a given previous state is as simple as adding up the rewards received after that point.

No more rewards from here on out. V(s) = 0

From here i went on to receive six rewards, so the true value of the state is six. V(s) = 6

From here I went on to receive two rewards, so the true value of this state is two. V(s) = 2


## 18

This is actually just a high-variance sample of V(s). The agent could easily have followed a different trajectory from the same state, thus receiving a different cumulative reward.

## 19

But Cranberry is going to stop and reflect many times before the day is over. She won't know how many points she'll actually go on to recieve from each state until the end of the game because the end of the game is hours away!

This clearing looks decent, I'd say I'll gather 18 more fox points before the day is done.

This is where she does something really clever - Cranberry Fox estimates how many points she'll go on to get from the last state in the set. Luckily, she has a state-estimator built right in - her critic!

With this estimate in hand, Cranberry can calculate the "true" values of the preceding states exactly as Monte Carlo Fox did. 

Monte Carlo Fox estimated the target "labels" by Doing a rollout of a trajectory and adding up the rewards from each sate onwards. A2Cs truncate this trajectory and replace it with and estimate from their critic. Tis boostrapping down the variance of the estimate and allows A2Cs to operate continuosly, though at the cost of introducting a bit of bias.

## 20

Rewards are often discounted to reflect the fact that a reward now is better than a rewardin the future. To keep thinks simple, Cranberry isn'y discounting her rewards.

## 21

Now Cranberry can go through each row of data and compare her state-value predictions to their actual values. She uses the difference between these numbers to hpone her prediction skills. Even three steps into the day, Cranberry has gatherered valuable experiences worth reflectiing on. 

I was way off for state 1 and 2! What did I miss?

Aha! Next time I see feathers like that, I know to increase V(s).

It may seem crazy that Cranberry is able to use her estimate of V(s) as the ground truth to compare her other predictions against. But animals (us included) do this all the time! If you feel like things are going well, no need to wait to reinforce the actions that led you there. 

## 22

By truncating our calculated returns and replacing them with a bootstrapped estimate, we've traded some of the variance of the Monte Carlo model for a bit of bias. RL models generally suffer from high-variance (imagine all the possible trajectories!), so this is usually a worthwile exchange. 

## 23

Cranberry repeats the same process all day, gathering three state-action-reward obervations and reflecting on them. 

## 24

Each set of three observations is a tiny, autocorrelated minibatch of labelled training data. To reduce thie autocorellation, many A2Cs run multiple agents in pararell, stacking their experiences together before pushing them into a shared neural network. 

## 25

The day is almost over. Only two steps to go. 

As we saw earlier, Cranberry's action recommendations are expressed as percentage confidences about her options. Instead of simply taking the highest-confidence choice, Cranberry samples from this action distribution. This ensures she doesn't always settle for safe but potentially mediocre actions. 

A:40% B:40% C:20%

I might regret it, but...

sometimes exploring unknown things can lead to exciting discoveries...

## 26

Low entropy discourage

High entropy encourage

To further encourage exploration, a value called entropy is subtracted from the loss function. Entropy refers to the "spread" of the action distribution. 

## 27

Looks like the gamble paid off!

## 28

Or did it?

## 29

Sometimes an agent will find itself in a state where all actions lead to negative outcomes. A2Cs, however, are excellent at making the most of bad situations.

## 30

## 31

## 32

## 33

As the sun sets, Cranberry reflects on this last set of decisions. 

We talked about how Cranberry tunes her inner critic. But how does she adjust her inner actor? How does she learn to make such refined choices?

## 34

A simple Policy Gradient Fox should look at the actual returns following an action and tune her policy to make good return more likely.

Looks like my policy from that state led to losing 20 points, I guess I better make "c" less likely in the future...

But wait!

It's not fair to place the blame on action C. That state had an estimated valued of -100, so taking "c" and ending up with -20 was actually a relative improvement of 80! I should make "c" more likely in the future.

Instead of tuning her policy in respose to the total return she got by taking action C, she tunes her actions to the relative returns of taking action c. This is called the "advantage".

## 35

What we called the advantage is just the error. As the advantage, Cranberry uses it to make actions that were surprisingly good more likely. As the error, she uses the same quantity to nudge her inner critic to make better estimations of state values. 

Actor uses advantage. 

Wow, that worked out better than I though, action C must have been a good idea. 

Critic uses error

But why was i surprised in the first place? I probably shouldn't have estimated that state as negatively as i did.

Now we can show how total loss is computed - this is the function we minimize to improve our model. 

Total loss = action loss + value loss - entropy.

Notice we're shoving gradients of three qualitatively differnet types back through a single NN. This is efficient but it can make convergence more difficult. 

## 36

Like all animals, as cranberry matures she'll hone her ability to predict state values, gain increasing confidence in her action choices and find herself surprised less often by rewards.

## 37

Not only do RL agents like Cranberry generate all the data they need, they also estimate the target labels themselves. That's right, RL models update previous estimates to better match new and improved estimates. 

As Dr. David Silver, head of the RL group at Google Deepmind, puts it: AI = DL + RL. When an agent like Cranberry can bootstrap her own intelligence, The possibilities are limitless...

The END



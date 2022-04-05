# ML-for-Supply-Chain


SUMMARY
This project aims to optimize an inventory system by finding an optimal (s, S) policy. The basic workflow of the project involves optimizing the inventory system by formulating it as a Markov decision process (MDP), where the inventory is controlled based on weekly review. A Python code was developed to comprehensively study, analyze and visualize MDP through graphical plots. To solve this MDP two methods have been used in this process.

First, MDP is formulated using a user-defined function called “MarkovDecisionProcess”. According to several cost parameters given in the problem statement, transition and reward are initialized for the “MarkovDecisionProcess” object.

The policy iteration algorithm is used as the first optimization method for the MDP. Users can use this technique to learn the optimal policy of the inventory system to maximize long-term reward. The user-defined function “BestPolicy(gamma)” with one input “gamma” value was used to run this algorithm. After optimizing the policy (s, S), the reward graph was plotted comparing the maximum value of the optimal policy as gamma changes.

For Q-learning method, the optimal policy is determined by learning optimal q values for each state action pair. First an inventory environment was created to return current stock and the order quantity. Q-table was setup along with the number of steps and finally a reward graph was plotted to show how reward values change as the number of iterations increased to find optimal value for the best policy. Optimal policy graph was also shown to demonstrate the change in stock as the number of iterations increased.

The code is segmented into two sections where “Method 1” section includes all necessary user-defined functions to run the policy iteration algorithm and “Method 2” section contains all the user-defined functions as well as other codes to get the optimal policy graphs and results.


MARKOV DECISION PROCESS (MDP)
Markov Decision Process is a mathematical framework for dynamic optimisation problems. It makes decision considering all the information available for an evolving situation in the environment. Here the information about the surrounding is given gradually, for example movement of a robot, inventor etc.

A basic MDP had 5 basic components as shown in the formulation, that is the states, actions, transition probabilities, reward functions and gamma.

For the inventory Problem, the states can be defined with the number of inventory units.

Policy: It is a collection of optimal solutions from the perspective of static optimization. The objective is to find the policy leading to the best result. A policy is denoted as a function ‘pi’ (symbol) mapping from where one is, to where one should go. A policy must cover all possible situations, and therefore, a policy should a mapping from all existing states to optimal action.

Policy.PNG

Optimization Equations: Value function can be understood as intuition is our decision making through multiple experiences. This Intuition can be captured in the form of a function called value function. Value function is defined as expectation of long-term return from an action and state. The optimality equation can be generated from the recursive equation which is the value function. This equation can be expresses as the Intuition when things are done optimally and is used in the policy improvement (equation shown along with policy improvement). link text

We can solve the recursive equation using many methods including

backward induction
value iteration
iteration
linear programming.
We have used the policy iteration method for project 2.


MDP FORMULATION
We know that,

Market price = 10 dollar

Wholesale price, c = 4 dollars

Unit holding cost per year = 2 dollars/year = 52/2 dollars per week

Fixed setup cost, k = 100 dollars

Stockout penalty, p = 15 dollars

Maximum capacity of storage, M = 500

Lead time = 2 weeks

Furthermore, we know that weekly demand follows a discrete uniform distribution between 0 and 10. Therefore, by solving for F(x),

F(x) = (x-a)/(b-a) = (x-0)/(10-0) = 0.1x

We also know that weekly demand lies between 0 and 10, therefore,

(Demand – 0)/(10 – 0) = F(x)

(Demand – 0)/(10 – 0) = 0.1x

Demand/10 = 0.1x

Demand = 1x

Max demand is 10, therefore demand = 10 for 1 week

p = 0.1

Discount factor ranges from 0 to 1

Formulation of MDP:

The 5 essential elements needed to represent the Markov Decision Process are given as (S, A, P, R, γ)

S - Finite set of states which is inventory level

S = {0, 1, ..., M}

A - Finite set of actions which is the number of units decided to buy

For a state s, a ∈ A(s) = {0, 1, ..., M − s}.

As we can not order more than the capacity of the store.

P – State transition probability matrix

P(s’|s,a) = P[St+1|St = s,At = a]

P(1,1) is the probability of the system state to transition from 1 to 1

R – Reward function

Depending on the transition, either a reward or a penalty will be provided.

R(s,a) formula.PNG

γ – Discount factor

Considers that the dollar today is worth more than a dollar tomorrow


METHOD 1: POLICY ITERATION
Policy Iteration for the inventory problem:
The problem is defined as shown in formulation and we first calculate the revenues based on probably distribution of demand. This revenue is used in the reward calculation. Later transition probably is calculated based on s+a and s2, that is, when inventory is stocked to s+a, what is the probably that it becomes s2.

Policy Evaluation:

Given a policy we are computing the long-term consequence. We initialize a random policy and to compute the long-term consequences, we use the Bellman Equation as shown below,

Bellman Eqn.PNG

Using this formula, we find the V(s=0) to V(s=500)

Policy Improvement:

Using the the V values from previous step we compute pi(s)

Policy improvement.PNG

pi(s) takes the max of all the values generated using a. The a with the max value will be the new pi, this way, we can change the original policy to a better policy by evaluated by solving system if linear equations.
By taking the difference between the policy pi and the policy improvement pi'. This will be looped until no better policy can be found (in our case, iterations are limited to 10,000).


METHOD 2 : Q-LEARNING
Q-learning is a reinforcement learning technique used for learning the optimal policy in a Markov decision process.

The goal of Q learning is to find the optimal policy by learning the optimal Q values for each state action pair. Once we have our optimal Q function( Q) we can determine the optimal policy by applying a reinforcement learning algorithm to find the action that maximizes Q for each state. The Q function for a given policy accepts a state and an action and returns the expected return from taking the given action in the given state . Making use of a table called a Q table to store the Q values for each state action pair the horizontal axis of the table represents the actions and the vertical axis represents the states so the dimensions of the table are the number of actions by the number of states. Q(state(S), action(A)) returns the expected future reward of that action at that state.This function can be estimated using Q-learning, which iteratively updates Q(s,a) using the Bellman equation.

image.png

In short, q learning can be represent as shown below

image.png

Step 1: Initialise q values estimates with zeroe values, and pick an initial state

Step 2: Agent picks an action to execute from the current state using epsilon greedy policy

Step 3: Agent obtains the observation data from the environment

Step 4: Update the current Q-value from the observed reward and the target Q-value.The target Q-value is action with the max Q-value from the next state using Bellamn equations above

it runs for 20000 iteration of the above 4 steps, with learning rate of 0.6 and discount rate 0.94


MAJOR FUNCTIONS IN THE CODE - POLICY ITERATION
class MarkovDecisionProcess() : This class is designed to formulate the Markov Decision process problem and is defined by the given states, actions, transition probabilities and reward function.

def_init(self): The _init(self) method is a reversed method of the MarkovDecision Process. Strictly speaking in terms of Object Oriented programming, the __init(self) method basically a constructed of the above class which initializes the various states, actions, Transition probabilities and reward functions.

def R() : This method returns the reward function, given a particular state and action.

def T() : This method returns the Transition probabilities for a given state, given action and resultant state.

def BestPolicy() : This method performs policy evaluation as well as policy improvement simultaneously. It basically iterates through all possible policies compares them to the previous policy and selects the best policy as the given policy, if no improvement can be done.


MAJOR FUNCTIONS IN THE CODE - QLEARNING
class inventory() : this class creates the inventory environment that encompasses the three function that are explained below

def__init__(self) : This function initiates the environment with the variables(stock, sale price, stock price, first week order quantity, second week order quantity)

def start() : This function returns the current stock and how much is ordered. This is called at start of training because it returns the current state that the policy needs to pick the best action

def step() : This function takes an action, and returns the resulting state, and reward


LESSONS LEARNT
Learning Rate – The leaning rate (α) in Q-learning can be set between 0 and 1. If the Q-values are never updated, it indicates a learning rate of 0. A learning rate of 0 means nothing is learned. For learning to occur quickly, a high learning rate such as 0.9 is required.

Discount rate (gamma) – The discount rate aka gamma (γ) can also be set between 0 and 1 like the learning rate. This factor decides whether the reinforcement learning (RL) agent gives importance to future rewards or immediate rewards. A discount rate γ=0 indicates if RL agent will act myopic while only learning about different actions that provides immediate rewards. On the other hand, a discount rate γ=1 will learn about different actions by summing total future rewards.

Greedy Policy – Greedy policy guarantees convergence despite of using function approximation to estimate different action values. Epsilon-Greedy balances between exploration and exploitation by randomly choosing between exploration and exploitation. This is an exploration strategy in RL, where ε (epsilon) refers to an exploratory action with probability exploiting mostly with a slim chance of exploring and 1-ε refers to a greedy action with probability.

Model for Q learning is easier than MDP. Unlike MDP, Q Learning does not need probability distribution. MDP, however depends on transition probabilities and is supposed to be more accurate


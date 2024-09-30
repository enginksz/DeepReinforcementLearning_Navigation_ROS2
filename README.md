# Deep Reinforcement Learning for Mobile Robot Navigation

This project implements **Deep Reinforcement Learning (DRL)** for mobile robot navigation using the **Twin Delayed Deep Deterministic Policy Gradient (TD3)** algorithm. The goal is to enable an autonomous robot to navigate to random goal points in a simulated environment while avoiding obstacles.

## Table of Contents
- [Introduction](#introduction)
- [Deep Reinforcement Learning](#deep-reinforcement-learning)
- [Key Concepts](#key-concepts)
  - [Policy Gradient](#policy-gradient)
  - [Deterministic Policy Gradient](#deterministic-policy-gradient)
  - [Deep Deterministic Policy Gradient](#deep-deterministic-policy-gradient)
  - [Twin Delayed Deep Deterministic Policy Gradient](#twin-delayed-deep-deterministic-policy-gradient)
- [Network Architecture](#network-architecture)
- [Installation and Usage](#installation-and-usage)
- [Acknowledgment](#acknowledgment)

## Introduction

Deep Reinforcement Learning combines reinforcement learning and deep learning to enable agents to learn optimal policies in complex environments. This project focuses on utilizing DRL for robot navigation in the ROS Gazebo simulator, specifically implementing the TD3 algorithm to handle continuous action spaces effectively.

---

## Deep Reinforcement Learning

Deep Reinforcement Learning is an advanced form of machine learning where an agent learns to make decisions by interacting with an environment. The agent receives feedback in the form of rewards or penalties and aims to maximize its cumulative reward over time.

### Key Concepts

#### Policy Gradient

**Policy Gradient** methods directly optimize the policy function, which defines the agent's behavior. The objective is to maximize the expected cumulative reward, denoted as:

\[
J(\theta) = \mathbb{E} \left[ \sum_{t=0}^{T} r_t \right]
\]

where \( r_t \) is the reward at time \( t \) and \( \theta \) are the policy parameters. The gradient of the objective function can be estimated using the policy gradient theorem:

\[
\nabla J(\theta) \approx \mathbb{E} \left[ \nabla \log \pi_\theta(a_t | s_t) Q_w(s_t, a_t) \right]
\]

where \( \pi_\theta \) is the policy and \( Q_w \) is the action-value function.

#### Deterministic Policy Gradient

**Deterministic Policy Gradient (DPG)** is a specific type of policy gradient method where the policy is deterministic, meaning it outputs a specific action for each state:

\[
a_t = \mu_\theta(s_t)
\]

The DPG algorithm updates the policy using the gradient of the expected reward with respect to the policy parameters:

\[
\nabla J(\theta) = \mathbb{E} \left[ \nabla_a Q(s_t, a) \big|_{a=\mu_\theta(s)} \nabla_\theta \mu_\theta(s_t) \right]
\]

This approach is suitable for continuous action spaces.

#### Deep Deterministic Policy Gradient

**Deep Deterministic Policy Gradient (DDPG)** extends DPG by leveraging deep neural networks to approximate both the policy (actor) and the value function (critic):

- **Actor Network**: Outputs the action given a state.
- **Critic Network**: Evaluates the action-value function \( Q(s, a) \).

The critic network is trained using the Bellman equation, and the actor is updated using the deterministic policy gradient as described above.

#### Twin Delayed Deep Deterministic Policy Gradient

**Twin Delayed Deep Deterministic Policy Gradient (TD3)** addresses some of the limitations of DDPG, specifically overestimation bias and stability issues. TD3 introduces the following enhancements:

1. **Clipped Double Q-Learning**: Two critic networks are maintained, and the minimum Q-value is used to reduce overestimation bias.
   
   \[
   Q(s, a) = \min(Q_1(s, a), Q_2(s, a))
   \]

2. **Delayed Policy Updates**: The actor network is updated less frequently than the critic networks, allowing for more accurate Q-value estimates.

3. **Target Policy Smoothing**: Adds noise to the target actions to promote robustness during training.

---

## Network Architecture

The following diagram illustrates the TD3 network architecture implemented in this project:

![Network Architecture](./drl_nav.png)

### Explanation of the Architecture

1. **State Input**: The robot's environment state, such as sensor readings, is input into the network.
2. **Actor Network**:
   - Composed of multiple fully connected layers that output the continuous actions (translational and angular velocities).
   - The architecture includes 800, 600, and 2 neurons in the final layer to output actions.
3. **Critic Networks**:
   - Two critic networks (Q1 and Q2) evaluate the actions taken by the actor.
   - These networks have several layers to estimate the action-value function, ensuring accurate evaluation by selecting the minimum of the two Q-values.
4. **TD3 Enhancements**:
   - The dual critic networks and delayed updates contribute to stability and prevent overfitting.

This architecture enables robust learning for mobile robot navigation in complex environments.

---


## Installation and Usage

### Build and Source the Project

1. **Navigate to the project directory and build the project**:
   ```bash
   cd DeepReinforcmentLearning_Navigation_ROS2
   colcon build
   ```

2. **Source the workspace**:
   ```bash
   source install/setup.bash
   ```

3. **Launch the training simulation**:
   ```bash
   ros2 launch td3 training_simulation.launch.py
   ```

4. **Launch the test simulation**:
   ```bash
   ros2 launch td3 test_simulation.launch.py
   ```

   

   

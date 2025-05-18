# What is OpenAI Gymnasium?

## 1. Background

OpenAI Gymnasium is a popular Python toolkit for developing and comparing reinforcement learning (RL) algorithms. It provides a wide variety of simulated environments where an AI agent can learn by trial and error.

> **Note:** Gymnasium is the modern, actively maintained fork of the original OpenAI Gym, adding improvements and better compatibility with recent Python and machine learning frameworks.


## 4. Taxi-v3 Environment

### What is Taxi-v3?

Taxi-v3 is a simple grid-world environment from OpenAI Gymnasium.  
It is used to teach and test reinforcement learning (RL) algorithms because of its easy-to-understand rules.

---

### What does it do?

- You control a taxi on a **5x5 grid** city map.  
- The goal is to **pick up a passenger** at one location and **drop them off** at another.  
- The taxi can move **up, down, left, right** and perform two special actions: **pick up** and **drop off**.  
- You get rewards or penalties based on your actions:  
  - Positive reward for successfully dropping off the passenger.  
  - Negative reward for illegal moves (like trying to pick up or drop off at the wrong place).  
  - Small negative reward for each move to encourage faster completion.

---

### How does Taxi-v3 work?

- **State / Observation**:  
  A single number that represents:  
  - The taxi’s position,  
  - The passenger’s location (four possible spots or inside the taxi),  
  - The passenger’s destination.

- **Actions**:  
  1. Move South (down)  
  2. Move North (up)  
  3. Move East (right)  
  4. Move West (left)  
  5. Pick up passenger  
  6. Drop off passenger  

- **Rewards**:  
  - +20 for successfully dropping off the passenger  
  - -10 for illegal pick-up or drop-off  
  - -1 for each time step (move) to encourage efficiency  

- **Episode ends** when the passenger is dropped off at the correct location.

- **Goal**:  
  Learn the best actions (policy) to maximize total rewards by efficiently picking up and dropping off the passenger.

---

### Why use Taxi-v3?

- It’s simple but not trivial, making it great for testing RL algorithms.  
- Combines navigation and decision-making tasks.  
- Provides a clear example of how RL agents can learn to act optimally.

```python
import gymnasium as gym
import pygame
import numpy as np

# Create environment with RGB render mode
env = gym.make("Taxi-v3", render_mode="rgb_array")
obs, info = env.reset()
total_reward = 0

# Initialize Pygame
pygame.init()

# Get original frame size
frame = env.render()
orig_height, orig_width, _ = frame.shape
scale_factor = 2  # You can try 2 or 3 based on how big you want
win_width = orig_width * scale_factor
win_height = orig_height * scale_factor

# Set smaller, fixed-size window
screen = pygame.display.set_mode((win_width, win_height))
pygame.display.set_caption("Taxi-v3 - Compact Pygame Interface")

clock = pygame.time.Clock()

running = True
for _ in range(1000):
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    if not running:
        break

    frame = env.render()
    frame = np.transpose(frame, (1, 0, 2))  # Convert (H, W, C) → (W, H, C)
    surface = pygame.surfarray.make_surface(frame)

    # Scale surface to window size
    surface = pygame.transform.scale(surface, (win_width, win_height))

    screen.fill((0, 0, 0))  # Optional: background color
    screen.blit(surface, (0, 0))
    pygame.display.flip()

    action = env.action_space.sample()
    obs, reward, terminated, truncated, info = env.step(action)
    total_reward += reward

    if terminated or truncated:
        obs, info = env.reset()

    clock.tick(5)  # Control FPS

env.close()
pygame.quit()
print(f"Total Reward: {total_reward}")
```

## Manual Control for CarRacing-v Environment with Domain Randomization

### What is CarRacing-v?

CarRacing-v is a continuous control environment in OpenAI Gym where you drive a car around a procedurally generated track.  
The goal is to learn to drive the car efficiently and complete laps without crashing.

---

### What does this code do?

- Creates the `CarRacing-v` environment with **domain randomization** enabled, meaning the track and visuals change each episode to improve agent robustness.
- Uses **Pygame** to allow manual control of the car via keyboard inputs.
- Controls:  
  - **Left/Right arrows** steer the car left or right (steer value from -1 to 1).  
  - **Up arrow** accelerates (gas).  
  - **Down arrow** applies the brake.

---

### How does it work?

- The environment is reset with domain randomization activated (`randomize=True`).
- Pygame captures keyboard inputs every frame to convert them into continuous actions for steering, gas, and brake.
- The environment runs a simulation step with the chosen action.
- Total reward is accumulated and printed when the race finishes.
- If the race ends (episode termination), the environment resets and the process repeats.
- The simulation runs at 60 frames per second.

---

### Why use this setup?

- Provides an interactive way to understand how the CarRacing environment works.
- Allows testing human driving strategies on a randomized environment.
- Can be used as a baseline for comparing reinforcement learning agents or training data collection.

---

### Controls Summary

| Key        | Action            |
|------------|-------------------|
| Left Arrow | Steer left        |
| Right Arrow| Steer right       |
| Up Arrow   | Accelerate (Gas)  |
| Down Arrow | Brake             |

---

### Notes

- Domain randomization helps build more generalized RL agents by exposing them to different track layouts.
- The action space is continuous with three dimensions: `[steer, gas, brake]`.
  
```python
import gym
import pygame
from pygame.locals import *

# Create CarRacing environment with domain randomization
env = gym.make("CarRacing-v", render_mode="human", domain_randomize=True)
env.reset(options={"randomize": True})  # You can toggle this

pygame.init()
pygame.display.set_caption("Manual Car Racing")
clock = pygame.time.Clock()

# Action format for CarRacing: [steer (-1 to 1), gas (0 to 1), brake (0 to 1)]
def get_action_from_keys():
    keys = pygame.key.get_pressed()
    steer = 0
    gas = 0
    brake = 0

    if keys[K_LEFT]:
        steer = -1
    elif keys[K_RIGHT]:
        steer = 1
    if keys[K_UP]:
        gas = 1
    if keys[K_DOWN]:
        brake = 0.8

    return [steer, gas, brake]

done = False
observation, _ = env.reset()
total_reward = 0

while not done:
    for event in pygame.event.get():
        if event.type == QUIT:
            done = True

    action = get_action_from_keys()
    observation, reward, terminated, truncated, info = env.step(action)
    total_reward += reward

    if terminated or truncated:
        print(f"Race Over! Total Reward: {total_reward:.2f}")
        observation, _ = env.reset()
        total_reward = 0

    clock.tick(60)

env.close()
pygame.quit()
```

# Acrobot-v1 Environment (OpenAI Gymnasium)

## 🧠 What is Acrobot-v1?

**Acrobot-v1** is a classic control problem provided by the OpenAI Gymnasium library. It simulates a **two-link pendulum** with one actuator at the joint. The challenge is to **swing the lower link’s tip high enough** to reach a goal height above the base.

---

## 🎯 Goal

To swing the Acrobot such that the **tip of the lower link** reaches a height equal to the length of one link **above the base** (i.e., tip_y ≥ 1.0).

---

## 🕹️ Actions

The environment uses a **discrete action space** with 3 possible actions:
- `0` → Apply **-1 torque**
- `1` → Apply **0 torque** (no movement)
- `2` → Apply **+1 torque**

---

## 🔢 Observations

Each state/observation is a **6-dimensional vector** representing:
- Cosine of first joint angle
- Sine of first joint angle
- Cosine of second joint angle
- Sine of second joint angle
- Angular velocity of first joint
- Angular velocity of second joint

---

## 💰 Rewards

- **-1** per timestep until the goal is reached
- No positive reward; the goal is to **minimize the number of steps**

---

## 🧪 Episode Termination

The episode ends when:
- The tip of the lower link reaches a vertical position ≥ 1.0 (`terminated=True`)
- OR the time limit is reached (`truncated=True`)

---

## 📜 Code Overview

```python
import gymnasium as gym

env = gym.make("Acrobot-v1", render_mode="human")
obs, _ = env.reset()

for _ in range(500):
    action = env.action_space.sample()
    obs, reward, terminated, truncated, _ = env.step(action)
    if terminated or truncated:
        obs, _ = env.reset()

env.close()
```

# 🚗 CarRacing-v3 Environment (OpenAI Gymnasium)

## 🧠 What is CarRacing-v3?

**CarRacing-v3** is a continuous control environment from OpenAI Gymnasium where the agent learns to drive a car on procedurally generated tracks. It’s widely used to test reinforcement learning algorithms in dynamic, high-dimensional visual environments.

---

## 🎯 Goal

To **drive the car along the track as efficiently and accurately as possible**, maximizing total reward and avoiding going off-road.

---

## 🕹️ Action Space

The environment uses a **continuous action space** with 3 values:

[steering, gas, brake]


- **Steering**: Range from `-1` (left) to `+1` (right)
- **Gas**: Range from `0` (no gas) to `1` (full throttle)
- **Brake**: Range from `0` (no brake) to `1` (full brake)

---

## 🔢 Observations

Each observation is a **96x96 RGB image** (visual input from the car’s front view). The agent learns from pixel data, similar to how a human learns by seeing.

---

## 💰 Rewards

- Positive rewards for staying on track.
- Negative penalties for driving off the track or idling.
- Reward is continuous and accumulates throughout the episode.

---

## 🧪 Episode Termination

The episode ends when:
- The car drives off the track for too long.
- The car idles or makes no progress.
- The maximum timestep limit is reached.

---

## 📜 Code Overview

```python
import gymnasium as gym

env = gym.make("CarRacing-v3", render_mode="human")
obs, _ = env.reset()

for _ in range(1000):
    action = env.action_space.sample()
    obs, reward, terminated, truncated, _ = env.step(action)
    if terminated or truncated:
        obs, _ = env.reset()

env.close()

```

# 🦿 BipedalWalker-v3 Environment (OpenAI Gymnasium)

## 🧠 What is BipedalWalker-v3?

**BipedalWalker-v3** is a physics-based reinforcement learning environment in OpenAI Gymnasium where a two-legged agent (walker) must learn to walk across a randomly generated terrain.

This environment is widely used to train and evaluate agents in **continuous control tasks**, simulating real-world challenges like walking, balance, and locomotion.

---

## 🎯 Goal

To **learn how to walk and navigate rough terrain** without falling. The agent must control its legs using torque to move forward.

---

## 🕹️ Action Space

- **Type**: Continuous `Box(-1, 1, (4,))`
- The action is a 4-dimensional vector controlling torque applied to the 4 motor joints:

[hip_1, knee_1, hip_2, knee_2]


---

## 🔢 Observation Space

- A **24-dimensional** vector that includes:
- LIDAR measurements of terrain
- Velocity and position of the body
- Joint angles and angular velocities

---

## 💰 Rewards

- **+300** for walking successfully across the terrain.
- Penalties for falling or not making progress.
- **Shaped rewards** encourage forward motion, balance, and control.

---

## 🧪 Episode Termination

The episode ends when:
- The walker **falls** or tips over.
- The agent reaches the **end of the terrain**.
- The maximum step limit (1600 steps) is reached.

---

## 📜 Sample Code

```python
import gymnasium as gym

env = gym.make("BipedalWalker-v3", render_mode="human")
obs, _ = env.reset()

for _ in range(1000):
  action = env.action_space.sample()  # Random actions
  obs, reward, terminated, truncated, _ = env.step(action)

  if terminated or truncated:
      obs, _ = env.reset()

env.close()
```

# 🚀 LunarLander-v2 Environment (OpenAI Gymnasium)

## 🌌 What is LunarLander-v2?

**LunarLander-v2** is a classic reinforcement learning environment where the goal is to land a spacecraft safely on the moon’s surface. It simulates a 2D lunar lander module using basic physics, making it perfect for learning control policies.

---

## 🎯 Goal

Control the lander to **safely touch down** on the landing pad between the flags with minimal fuel usage.

---

## 🕹️ Action Space

- **Type**: Discrete (`Discrete(4)`)
- **Actions**:
  - `0`: Do nothing
  - `1`: Fire left engine
  - `2`: Fire main (bottom) engine
  - `3`: Fire right engine

---

## 🔢 Observation Space

- **Type**: Continuous `Box(8,)`
- Includes:
  - X & Y position
  - X & Y velocities
  - Landers angle
  - Angular velocity
  - Left and right leg contact indicators (binary)

---

## 💰 Rewards

- **+100 to +140** for a successful landing.
- **-100** or more for crashing.
- Small **negative reward** for using engines (fuel penalty).
- Bonus for keeping the lander upright.

---

## 🧪 Episode Termination

The episode ends if:
- The lander crashes or flies off-screen.
- The lander comes to rest.
- It reaches the maximum number of steps (1000 by default).

---

## 📜 Sample Code

```python
import gymnasium as gym

env = gym.make("LunarLander-v2", render_mode="human")
obs, _ = env.reset()

for _ in range(1000):
    action = env.action_space.sample()  # Random action
    obs, reward, terminated, truncated, _ = env.step(action)

    if terminated or truncated:
        obs, _ = env.reset()

env.close()
```

# 🏔️ MountainCar-v0 Environment (OpenAI Gymnasium)

## 📘 What is MountainCar-v0?

`MountainCar-v0` is a classic control problem in reinforcement learning. The task is to help a car stuck in a valley **reach the top of a mountain** on the right. However, the car’s engine is not strong enough to climb directly, so it must build momentum by swinging back and forth.

---

## 🎯 Goal

Accelerate the car so it gains enough momentum to **reach the goal** (the flag at the top-right).

---

## 🕹️ Action Space

- **Type**: `Discrete(3)`
- **Actions**:
  - `0`: Push left
  - `1`: Do nothing
  - `2`: Push right

---

## 🔢 Observation Space

- **Type**: `Box(2,)`
- **Observations**:
  - `position`: Current position of the car (range: `-1.2` to `0.6`)
  - `velocity`: Current velocity of the car (range: `-0.07` to `0.07`)

---

## 💰 Rewards

- **-1** per timestep until the goal is reached.
- No reward for reaching the goal; the goal is **efficiency** (fewer steps).

---

## 🧪 Episode Termination

The episode ends when:
- The car’s position reaches **0.5** or more (i.e., it reaches the flag).
- Or after **200 steps**, whichever comes first.

---

## 🧪 Sample Code

```python
import gymnasium as gym

env = gym.make("MountainCar-v0", render_mode="human")
obs, _ = env.reset()

for _ in range(200):
    action = env.action_space.sample()  # Random action
    obs, reward, terminated, truncated, _ = env.step(action)

    if terminated or truncated:
        obs, _ = env.reset()

env.close()
```


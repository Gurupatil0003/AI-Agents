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
```
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

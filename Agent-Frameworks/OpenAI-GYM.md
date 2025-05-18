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

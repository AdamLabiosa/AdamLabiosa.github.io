---
title: "Custom Brax Environments"
date: 2026-04-15
summary: "How to create non-mujoco environments for RL training with brax."
tags:
  - reinforcement-learning
  - brax
  - environments
---

## Context

I think we all recognize brax environments are useful for very fast RL training. How do we create custom envs which are useful?

## Setup

Have this at the top:
```python

import jax
import jax.numpy as jnp
from brax.envs.base import Env, State
```

And is setup with `class EnvName(Env):`

## Init function

Anything which is assigned to self stays the same throughout training.


### Obs and action space
In the init, you must define the action space, observation space and backend for jax

e.g.
```python
 @property
def observation_size(self) -> int:
    return 4

@property
def action_size(self) -> int:
    return 4

@property
def backend(self) -> str:
    return "abstract"
```

For hybrid observations, we need to do something special. First, pass the state out of step flattened. Then, reshape it back to the shape we want. For example of a encoder:

```python
import flax.linen as nn
import jax.numpy as jnp

class HybridEncoder(nn.Module):
    @nn.compact
    def __call__(self, x: jnp.ndarray):
        # Reshape back to image
        image_part = x[..., :3072].reshape((-1, 32, 32, 3))
        symbolic_part = x[..., 3072:]

        # Pass through cnn
        x_img = nn.Conv(features=32, kernel_size=(3, 3))(image_part)
        x_img = nn.relu(x_img)
        x_img = nn.max_pool(x_img, window_shape=(2, 2), strides=(2, 2))
        x_img = x_img.reshape((x_img.shape[0], -1))  # Flatten
        
        # Concat and pass to mlp
        combined = jnp.concatenate([x_img, symbolic_part], axis=-1)
        x_mlp = nn.Dense(features=256)(combined)
        return nn.relu(x_mlp)
```

Then we have to do something like this (TODO: UPDATE):
```python
def make_my_networks(
    observation_size, 
    action_size, 
    preprocess_observations_fn):
    
    return ppo_networks.make_ppo_networks(
        observation_size=observation_size,
        action_size=action_size,
        preprocess_observations_fn=preprocess_observations_fn,
        # Brax will append policy/value heads to this encoder
        policy_network_factory=lambda: HybridEncoder() 
    )

# Pass the factory to ppo.train
train_fn = functools.partial(
    ppo.train,
    num_timesteps=100_000,
    network_factory=make_my_networks, # Use the custom factory here
    # ... other hyperparams
)
```


## Reset function

Reset returns a state object. Here is an example:

```python
def reset(self, rng: jnp.ndarray) -> State:
    initial_obs = jnp.zeros(self.observation_size) 

    return State(
        pipeline_state=None,  # Set to none for custom envs
        obs=initial_obs,
        reward=jnp.array(0.0),
        done=jnp.array(0.0),
        metrics={'episode_reward': 0.0}, # Accumulates over training automatically. Use for wandb. Only for floats
        info={} # For things like task id or single step metrics
    )
```

## Step function

Step returns a state object:

```python
def step(self, state: State, action: jnp.ndarray) -> State:
    # Calculate new state
    new_obs = state.obs + action
    
    # Calcuate reward 
    reward = -jnp.sum(jnp.square(new_obs))

    # Done is 1 for done, 0 for not
    done = jnp.where(jnp.abs(new_obs) > 10.0, 1.0, 0.0)
    
    # Return new state by doing state.replace
    return state.replace(
        obs=new_obs,
        reward=reward,
        done=done
    )
```


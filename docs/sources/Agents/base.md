<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/basicAgents.py#L9)</span>
## AgentBase

```python
connectFourLab.game.agents.basicAgents.AgentBase()
```

Base class Agent

To inherit from this class it's required to implement
the `action` method

```python
def action(self, board):
    # choose a column based in a given board
    return chosen_column # 0-6
```

__Attributes__

- `name` -  str, required, name of the agent
- `description` -  str, required, description of the agent
- `kind` -  str, required, category of the agent
- `model_key` -  str, required for agents who uses
    neural network models
- `clock_management` -  bool, optional, default `False` (*)
    - flag that indicates whether or not the agent 
        can manage a time limit game
- `require_nn_model` -  bool, optional, default `False` (*)
    - flag that indicates whether or not the agent
        needs a neural network model

(*) - only necessary in the `app` interface

__Saving the data__

You can easily save the data from all turns of the game
using `save` all data will be saved separately in:

- `data_scenario` -  state of the board
- `data_action` -  taken action
- `data_reward` -  attributed reward

```python
def action(self, board):
    choice = self.random_choice(board)
    self.save(board, choice)
    return choice
```
__Exception__

- `BadImplementation` -  some class didn't implemented the
    required method(s)

### Clock management
If the game is time limited the `update_clock` will be
called every turn and you will be able to manage the
`Timer` (see [Timer](../Game/timer)) in `self.clock`.

### Using Neural Networks
To use neural network you need a [Trainer](./trainers)
which have to create and train a model for the agent.

The trained models are stored in the `game/models/` folder.
__Example__

- `MCTSNN` -  Monte Carlo Tree Search with Neural Network implementation -
    see [documentation](./agents#MCTSNN)


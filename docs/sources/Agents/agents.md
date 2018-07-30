<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/basicAgents.py#L156)</span>
## AgentHuman

```python
connectFourLab.game.agents.basicAgents.AgentHuman()
```

Human Agent represents the user as a player

When a UI implements `RunGame` it will
assign a function to `get_input` which will be
called each turn. The function should return 
the column choosed by the user.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/basicAgents.py#L177)</span>
## AgentRandom

```python
connectFourLab.game.agents.basicAgents.AgentRandom()
```

Random agent, play a valid column chosen randonly

----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/negamax.py#L9)</span>
## AgentNegamax

```python
connectFourLab.game.agents.negamax.AgentNegamax()
```

Negamax Agent

This agent uses the negamax algorithm which is a
simplified version of the minimax algorithm.

Negamax is based on the observation that
`max(a,b) = -min(-a,-b)`

This agent uses negamax in all actions using the maximum
depth of `5`.

It also uses a __zobrist hash table__ to store all searches in order
to be more efficient.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/monteCarlo.py#L13)</span>
## AgentSimulation

```python
connectFourLab.game.agents.basicAgents.AgentSimulation()
```

Simulation agent

When taking an action this agent simulates a number of
games for each available column and then choose the 
best probabilistic option.

The best probabilistic option is the best score of a column,
in a 100 simulated games.

Each simulation consists in simulate every turn randomly for
both players until the simulation ends in a terminal state.
Then it will return `1` for victory, `-1` for defeat or `0`
in case of a draw.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/monteCarlo.py#L60)</span>
## AgentSimulationTL

```python
connectFourLab.game.agents.basicAgents.AgentSimulationTL()
```

Simulation strategy with time management

This agent act by simulating a number of
games for each available column and then choosing the 
best probabilistic option.

Similar with the `AgentSimulation`. Differentiating
itself by instead of simulating a fixed number of games,
this agent manage the time available through the game,
simulating the maximum amount of games (equaly distributed
to all possible columns) without letting the time run out
and thus being able to handle limited time games.

In unlimited time games the agents takes the maximum of
20 seconds per turn before returning the choice.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/monteCarlo.py#L129)</span>
## AgentMonteCarlo

```python
connectFourLab.game.agents.basicAgents.AgentMonteCarlo()
```

Monte Carlo agent.

This agent applies the Monte Carlo Tree Search method.
The method consists in search the possibilities of
the board evaluating each stage of the board (rollout), but
different from a minimax tree search this search
don't go into all the possibilities, it uses the
UCB1 algorithm to measure how much of the search effort
goes into exploiting promissing branches and exploring
less prommissing branches.

This agent evaluate the rollout with a simulation
of 100 games per board state.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/mctsnn.py#L9)</span>
## AgentMCTSNN

```python
connectFourLab.game.agents.mctsnn.AgentMCTSNN(model_file=None, model=None)
```

Agent Monte Carlo Tree Search with Neural Network evaluation.

This agent inherit from `AgentMonteCarlo` using the same
process but evaluating the rollouts by using a trained
neural network model.

This agent uses the Node `NodeMCTSNN` which evaluate the rollout
score by predicting the board state in a trained model.

__Arguments__

- `model_file` -  str; path to the '.h5' model file;
- `model` -  str, loaded `keras.models.Model` object;

__Exceptions__

- `MissingModel` -  raise when creating a new instance, if
    both model_file and model are None.

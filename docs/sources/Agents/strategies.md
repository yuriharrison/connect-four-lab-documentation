# Strategies

Strategies are the classes with common algorithms that can be shared among multiple
[agents](./agents). It makes easy to implement variances or combine diferent strategies when creating an new agent.

### Example
```python
# inheriting from two strategy classes
class AgentSimulationTL(AgentBase, SimulationStrategy, TimerStrategy):
    name = 'Simulation TL'
    description = 'Simple simulation strategy (simulates managing the time limit)'
    kind = 'simulation'
    clock_management = True

    # picking a single method from a strategy class
    childs = TreeSearchStrategy.childs

    def action(self, board):
        # applying the strategy method from TimerStrategy
        rule = lambda time_left: time_left/(25-self.turn)
        self.start_timer(rule, max=20)
        
        ...
```

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/random.py#L6)</span>
## RandomStrategy

```python
connectFourLab.game.agents.strategies.random.RandomStrategy()
```

Random strategy

---
## RandomStrategy methods

### random_choice


```python
random_choice(board)
```


Random choice. Return a valid column from a given
board state.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/timer.py#L7)</span>
## TimerStrategy

```python
connectFourLab.game.agents.strategies.timer.TimerStrategy()
```

Timer strategy allow you to easily program
a timer to control the time of each turn.

Using the `start_timer` method you can set a rule
which will determine the start point of the timer
and when the time expires the `time_out` flag will
be changed to `True`.

__Example__


```python
from . import AgentBase
from .strategies import TimerStrategy

class AgentNew(AgentBase, TimerStrategy):

    def action(self, board):
        rule = lambda time_left: time_left/(25-self.turn)
        self.start_timer(rule, max=20)

        while not self.time_out:
            # loop process

        return column
```


---
## TimerStrategy methods

### start_timer


```python
start_timer(rule, max)
```


Set the rule and starts the timer

__Arguments__

- `rule` -  lambda, required, rule which will determine
    the time limit of the turn.
- `max` -  float, required; maximum time allowed per turn.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/treeSearch.py#L10)</span>
## ZobristHashingStrategy

```python
connectFourLab.game.agents.strategies.treeSearch.ZobristHashingStrategy()
```

Zobrist hashing table strategy provid an
easy way to implement a hash table using the a board
state to crate the hash. Useful to store the data in
a tree search.

__Examples__

- `TreeSearchStrategy` -  [documentation](#treesearchstrategy)
- `Node` -  [documentation](#node)


---
## ZobristHashingStrategy methods

### init_zobrist


```python
init_zobrist()
```


Initialize Zobrist hashing table.

This method creates a list of two tables, filled
randomly with int64 numbers, to represent
all possitions in a 7x7 board, one table for each
player. It also generate two random int64 numbers
which will represent the owner of a board state.

Both lists are used when generating a hash in the
`hash` method.


---
### hash


```python
hash(board, color)
```


Create a __zobrist hash__ for a given board state.

__Arguments:__

- `board` -  matrix, required, board state
- `color` -  int (1 or -1), required, owner of 
    the board state.

__Return:__

Generated zobrist hash.


---
### next_random64


```python
next_random64()
```


Return a random int64 number

----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/treeSearch.py#L85)</span>
## TreeSearchStrategy

```python
connectFourLab.game.agents.strategies.treeSearch.TreeSearchStrategy()
```

Tree Search Strategy provide the necessary methods
to an agent make a tree search in a given board state.

__Example__

- `AgentNegamax` -  [documentation](./agents#agentnegamax)


---
## TreeSearchStrategy methods

### negamax


```python
negamax(node, depth, color)
```


__Negamax algorithm__

This algorithm, in order to be more efficiente, 
also uses a __Zobrist hashing table__ to store
the values of board states and avoid duplicated
searches in equal board states in the same depth.

__Arguments__

- `node` -  matrix, required, board state
- `depth` -  int, required, number which controls the
    depth limit that the recursive calls should hit
- `color` -  int (1 or -1), required, owner of the board state

__Return__

Best value of a `node`


---
### childs


```python
childs(node, color=1)
```


Create an return the children of a given `node`.

The children are generated from all available
positions in a given `board`

__Arguments__

- `node` -  matrix, required, board state
- `color` -  int (1 or -1), optional, default 1
    owner of the board state

__Return__

List of children. Each item = (column, new_board_state)


---
### save_search


```python
save_search(board, value, depth, color)
```


Save the data of a search in a __zobrist hashing table__.

__Arguments:__

- `board` -  matrix, required, board state
- `value` -  int, value of the board state
- `depth` -  int, current depth of the search
- `color` -  int (1 or -1), owner of the board state


---
### stored_value


```python
stored_value(board, depth, color)
```


Get the stored value of a board state.

Search in the hash table if the is already a value
stored for the `board` and return the value if there is
one and if it's necessary to update the value.

The search is made on a __Zobrist hashing table__.

It will require an update in the value stored when
the stored value depth is inferior to
the current search depth.

__Arguments__

- `board` -  matrix, required, board state
- `depth` -  int, required, current search depth
- `color` -  int (1 or -1), owner of the board state

__Return__

- `value` -  int, value of the given board if it finds one
- `update` -  boolean, `True` when the value stored needs to
    be updated.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/monteCarlo.py#L10)</span>
## SimulationStrategy

```python
connectFourLab.game.agents.strategies.monteCarlo.SimulationStrategy()
```

Simulation Stragegy provide the method necessary
to simulate matches.

---
## SimulationStrategy methods

### simulate


```python
simulate(board, color=-1)
```


Simulate a match to the end from a given
board state. All turns are played randomly till
the board hits a terminal state then the value is
returned.

__Arguments__

- `board` -  matrix, required, board state to be simulated
- `color` -  int, required, id of the owner of the board state

__Return__

Id of the winner of the simulation or zero in case
the simulation ends in a draw.

__Example__

- `AgentSimulation` -  [documentation](./agents#agentsimulation)


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/monteCarlo.py#L109)</span>
## Node

```python
connectFourLab.game.agents.strategies.monteCarlo.Node(board, memory=None, parent=None, position=None, color=1)
```

Node class is a base for a complex node for a Monte Carlo
Tree Searches.

This class has specific methods to perform the Monte Carlo Tree
Search.

This class __don't work on its own__, because it doesn't have a
default board evaluation algorithm. When inherited it's required
to implement the `rollout_score` method which is an evaluation
of a given state.

This class uses a __zobrist hashing table__ to optimize the search.

__Arguments__

- `board` -  matrix, required, board state
- `memory` -  empty dictionary, required(*), default None
    - this dictionary will store all searches
        with zobrist hashing.
- `parent` -  `Node` object, required (**), default None
    - Node above in the tree hierarchy.
- `position` -  int, required (**), default None
    - Index of the column which generated the board
        current `board`.
- `color` -  int, required (**), default 1

(*) required when creating a new root `Node` object.

(**) required when creating a new "children"
`Node` object (`new_node` method).

__Properties__

- `UCB1` -  float, UCB1 value (*) of the node.
- `visits` -  int, total number of Node visits
- `value` -  float, total number of score divided by the
    number of visits
    - the total number of score is the sum of all
        scores made by the Node and his children.

(*) UBC1 is an algorithm which calculates the distribution
of search effort for exploration and exploitation in
the Monte Carlo Tree Search strategy.

__Example__

- `AgentMonteCarlo` -  [documentation](./agents#agentsimulation)
- `AgentMCTSNN` -  [documentation](./agents#agentmctsnn)


---
## Node methods

### rollout


```python
rollout()
```


Node rollout.

It will execute a rollout if this is the first
visits of the Node, otherwise it will return `False`.

Each rollout adds a visit in the counter and the score
of the `board` to the Node and all the parents above
in the tree.

__Return__

`True` when the rollout occur, `False` when it do not.


---
### rollout_score


```python
rollout_score()
```


This method must return a score (float), evaluation, 
for the Node board state (`self.board`), from the 
perspective of the player id 1.

This method is called every rollout. The rollout occur
in the first visit of the Node, the value is stored in
the zobrist table and it is __not__ recalculated during the match.

__Return__

float, score of the Node `board`


---
### children


```python
children()
```


Get all the childen Nodes.

Generate or get from the memory, all the childen
Nodes. Each node is generated from the available
possitions in the `board` of the current Node.


---
### new_node


```python
new_node(board, column)
```


This method is called by `children` method to
generate a new Node.

__Arguments:__

- `board` -  matrix, new board state
- `column` -  int, index of the last column

__Return__

A new instance of `Node` object.


----

<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/agents/strategies/monteCarlo.py#L55)</span>
## DepthMeasure

```python
connectFourLab.game.agents.strategies.monteCarlo.DepthMeasure()
```

Use this class to help when measuring the depth of a
tree search. Useful when debugging and measuring performance
optimization.

__Example__


```
class AgentNew(AgentBase, TimerStrategy):
    
    def action(self, board):
        DepthMeasure.start()
        self.start_timer(...)

        while not self.time_out:
            # place the line "DepthMeasure.add()" inside the
            # search method, when it creates a new depth
            self.run_search()
            DepthMeasure.reset()

        DepthMeasure.print()

        return ...
```


---
## DepthMeasure methods

### start


```python
start()
```


Reset all the variables to begin a new measurement.

---
### add


```python
add()
```


Add more 1 depth.

---
### reset


```python
reset()
```


Reset the depth before start a new search episode. Save the
current depth if it's the deepiest till know.


---
### print


```python
print()
```


Print the deepiest depth reached.
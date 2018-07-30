<span style="float:right;">[[source code]](https://github.com/yuriharrison/connect-four-lab/blob/master/connectFourLab/game/game.py#L27)</span>
## RunGame

```python
connectFourLab.game.game.RunGame(player_one=None, player_two=None, first_player_randomized=True, time_limit=None, print_result_on_console=False, start=True, async=False)
```

Run matches of Connect Four (7x7)

This class uses [Agents](./Agents) to take actions
each turn and [Timer](./timer) to control the `time_limit`.

__Arguments__

- `player_one` -  type or instance, optional, default `None`
    - Type or instance of any `AgentBase` object
    - If `None` the `AgentRandom` object will be assigned 
- `player_two` -  same as `player_one`
- `first_player_randomized` -  bool, optional, default False
    - Defines if the first turn will be randomized or the player one
        will start the game
- `time_limit` -  int, optional, default None
    - Defines the limit of time each player have to play the match
    - `None` - Unlimited time
- `print_result_on_console` -  bool, optional, default False
    - Defines if in the end of the match the result will be printed
        on the console. Good for debug.
- `start` -  bool, optional, default True
    - If `True` the game will be started in the init or 
        else you have to call `start`
- `async` -  bool, optional, default False
    - If `True` the game will be run asynchronously

__Attributes__

- `GameStatus` -  Enum -> (running, winner, tie, timeout, killed, exception)
    - Example: RunGame.GameStatus.winner
- `BOARD_FORMAT` -  tuple, constant, value (7,7), define the board dimensions
- `MAX_TURNS_POSSIBLE` -  int, the maximum number of turns in a match
- `winner` -  `AgentBase` object
    - The Agent winner of the last match, if the is one.
- `status` -  GameStatus, check the example(2) bellow
    - `None` - New instance, not started yet 
    - `running` - Game is running
    - `winner` - Last game ended with a winner
    - `tie` - Last game ended in a tie
    - `timeout` - In the last game one of the players
        left the time run out
    - `killed` - The game was stoped with the `kill` method
    - `exception` - Exception during the game execution

__Properties__

- `is_running` -  Return `True` if the game is currently running

__Exeptions__

- `InvalidColumn` -  raised when the agent return a column out of range
    or a column already fulfilled

__Example 1__

```python
from connectFourLab.game import RunGame
RunGame(print_result_on_console=True)
```

__Example 2__

```python
import time
from connectFourLab.game import RunGame
from connectFourLab.game.agents.monteCarlo import AgentSimulation

game = RunGame(player_one=AgentSimulation, async=True)
while game.is_running:
    time.sleep(1)

if game.status == game.GameStatus.tie:
    print('The game ended in a tie.')
elif game.status == game.GameStatus.winner:
    print('Winner:', game.winner.name)
elif etc...
```

### UI implementation

To implement an User Interface you can override the events and
implement the `get_human_input`

__Events__

- `on_game_start` -  Triggered when the game starts
- `on_new_turn` -  Triggered when a new turn starts
- `on_end_turn` -  Triggered when the turn ends
- `on_game_end` -  Triggered when the game ends

__User input__

- `get_human_input` -  It's called by any AgentHuman in the match

__UI Attributes__

- `memory` -  list, register all turns data
    - Item: (player_id, current_board, column_choosed)
- `players` -  dict, two Agents one for each player
    - player one: id `1` - two: id `-1`
- `clock` -  dict, two clocks (`Chronometer` or `Timer`) one for each player
    - player one: id `1` - two: id `-1`

__UI Example__

See [Game Screen - Board class](../../app/screens/game#board-class)


---
## RunGame methods

### start


```python
start()
```


Starts a new match

---
### kill


```python
kill()
```


Stop the current match.

Force the game to stop and wait for the status confirmation.


---
### get_human_input


```python
get_human_input(player, board)
```


Event game start

To be overridden by an UI.

This method will be called by every `AgentHuman`
present in the match, if the is one. This method
should return the user input, the player
column of choice.

If this method is not overridden, the `AgentHuman`
won't work

__Arguments__

- `player` -  `AgentHuman`, owner of the turn
- `board` -  current state of the board

__Return__

User input, column of choice, as int 0-6


---
### on_game_start


```python
on_game_start()
```


Event game start

To be overridden by an UI. 
Called in the beginning of a match.


---
### on_game_end


```python
on_game_end()
```


Event game end

To be overridden by an UI. 
Called in the end of a match.

---
### on_new_turn


```python
on_new_turn(player, clock)
```


Event new turn

To be overridden by an UI.
Called in the beginning of each turn.

__Arguments__

- `player` -  `AgentBase` object, owner of the turn
- `clock` -  `Chronometer` or `Timer`,
    clock of the owner of the turn


---
### on_end_turn


```python
on_end_turn()
```


Event end turn

To be overridden by an UI. 
Called in the end of a turn.
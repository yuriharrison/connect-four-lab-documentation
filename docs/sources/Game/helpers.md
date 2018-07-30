### check_winner


```python
check_winner(game_board)
```


Check if there is a winner in a given board

Split the `game_board` for each player and check, 
using binary operations, if there is a required 
combination for a win.

__Arguments__

- `game_board` -  matrix (7x7), required

__Return__

- `1` -  id 1 is the winner
- `-1` -  id -1 is the winner
- `None` -  no winner in the giver table

__Example__


```
import numpy as np
board = np.zeros((7,7), dtype=int)
board[1,0] = 1
board[2,0] = 1
board[3,0] = 1
board[4,0] = 1
board[3,1] = -1
board[3,2] = -1
board[3,3] = -1

winner = check_winner(board)
if winner:
    print('Winner id:', winner)
```


----

### bit_board_split


```python
bit_board_split(board)
```


Split a given board in a bit board for each player

__Arguments__

- `board` -  matrix (7x7), required

__Return__

Return a bit board of 56 of length, one for each player, which represents
(in binary) the positions of the player on the board.


----

### next_position


```python
next_position(board, column)
```


Return the next availabe position in a column

__Arguments__

- `board` -  matrix, required
- `column` -  int, required - Index of the column

__Return__

Index (row) of availabe position


----

### available_positions


```python
available_positions(board)
```


Yield all empty positions of a given board

__Arguments__

- `board` -  matrix, required

__Yield__

Index of the column and row of all empty positions in the `board`

__Example__


```python
import numpy as np
board = np.zeros((7,7), dtype=int)
board[0,0] = 1
board[1,0] = -1
board[3,0] = 1
board[3,1] = -1
board[6,0] = -1

for x, y in available_positions(board):
    print('Column: {} - Row: {}'.format(x, y))
    print('Position:', board[x,y])
```


----

### seconds_to_hms


```python
seconds_to_hms(seconds)
```


Convert seconds in hour, minutes and seconds

__Example__


```python
h, m, s = seconds_to_hms(5401)
print('Hour: {} Minutes: {} Seconds: {}'.format(h, m, s))
```

# QUESTION 5

**Best Stock Trade** *(Top portion of problem statement cut off in screenshot)*

**Problem Statement:**
- *(Partial text visible)*: ...buying at 50 (day 3) and selling at 50 (day 4) earns 50 - 50 = 0...
- Buying at 36 (day 5) and selling at 55 (day 7) earns 55 - 36 = **19**, the best.

**Input Format:**
* N, after `Enter N:`
* The N prices in day order on one line, after `Enter the prices:`

**Output Format:**<br>
Two lines: `Best profit:` and the profit, then `Sell on day:` and the selling day. If she does not trade, print `Best profit: 0` and `Sell on day: NONE`.

**Constraints:**
* 2 <= N <= 50; each price is from 1 to 1000.
* Prices may repeat; buying and selling at the same price earns 0.
* The highest price may come before the lowest.

### Sample Outputs

```
Sample Output 1
Enter N: 7
Enter the prices: 45 38 50 42 36 47 55
Best profit: 19
Sell on day: 7

Sample Output 2
Enter N: 4
Enter the prices: 90 80 80 70
Best profit: 0
Sell on day: NONE

Sample Output 3
Enter N: 2
Enter the prices: 10 30
Best profit: 20
Sell on day: 2
```

## ANSWER 5

```python
N = int(input("Enter N: "))
prices = list(map(int, input("Enter the prices: ").split()))

min_price = prices[0]
best_profit = 0
sell_day = "NONE"

for day in range(1, N):
    profit = prices[day] - min_price
    if profit > best_profit:
        best_profit = profit
        sell_day = day + 1
    if prices[day] < min_price:
        min_price = prices[day]

print(f"Best profit: {best_profit}")
print(f"Sell on day: {sell_day}")
```

---

# QUESTION 6

**Sliding Tile Row**

**Problem Statement:**<br>
In a sliding tile game, a row of **N** cells holds number tiles, where `0` means empty. Pushing the row slides the tiles to one side, and two equal tiles side by side merge into one tile of double the value. A tile merges at most once per push.

Push the row **once** to the **left**:
* Non-zero tiles slide left without gaps, keeping their order.
* From the left, equal neighbours merge in pairs into a tile of double the value, which is added to the score. A merged tile cannot merge again.
* Tiles slide left again; the cells left on the right become `0`.

For example, take **N = 6** and the row `2 0 2 4 4 8`. Sliding gives `2 2 4 4 8`. Then:<br>
2 + 2 makes 4 (+4)<br>
4 + 4 makes 8 (+8)<br>
The new 8 cannot merge with the old 8, so the row becomes `4 8 8 0 0 0` and the score is **12**.

**Input Format:**
* **N**, after `Enter N:`
* The **N** tiles, after `Enter the tiles:`

**Output Format:**<br>
Two lines: `Final row:` and the **N** values separated by single spaces, then `Score:` and the score.

**Constraints:**
* 1 <= N <= 20. Each tile is from 0 to 1000 and need not be a power of two.
* All tiles may be equal. In a run of three or more equal tiles, pairs form from the left.
* There may be no merge; tiles still slide and the score is 0. Equal tiles not side by side after sliding do not merge.

### Sample Outputs

```
Sample Output 1
Enter N: 6
Enter the tiles: 2 0 2 4 4 8
Final row: 4 8 8 0 0 0
Score: 12

Sample Output 2
Enter N: 5
Enter the tiles: 2 2 2 0 0
Final row: 4 2 0 0 0
Score: 4

Sample Output 3
Enter N: 5
Enter the tiles: 0 5 0 7 5
Final row: 5 7 5 0 0
Score: 0
```

## ANSWER 6

```python
N = int(input("Enter N: "))
tiles = list(map(int, input("Enter the tiles: ").split()))

non_zero = [t for t in tiles if t != 0]
merged = []
score = 0
i = 0

while i < len(non_zero):
    if i + 1 < len(non_zero) and non_zero[i] == non_zero[i + 1]:
        val = non_zero[i] * 2
        merged.append(val)
        score += val
        i += 2
    else:
        merged.append(non_zero[i])
        i += 1

final_row = merged + [0] * (N - len(merged))

print(f"Final row: {' '.join(map(str, final_row))}")
print(f"Score: {score}")
```

---

# QUESTION 7

**Round-Robin Champion**

**Problem Statement:**
**N** teams played a round-robin tournament, so every pair of teams met exactly once. In the $\mathbf{N \times N}$ score table, `g[i][j]` (row `i`, column `j`) is the number of goals team `i` scored against team `j`, so their match ended `g[i][j]` to `g[j][i]`. Teams are numbered `1` to **N** in row order.
<br><br>
A win (more goals than the opponent) earns **3** points, a draw **1** point and a loss **0** points. The **champion** is the team with the most points; if teams tie on points, the higher goal difference wins, then the smaller team number. Print the champion's number, its points and its **goal difference**: the champion's total goals scored minus its total goals conceded.
<br><br>
For example, with **3** teams and these rows:
```
0 1 2
3 0 2
2 2 0
```

Team 2 beat Team 1 3–1 and the other two matches were 2–2 draws. The points are 1, 4 and 2, so **Team 2** is the champion with **4** points and a goal difference of:
`(3 + 2) - (1 + 2) = 2`

**Input Format:**
* **N**, after the prompt `Enter number of teams:`
* The program then prints `Enter the score table:` on its own line and reads **N** lines of **N** space-separated integers. Line `i` holds the goals of team `i` against teams 1 to **N**.

**Output Format:**
* `Champion: Team X`, the champion's team number
* `Points: P`, the champion's points
* `Goal Difference: G`, the champion's goal difference

**Constraints:**
* $2 \le \mathbf{N} \le 10$, and every goal value is from 0 to 20.
* Every diagonal value (row `i`, column `i`) is 0 and is ignored.
* Ties: most points first, then higher goal difference, then smaller team number.
* The goal difference can be positive, zero or negative (printed with a leading minus sign).

### Sample Outputs

```
Sample Output 1
Enter number of teams: 3
Enter the score table:
0 1 2
3 0 2
2 2 0
Champion: Team 2
Points: 4
Goal Difference: 2

Sample Output 2
Enter number of teams: 4
Enter the score table:
0 2 0 3
0 0 1 1
1 0 0 5
0 1 0 0
Champion: Team 3
Points: 6
Goal Difference: 5

Sample Output 3
Enter number of teams: 2
Enter the score table:
0 0
0 0
Champion: Team 1
Points: 1
Goal Difference: 0
```

## ANSWER 7

```python
N = int(input("Enter number of teams: "))
print("Enter the score table:")
table = [list(map(int, input().split())) for _ in range(N)]

best_team = None

for i in range(N):
    points = 0
    goals_for = 0
    goals_against = 0

    for j in range(N):
        if i == j:
            continue
        gf = table[i][j]
        ga = table[j][i]
        goals_for += gf
        goals_against += ga

        if gf > ga:
            points += 3
        elif gf == ga:
            points += 1

    goal_diff = goals_for - goals_against
    # Rank key: higher points, higher goal_diff, smaller team number (i + 1)
    candidate = (points, goal_diff, -(i + 1), i + 1)

    if best_team is None or candidate[:3] > best_team[:3]:
        best_team = candidate

_, best_points, _, champ_num = best_team
best_gd = best_team[1]

print(f"Champion: Team {champ_num}")
print(f"Points: {best_points}")
print(f"Goal Difference: {best_gd}")
```

---

# QUESTION 8

**Four-Drop Showdown**

**Problem Statement:**
In Connect Four, players take turns dropping discs into the columns of an upright board, and each disc falls to the **lowest empty cell** of its column. **A** and **B** play on **R** rows and **C** columns, numbered 1 to **C** from the left. **A** drops first.
<br><br>
A player wins as soon as their new disc completes an unbroken **horizontal** or **vertical** line of **4 or more** of their discs. Later drops are ignored. **Diagonal lines never count.**
<br><br>
For example, on a $\mathbf{6 \times 7}$ board with the drops `1 2 2 3 3 4 3 4 4 1 4 5 6`, **A**'s drop 11 completes only a diagonal. **B**'s drop 12 lands at the bottom of column 5, beside **B**'s bottom discs in columns 2, 3 and 4, so **B** wins on move **12**.
<br><br>
The board after move 12 (`A` for **A**'s discs, `B` for **B**'s discs, `X` for empty cells):
```
X X X X X X X
X X X X X X X
X X X A X X X
X X A A X X X
B A A B X X X
A B B B B X X
```

**Input Format:**
* **R** and **C**, after `Enter rows and columns:`
* **M**, after `Enter number of moves:`
* The **M** column numbers, after `Enter the moves:`

**Output Format:**
* `Winner: X on move K`, where **X** is the winner and **K** the winning drop's number (from 1)
* `Draw`, if nobody wins

**Constraints:**
* $4 \le \mathbf{R}, \mathbf{C} \le 10$, $1 \le \mathbf{M} \le \mathbf{R \times C}$, and each column number is 1 to **C**.
* No column gets more than **R** drops, ignored ones included. The board may end up full.
* The new disc may be at the end or the middle of the line, the line may be longer than 4, and the win may come on the last drop.
* A diagonal of 4 may form before the real win.

### Sample Outputs

```
Sample Output 1
Enter rows and columns: 6 7
Enter number of moves: 13
Enter the moves: 1 2 2 3 3 4 3 4 4 1 4 5 6
Winner: B on move 12
```

## ANSWER 8

```python
R, C = map(int, input("Enter rows and columns: ").split())
M = int(input("Enter number of moves: "))
moves = list(map(int, input("Enter the moves: ").split()))

# board[0] is top row, board[R - 1] is bottom row
board = [['X' for _ in range(C)] for _ in range(R)]
winner = None
winning_move = None

for idx, col_1based in enumerate(moves):
    if winner is not None:
        break

    move_num = idx + 1
    player = 'A' if move_num % 2 == 1 else 'B'
    col = col_1based - 1

    # Drop disc to lowest empty cell in column
    placed_row = -1
    for r in range(R - 1, -1, -1):
        if board[r][col] == 'X':
            board[r][col] = player
            placed_row = r
            break

    if placed_row == -1:
        continue

    # Check horizontal line through (placed_row, col)
    h_count = 1
    c = col - 1
    while c >= 0 and board[placed_row][c] == player:
        h_count += 1
        c -= 1
    c = col + 1
    while c < C and board[placed_row][c] == player:
        h_count += 1
        c += 1

    # Check vertical line through (placed_row, col)
    v_count = 1
    r = placed_row - 1
    while r >= 0 and board[r][col] == player:
        v_count += 1
        r -= 1
    r = placed_row + 1
    while r < R and board[r][col] == player:
        v_count += 1
        r += 1

    if h_count >= 4 or v_count >= 4:
        winner = player
        winning_move = move_num

if winner is not None:
    print(f"Winner: {winner} on move {winning_move}")
else:
    print("Draw")
```

# QUESTION 5

**Road Leveling Passes**

**Problem Statement:**

A straight road is split into **N** sections, numbered 1 to **N** from left to right. Each section has a height: positive is a bump, negative is a dip, and **0 is level**.

In one pass, a leveling machine picks one unbroken stretch of neighbouring sections (a single section counts) and either lowers all of them by 1 or raises all of them by 1. A stretch may include any sections, even level ones.

Find the fewest passes that make every section **level**.

For example, take the heights `2 3 1 -2 -2 0 1`:

- Lower sections 1 to 3 once, giving `1 2 0 -2 -2 0 1`.
- Lower sections 1 to 2 once, giving `0 1 0 -2 -2 0 1`.
- Lower section 2 once, giving `0 0 0 -2 -2 0 1`.
- Raise sections 4 to 5 twice, giving `0 0 0 0 0 0 1`.
- Lower section 7 once, giving `0 0 0 0 0 0 0`.

That is 6 passes, no plan uses fewer, so the answer is **6**.

**Input Format:**

- The integer **N**, read after the prompt `Enter number of sections:`
- The **N** heights on one line, section 1 first, separated by spaces, read after the prompt `Enter heights:`

**Output Format:**

- `Fewest passes:` X on its own line.

**Constraints:**

- 1 <= N <= 100
- Every height is an integer from -1000 to 1000, and a height may be 0.
- Every section may already be level; the answer is then 0.
- Neighbouring sections may be a bump and a dip.

### Sample Outputs

```text
Sample Output 1
Enter number of sections: 7
Enter heights: 2 3 1 -2 -2 0 1
Fewest passes: 6

Sample Output 2
Enter number of sections: 3
Enter heights: 5 1 5
Fewest passes: 9

Sample Output 3
Enter number of sections: 1
Enter heights: -7
Fewest passes: 7
```

## ANSWER 5

```python
n = int(input("Enter number of sections: "))
heights = list(map(int, input("Enter heights: ").split()))

# Pad with level ground on both sides, then every rise between
# neighbours must be paid for by at least one pass starting there.
padded = [0] + heights + [0]

passes = 0
for i in range(1, len(padded)):
    diff = padded[i] - padded[i - 1]
    if diff > 0:
        passes += diff

print(f"Fewest passes: {passes}")
```

---

# QUESTION 6

**Round Table Regrouping**

**Problem Statement:**

Seats around a round table are numbered 1 to **N** in order, and seat **N** is next to seat 1. Each seat holds one person: `F` marks one of your friends and `.` marks anyone else.

You want all your friends to sit together, that is, in seats that form one unbroken stretch around the table. The stretch may pass from seat **N** to seat 1. In one **swap**, any two people trade seats; they do not have to be neighbours. Find the smallest number of swaps that seats all your friends together.

For example, in `F..F.F.F..` the friends sit in seats 1, 4, 6 and 8.

- Swap seats 1 and 5, giving `...FFF.F..`.
- Swap seats 8 and 7, giving `...FFFF...`.

The friends are now in seats 4 to 7. No single swap can do it, so the answer is **2**.

**Input Format:**

- One word of **N** characters, seat 1 first, read after the prompt `Enter seats:`

**Output Format:**

- `Minimum swaps:` X on its own line.

**Constraints:**

- 1 <= N <= 100, and every character is `F` (uppercase) or `.`
- There is at least one friend.
- The friends may already sit together, even across seats **N** and 1; the answer is then 0.
- The best stretch may cross from seat **N** to seat 1.

### Sample Outputs

```text
Sample Output 1
Enter seats: F..F.F.F..
Minimum swaps: 2

Sample Output 2
Enter seats: FF......F.F
Minimum swaps: 1

Sample Output 3
Enter seats: F.......FF
Minimum swaps: 0
```

## ANSWER 6

```python
seats = input("Enter seats: ")

n = len(seats)
k = seats.count("F")

# Doubling the string lets a window wrap from seat N back to seat 1.
doubled = seats + seats

best = k
for start in range(n):
    window = doubled[start:start + k]
    best = min(best, window.count("."))

print(f"Minimum swaps: {best}")
```

---

# QUESTION 7

**Ranked Ballot Runoff**

**Problem Statement:**

**N** candidates are named by the first **N** uppercase letters. Each ballot is a word listing some candidates, favourite first. Count in **rounds**:

1. Each ballot votes for its **earliest** listed candidate still in the race; a ballot with none left is **exhausted** and not counted.
2. A candidate with **more than half** of the counted votes wins.
3. Otherwise, if all remaining candidates have equal votes, the count ends in a **tie** among them.
4. Otherwise, every candidate with the fewest votes (0 included) is removed, and a new round begins.

For example, with 4 candidates and ballots `AB BCA CBA D DC ACB BC`:

- Round 1: A 2, B 2, C 1, D 2 votes, so C is removed.
- Round 2: A 2, B 3, D 2 votes (3 is not more than half of 7), so A and D are removed.
- Round 3: `D` and `DC` are exhausted and B has all 5 votes.

**B** wins after 3 rounds, 2 ballots exhausted.

**Input Format:**

- **N**, after the prompt `Enter number of candidates:`
- **M**, the number of ballots, after the prompt `Enter number of ballots:`
- The **M** ballots on one line, space-separated, after the prompt `Enter ballots:`

**Output Format:**

- `Rounds:` and the number of rounds, on its own line.
- `Exhausted:` and the number of ballots exhausted in the last round, on its own line.
- `Winner:` and the winner, or `Tie:` and the tied letters alphabetically, each preceded by one space, on its own line.

**Constraints:**

- 2 <= N <= 6 and 1 <= M <= 25.
- Each ballot lists 1 to **N** different candidates; a candidate may be on no ballot.
- Exactly half is not a win.
- A tie can happen in any round, among two or more candidates.
- Several candidates, even with 0 votes, can be removed at once.

### Sample Outputs

```text
Sample Output 1
Enter number of candidates: 4
Enter number of ballots: 7
Enter ballots: AB BCA CBA D DC ACB BC
Rounds: 3
Exhausted: 2
Winner: B

Sample Output 2
Enter number of candidates: 3
Enter number of ballots: 6
Enter ballots: AB AC AB BA CB CA
Rounds: 2
Exhausted: 0
Winner: A

Sample Output 3
Enter number of candidates: 4
Enter number of ballots: 6
Enter ballots: A B CA CB D DC
Rounds: 2
Exhausted: 2
Tie: C D
```

## ANSWER 7

```python
n = int(input("Enter number of candidates: "))
m = int(input("Enter number of ballots: "))
ballots = input("Enter ballots: ").split()

remaining = [chr(ord("A") + i) for i in range(n)]
rounds = 0

while True:
    rounds += 1
    votes = {c: 0 for c in remaining}
    exhausted = 0

    for ballot in ballots:
        for ch in ballot:
            if ch in votes:
                votes[ch] += 1
                break
        else:
            exhausted += 1

    counted = sum(votes.values())

    winner = None
    for c in remaining:
        if votes[c] * 2 > counted:
            winner = c

    if winner is not None:
        print(f"Rounds: {rounds}")
        print(f"Exhausted: {exhausted}")
        print(f"Winner: {winner}")
        break

    if len(set(votes.values())) == 1:
        print(f"Rounds: {rounds}")
        print(f"Exhausted: {exhausted}")
        print("Tie: " + " ".join(sorted(remaining)))
        break

    fewest = min(votes.values())
    remaining = [c for c in remaining if votes[c] != fewest]
```

---

# QUESTION 8

**Flip-Gate Pegboard**

**Problem Statement:**

A pegboard has **R** rows and **C** columns of cells: `.` (empty), `<` (left gate), `>` (right gate) and `#` (block). You drop **K** balls one at a time, each into a given column, and every effect a ball makes stays for the balls after it.

Drop a ball into the top cell of its column. If that cell is `#`, the drop is **rejected**. Otherwise, the ball goes down one row at a time, and the cell it is on decides where it goes next:

- `.` sends it straight down.
- `<` sends it down one column to the **left**.
- `>` sends it down one column to the **right**.

Check each move in this order:

1. If the move would go off the left or right side of the board, or onto a `#`, the ball **stops** where it is and that cell becomes `#`.
2. Otherwise, if the ball is on the bottom row, it falls into the **bin** under the column it moves to.
3. Otherwise, the ball moves to the new cell.

Whenever a ball leaves a gate (`<` or `>`) into a new cell or into a bin, the gate flips: `<` becomes `>` and `>` becomes `<`.

For example, take this board with drops `1 1 4 2 1`:

```text
>..<
.<..
..#.
```

Ball 1 goes right, then left, and lands in bin 1, flipping both gates.

Ball 2 is sent left off the board from the top-left cell and stops there, so that cell becomes `#`.

Ball 3 moves from column 4 into row 2, then is blocked by the `#` below and stops.

Ball 4 meets the flipped `>` in row 2 and stops above the same `#`.

Ball 5 is rejected because column 1's top cell is now `#`.

So the bins are `1 0 0 0`, 3 balls stopped and 1 drop was rejected.

**Input Format:**

- Print `Enter rows and columns:` and read **R** and **C**.
- Print `Enter the board:` on its own line, then read **R** lines of exactly **C** characters each.
- Print `Enter number of balls:` and read **K**.
- Print `Enter drop columns:` and read **K** column numbers (1 = leftmost) on one line.

**Output Format:**

- `Bins:` followed by the **C** bin counts from left to right, each preceded by one space.
- `Stopped:` followed by the number of balls that stopped.
- `Rejected:` followed by the number of rejected drops.
- `Final board:` on its own line, then the **R** rows of the final board.

**Constraints:**

- 1 <= R, C <= 20 and 1 <= K <= 100; every drop column is from 1 to **C**.
- Blocks may appear anywhere, including the top and bottom rows.
- A gate on the bottom row sends its ball into the bin to its left or right.
- Bins, the stopped count and the rejected count can all be 0.

### Sample Outputs

```text
Sample Output 1
Enter rows and columns: 3 4
Enter the board:
>..<
.<..
..#.
Enter number of balls: 5
Enter drop columns: 1 1 4 2 1
Bins: 1 0 0 0
Stopped: 3
Rejected: 1
Final board:
#..>
.##.
..#.

Sample Output 2
Enter rows and columns: 3 5
Enter the board:
..>..
.<.>.
.....
Enter number of balls: 5
Enter drop columns: 3 3 3 3 3
Bins: 1 0 2 0 2
Stopped: 0
Rejected: 0
Final board:
..<..
.<.<.
.....

Sample Output 3
Enter rows and columns: 1 1
Enter the board:
>
Enter number of balls: 3
Enter drop columns: 1 1 1
Bins: 0
Stopped: 1
Rejected: 2
Final board:
#
```

## ANSWER 8

```python
R, C = map(int, input("Enter rows and columns: ").split())
print("Enter the board:")
board = [list(input()) for _ in range(R)]
K = int(input("Enter number of balls: "))
drops = list(map(int, input("Enter drop columns: ").split()))

bins = [0] * C
stopped = 0
rejected = 0
flip = {"<": ">", ">": "<"}

for d in drops:
    col = d - 1

    if board[0][col] == "#":
        rejected += 1
        continue

    r, c = 0, col
    while True:
        cell = board[r][c]

        if cell == "<":
            nc = c - 1
        elif cell == ">":
            nc = c + 1
        else:
            nc = c

        # Rule 1: off the side, or onto a block -> ball stops here
        if nc < 0 or nc >= C or (r + 1 < R and board[r + 1][nc] == "#"):
            board[r][c] = "#"
            stopped += 1
            break

        # The ball leaves this cell, so a gate flips
        if cell in flip:
            board[r][c] = flip[cell]

        # Rule 2: bottom row -> falls into the bin
        if r == R - 1:
            bins[nc] += 1
            break

        # Rule 3: move to the new cell
        r += 1
        c = nc

print("Bins: " + " ".join(map(str, bins)))
print(f"Stopped: {stopped}")
print(f"Rejected: {rejected}")
print("Final board:")
for row in board:
    print("".join(row))
```

# 1. CALCULATOR WITHOUT ZEROES

<img width="448" height="789" alt="image" src="https://github.com/user-attachments/assets/a119c9e1-ddf1-42f5-a8f2-2e85bdeafb97" />

## SOLUTION
```
#include<stdio.h>

int main() {
    int N, D = 0, i = 1, temp;
    
    printf("Enter number: ");
    scanf("%d", &N);
    
    while (N > 0) {
        temp = N % 10;
        N /= 10;
        // i *= 10;
        
        if (temp == 0) continue;
        // D += temp * i;
        D += temp * i;
        i *= 10;
    }

```
> **Note:** These practice questions are incomplete. I could not get pictures of the rest of the questions, so only the ones below are covered.

---

# QUESTION 2

**Rotting Fruit**

*Based on LeetCode 994: Rotting Oranges*

**Problem Statement:**

A crate of fruit is laid out in a grid of **R** rows and **C** columns. Each cell is one of:

- `0`: an empty cell
- `1`: a fresh fruit
- `2`: a rotten fruit

Every minute, each fresh fruit that is directly next to a rotten fruit (up, down, left or right) becomes rotten.

Find the minimum number of minutes that must pass until no fresh fruit is left. If some fresh fruit can never rot, the answer is **-1**.

For example, with the grid:

```text
2 1 1
1 1 0
0 1 1
```

- After minute 1, the fruit next to the rotten one rots.
- After minute 2 and 3, the rot keeps spreading.
- After minute 4, the last fresh fruit rots.

So the answer is **4 minutes**.

**Input Format:**

- **R** and **C**, space-separated, after the prompt `Enter rows and columns:`
- The word `Enter the grid:` on its own line, then **R** lines of **C** space-separated numbers.

**Output Format:**

- `Minutes:` followed by the number of minutes (or -1), on its own line.

**Constraints:**

- 1 <= R, C <= 10
- Every cell is 0, 1 or 2.
- If there is no fresh fruit at the start, the answer is 0.
- Fresh fruit that no rotten fruit can ever reach makes the answer -1.

### Sample Outputs

```text
Sample Output 1
Enter rows and columns: 3 3
Enter the grid:
2 1 1
1 1 0
0 1 1
Minutes: 4

Sample Output 2
Enter rows and columns: 3 3
Enter the grid:
2 1 1
0 1 1
1 0 1
Minutes: -1

Sample Output 3
Enter rows and columns: 1 2
Enter the grid:
0 2
Minutes: 0
```

## ANSWER 2

```python
from collections import deque

R, C = map(int, input("Enter rows and columns: ").split())
print("Enter the grid:")
grid = [list(map(int, input().split())) for _ in range(R)]

queue = deque()
fresh = 0

for r in range(R):
    for c in range(C):
        if grid[r][c] == 2:
            queue.append((r, c))
        elif grid[r][c] == 1:
            fresh += 1

minutes = 0

while queue and fresh > 0:
    for _ in range(len(queue)):
        r, c = queue.popleft()
        for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            nr, nc = r + dr, c + dc
            if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc))
    minutes += 1

print(f"Minutes: {minutes if fresh == 0 else -1}")
```

---

# QUESTION 3

**Broken Street Lamps**

*Based on LeetCode 1004: Max Consecutive Ones III (sliding window over a string)*

**Problem Statement:**

A street has a row of lamps. Each lamp is either working (`1`) or broken (`0`).

The city can repair **at most one** broken lamp. Find the length of the longest unbroken streak of consecutive working lamps after the best possible repair.

For example, for `1101110`:

- Repairing the broken lamp in position 3 gives `1111110`.
- The longest streak of working lamps is now 6.

So the answer is **6**.

**Input Format:**

- One line of `0` and `1` characters, read after the prompt `Enter lamps:`

**Output Format:**

- `Longest streak:` followed by the length, on its own line.

**Constraints:**

- 1 to 100 lamps.
- Every character is `0` or `1`.
- At most one lamp can be repaired; repairing is optional.
- If every lamp is broken, repairing one gives a streak of 1.

### Sample Outputs

```text
Sample Output 1
Enter lamps: 1101110
Longest streak: 6

Sample Output 2
Enter lamps: 1111
Longest streak: 4

Sample Output 3
Enter lamps: 000
Longest streak: 1
```

## ANSWER 3

```python
lamps = input("Enter lamps: ")

left = 0
broken = 0
best = 0

for right in range(len(lamps)):
    if lamps[right] == "0":
        broken += 1

    # Keep the window to at most one broken lamp
    while broken > 1:
        if lamps[left] == "0":
            broken -= 1
        left += 1

    best = max(best, right - left + 1)

print(f"Longest streak: {best}")
```

---

# QUESTION 4

**Warmer Days**

*Based on LeetCode 1732: Find the Highest Altitude (running comparison over an array)*

**Problem Statement:**

A weather station recorded the temperature on each of **N** days. Find the **warmest** temperature recorded.

For example, with temperatures `21 25 19 30 27`:

- The warmest day was 30 degrees.

So the answer is **30**.

**Input Format:**

- **N**, after the prompt `Enter number of days:`
- The **N** temperatures on one line, space-separated, after the prompt `Enter temperatures:`

**Output Format:**

- `Warmest:` followed by the highest temperature, on its own line.

**Constraints:**

- 1 <= N <= 100
- Every temperature is an integer from -50 to 60.
- Temperatures may be negative.
- If only one day was recorded, that day is the warmest.

### Sample Outputs

```text
Sample Output 1
Enter number of days: 5
Enter temperatures: 21 25 19 30 27
Warmest: 30

Sample Output 2
Enter number of days: 3
Enter temperatures: -5 -2 -9
Warmest: -2

Sample Output 3
Enter number of days: 1
Enter temperatures: 18
Warmest: 18
```

## ANSWER 4

```python
n = int(input("Enter number of days: "))
temps = list(map(int, input("Enter temperatures: ").split()))

warmest = temps[0]

for t in temps:
    if t > warmest:
        warmest = t

print(f"Warmest: {warmest}")
```

---

# QUESTION 5

**Longest Word in String**

*Based on LeetCode 1446 / 2490-style string scanning: finding the longest word in a sentence*

**Problem Statement:**

Given a sentence, find the **longest word** in it. Words are separated by single spaces.

If two or more words are tied for the longest length, report the one that appears **first**.

For example, in `the quick brown fox jumped`:

- The word lengths are 3, 5, 5, 3 and 6.
- The longest word is `jumped`, with 6 letters.

**Input Format:**

- One sentence on one line, read after the prompt `Enter sentence:`

**Output Format:**

Two lines:
- `Longest word:` followed by the word, on its own line.
- `Length:` followed by the number of letters, on its own line.

**Constraints:**

- The sentence has 1 to 100 words.
- Words contain letters only and are separated by single spaces.
- On a tie, the first longest word is reported.

### Sample Outputs

```text
Sample Output 1
Enter sentence: the quick brown fox jumped
Longest word: jumped
Length: 6

Sample Output 2
Enter sentence: I love cats and dogs
Longest word: love
Length: 4

Sample Output 3
Enter sentence: hello
Longest word: hello
Length: 5
```

## ANSWER 5

```python
sentence = input("Enter sentence: ")

words = sentence.split()

longest = words[0]

for word in words:
    if len(word) > len(longest):
        longest = word

print(f"Longest word: {longest}")
print(f"Length: {len(longest)}")
```

---

# QUESTION 6

**Library Fines**

*Based on HackerRank "Library Fine" and LeetCode-style per-item cost with a cap*

**Problem Statement:**

A library charges a fine for each borrowed book depending on how many days it was kept:

- A book kept for **7 days or less** is **free**.
- A book kept for more than 7 days is charged **$5 for every day over 7**.
- The fine for any single book is **capped at $65**.

Given the days each book was borrowed, find the total fine.

For example, with 3 books borrowed for `5 10 30` days:

- 5 days: free, so $0.
- 10 days: 3 days over 7, so 3 × 5 = $15.
- 30 days: 23 days over 7, so 23 × 5 = $115, capped at $65.

Total: 0 + 15 + 65 = **$80**.

**Input Format:**

- **N**, the number of books, after the prompt `Enter number of books:`
- The **N** borrowed-day counts on one line, space-separated, after the prompt `Enter days borrowed:`

**Output Format:**

- `Total fine: $X` on its own line.

**Constraints:**

- 1 <= N <= 100
- Each day count is from 1 to 365.
- 7 days exactly is still free.
- The $65 cap applies to each book separately, not to the total.

### Sample Outputs

```text
Sample Output 1
Enter number of books: 3
Enter days borrowed: 5 10 30
Total fine: $80

Sample Output 2
Enter number of books: 2
Enter days borrowed: 7 3
Total fine: $0

Sample Output 3
Enter number of books: 2
Enter days borrowed: 14 100
Total fine: $100
```

## ANSWER 6

```python
n = int(input("Enter number of books: "))
days = list(map(int, input("Enter days borrowed: ").split()))

total = 0

for d in days:
    if d > 7:
        fine = (d - 7) * 5
        total += min(fine, 65)

print(f"Total fine: ${total}")
```

---

# QUESTION 7

**Theater Seats**

*Based on LeetCode 441: Arranging Coins (rows that grow by a fixed step)*

**Problem Statement:**

A theater's first row has **N** seats, and every row after it has **2 more seats** than the row before it.

Given the number of rows, find the total number of seats in the theater.

For example, if the first row has 10 seats and there are 4 rows:

- Row 1: 10 seats
- Row 2: 12 seats
- Row 3: 14 seats
- Row 4: 16 seats

Total: 10 + 12 + 14 + 16 = **52 seats**.

**Input Format:**

- **N**, the seats in the first row, after the prompt `Enter first row seats:`
- The number of rows, after the prompt `Enter number of rows:`

**Output Format:**

- `Total seats:` followed by the total, on its own line.

**Constraints:**

- 1 <= N <= 100
- 1 <= rows <= 100
- Each row has exactly 2 more seats than the one before it.
- With a single row, the total is just N.

### Sample Outputs

```text
Sample Output 1
Enter first row seats: 10
Enter number of rows: 4
Total seats: 52

Sample Output 2
Enter first row seats: 5
Enter number of rows: 1
Total seats: 5

Sample Output 3
Enter first row seats: 20
Enter number of rows: 3
Total seats: 66
```

## ANSWER 7

```python
N = int(input("Enter first row seats: "))
rows = int(input("Enter number of rows: "))

total = 0
seats = N

for _ in range(rows):
    total += seats
    seats += 2

print(f"Total seats: {total}")
```

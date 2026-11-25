
# QUESTION 1

**TV Remote Presses**

**Problem Statement:**

A TV starts on channel 1, and each up or down press moves one channel. A viewer picks **N** channels in order:

- Picking the channel already showing needs no presses.
- Any other pick is one change and costs one press per channel moved.

For example, with picks `4 4 9 2 2 6`:

- 1 to 4 takes 3 presses.
- The repeated 4 needs none.
- 4 to 9 takes 5.
- 9 to 2 takes 7.
- The repeated 2 needs none.
- 2 to 6 takes 4.

Total: 3 + 5 + 7 + 4 = **19 presses, 4 changes.**

**Input Format:**

- **N**, after the prompt `Enter number of picks:`
- The **N** channels on one line, space-separated, after the prompt `Enter channels:`

**Output Format:**

Two lines:
- `Presses: P` on its own line
- `Changes: C` on its own line

**Constraints:**

- 1 <= N <= 20, channels 1 to 50.
- Picking 1 first needs no presses. If no pick changes the channel, both answers are 0.
- Returning to a channel left earlier is a new change.

### Sample Outputs

```text
Sample Output 1
Enter number of picks: 7
Enter channels: 2 3 4 5 6 7 8
Presses: 7
Changes: 7

Sample Output 2
Enter number of picks: 6
Enter channels: 4 4 9 2 2 6
Presses: 19
Changes: 4

Sample Output 3
Enter number of picks: 1
Enter channels: 1
Presses: 0
Changes: 0
```

## ANSWER 1

```python
N = int(input("Enter number of picks: "))
channels = list(map(int, input("Enter channels: ").split()))

current = 1
presses = 0
changes = 0

for channel in channels:
    if channel != current:
        presses += abs(channel - current)
        changes += 1
        current = channel

print(f"Presses: {presses}")
print(f"Changes: {changes}")
```

---

# QUESTION 2

**Luhn Check Digit**

**Problem Statement:**

Some ID and card numbers end with a check digit computed from the digits before it. Given those digits, compute the check digit with the Luhn method:

1. Number digits from the right: the rightmost digit is position 1, the one to its left is position 2, and so on (basically, reversing the number).
2. Double every digit at an odd position (1, 3, 5, ...). If a doubled value is more than 9, subtract 9 from it.
3. Digits at even positions stay as they are.
4. Add up all the values to get the total.
5. The check digit is the digit from 0 to 9 that makes total + check digit a multiple of 10.

For example, for `5847`:

Its reverse is `7485`, so position 1 is 7, position 2 is 4, and so on.

The digits at odd positions are: **7 4 8 5**

- 7 × 2 = 14, greater than 9: 14 - 9 = 5
- 8 × 2 = 16, greater than 9: 16 - 9 = 7
- 5 + 4 + 7 + 5 = 21

21 + 9 = 30, so the check digit is **9**.

**Input Format:**

- One line of digits, read after the prompt `Enter the digits:`

**Output Format:**

- `Check digit:` followed by the digit, on its own line.

**Constraints:**

- 1 to 30 digits; leading zeros count as digits.
- A single digit is at position 1, so it is doubled.
- If the total is already a multiple of 10, the check digit is 0.

### Sample Outputs

```text
Sample Output 1
Enter the digits: 5847
Check digit: 9

Sample Output 2
Enter the digits: 8350126
Check digit: 2

Sample Output 3
Enter the digits: 6
Check digit: 7
```

## ANSWER 2

```python
digits = input("Enter the digits: ")

total = 0

for i in range(len(digits)):
    digit = int(digits[-(i + 1)])

    if i % 2 == 0:
        digit *= 2
        if digit > 9:
            digit -= 9

    total += digit

check_digit = (10 - (total % 10)) % 10

print(f"Check digit: {check_digit}")
```

---

# QUESTION 3

**Identifier Style Converter**

**Problem Statement:**

Identifiers come in three styles:

- **camelCase**: letters only; the first word is lowercase and each later word starts with an uppercase letter. **Every** uppercase letter starts a new word, so consecutive uppercase letters are one-letter words.
- **snake_case**: lowercase words joined by underscores (`_`).
- **kebab-case**: lowercase words joined by hyphens (`-`).

Detect the identifier's style, print it in the other two styles, and count its words.

For example, `user_login_count` is **snake_case** with the words user, login and count.

- In camelCase it is `userLoginCount`.
- In kebab-case it is `user-login-count`.
- It has **3 words**.

**Input Format:**

- One identifier, read after the prompt `Enter identifier:`

**Output Format:**

- `Detected:` followed by the style name, on its own line.
- The other two styles in the order camelCase, snake_case, kebab-case, each on its own line as the style name, `:`, then the converted identifier.
- `Words:` followed by the word count, on its own line.

**Constraints:**

- The identifier has 1 to 100 characters and follows one of the three styles.
- An identifier with no underscore, hyphen or uppercase letter is one word and is treated as camelCase; it may be a single letter.
- Any word, even the first or last, may be one letter.
- Separators never start or end an identifier or appear twice in a row.

### Sample Outputs

```text
Sample Output 1
Enter identifier: user_login_count
Detected: snake_case
camelCase: userLoginCount
kebab-case: user-login-count
Words: 3

Sample Output 2
Enter identifier: parse-x-m-l-file
Detected: kebab-case
camelCase: parseXMLFile
snake_case: parse_x_m_l_file
Words: 5

Sample Output 3
Enter identifier: readCSVRowA
Detected: camelCase
snake_case: read_c_s_v_row_a
kebab-case: read-c-s-v-row-a
Words: 6
```

## ANSWER 3

```python
identifier = input("Enter identifier: ")

if "_" in identifier:
    style = "snake_case"
    words = identifier.split("_")
elif "-" in identifier:
    style = "kebab-case"
    words = identifier.split("-")
else:
    style = "camelCase"
    words = []
    for ch in identifier:
        if ch.isupper() or not words:
            words.append(ch.lower())
        else:
            words[-1] += ch

camel = words[0] + "".join(w.capitalize() for w in words[1:])
snake = "_".join(words)
kebab = "-".join(words)

print(f"Detected: {style}")
if style != "camelCase":
    print(f"camelCase: {camel}")
if style != "snake_case":
    print(f"snake_case: {snake}")
if style != "kebab-case":
    print(f"kebab-case: {kebab}")
print(f"Words: {len(words)}")
```

---

# QUESTION 4

**Dice Poker Scorer**

**Problem Statement:**

Check the hands below from best to worst and report the **first** match. The score is the hand's base points **plus the sum of all five dice**.

- **Five of a Kind**: all five match. Base 50.
- **Four of a Kind**: four match. Base 40.
- **Full House**: three of one face, two of another. Base 30.
- **Three of a Kind**: three match. Base 20.
- **Two Pair**: two faces each appear exactly twice. Base 10.
- **One Pair**: one face appears exactly twice. Base 5.
- **Nothing**: none of the above. Base 0.

For example, `3 5 3 5 3` has three 3s and two 5s, so it is a **Full House**.

The dice sum to 3 + 5 + 3 + 5 + 3 = 19, so the score is:

30 + 19 = **49**

**Input Format:**

- Five space-separated integers, read after the prompt `Enter five dice:`

**Output Format:**

- `Hand:` followed by the hand name exactly as listed, on its own line.
- `Score:` followed by the score, on its own line.

**Constraints:**

- Each die is 1 to 6; order does not matter.
- Report only the highest matching hand: a Full House is not also reported as Three of a Kind or One Pair.
- There are no straights: five different faces, even consecutive ones, score as Nothing.
- Hand names are case-sensitive.

### Sample Outputs

```text
Sample Output 1
Enter five dice: 3 5 3 5 3
Hand: Full House
Score: 49

Sample Output 2
Enter five dice: 2 6 2 1 4
Hand: One Pair
Score: 20

Sample Output 3
Enter five dice: 4 4 4 4 4
Hand: Five of a Kind
Score: 70
```

## ANSWER 4

```python
from collections import Counter

dice = list(map(int, input("Enter five dice: ").split()))

counts = sorted(Counter(dice).values(), reverse=True)

if counts == [5]:
    hand, base = "Five of a Kind", 50
elif counts == [4, 1]:
    hand, base = "Four of a Kind", 40
elif counts == [3, 2]:
    hand, base = "Full House", 30
elif counts == [3, 1, 1]:
    hand, base = "Three of a Kind", 20
elif counts == [2, 2, 1]:
    hand, base = "Two Pair", 10
elif counts == [2, 1, 1, 1]:
    hand, base = "One Pair", 5
else:
    hand, base = "Nothing", 0

print(f"Hand: {hand}")
print(f"Score: {base + sum(dice)}")
```

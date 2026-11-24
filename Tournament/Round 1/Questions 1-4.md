# QUESTION 1

**Home Row Letters**

**Problem Statement:**<br>
On a QWERTY keyboard, the middle row of letters is the **home row**: `asdfghjkl`. Given a word, count how many of its letters are on the home row.

For example, in `keyboard` only `k`, `a` and `d` are home row letters, so the count is **3**.

**Input Format:**
* One word, read after the prompt `Enter word:`

**Output Format:**
* `Home row letters: X`, where **X** is the number of home row letters in the word

**Constraints:**
* The word has 1 to 100 characters, all lowercase letters `a` to `z`.
* A letter that appears several times is counted every time it appears.
* Every letter may be on the home row, or none may be (the count is then 0).

### Sample Outputs

```
Sample Output 1
Enter word: keyboard
Home row letters: 3

Sample Output 2
Enter word: flask
Home row letters: 5

Sample Output 3
Enter word: z
Home row letters: 0
```

## ANSWER 1

```python
word = input("Enter word: ")
home_row = "asdfghjkl"
count = sum(1 for char in word if char in home_row)
print(f"Home row letters: {count}")
```

---

# QUESTION 2

**Fewest Peso Pieces**

**Problem Statement:**<br>
A cashier pays out **N** pesos using only four kinds of pieces: 100, 20, 5 and 1 peso. The cashier hands over as many 100s as fit, then as many 20s as fit in what is left, then 5s, and finally 1s. Print how many pieces are paid out.

For example, **389** is paid as three 100s, four 20s, one 5 and four 1s, so the cashier uses:
3 + 4 + 1 + 4 = **12** pieces

**Input Format:**
* The integer **N**, read after the prompt `Enter amount:`

**Output Format:**
* `Pieces: P`, where **P** is the number of pieces paid out

**Constraints:**
* 1 <= N <= 1000000000
* There is an unlimited supply of every piece, so a value may be used several times or not at all.
* **N** may be paid with a single piece, or with 1-peso pieces only (when **N** is less than 5).

### Sample Outputs

```
Sample Output 1
Enter amount: 389
Pieces: 12

Sample Output 2
Enter amount: 1
Pieces: 1

Sample Output 3
Enter amount: 125
Pieces: 3
```

## ANSWER 2

```python
n = int(input("Enter amount: "))
pieces = 0

for denom in [100, 20, 5, 1]:
    pieces += n // denom
    n %= denom

print(f"Pieces: {pieces}")
```

---

# QUESTION 3

**Sticky Keyboard Replay**

**Problem Statement:**<br>
Replay a line of keystrokes from left to right and print the text left on the screen. The screen starts empty and Caps Lock starts **OFF**.

* A letter is added to the end of the text, with its case flipped if Caps Lock is **ON**.
* `^` toggles Caps Lock.
* `<` deletes the last character.

The symbols are never typed into the text.

For example, for `ab^cD<e^f`:
`ab` is typed.
`^` turns Caps Lock ON.
`c` and `D` become `C` and `d`, giving `abCd`.
`<` removes the `d`.
`e` becomes `E`.
`^` turns Caps Lock OFF.
`f` stays `f`, so the final text is `abCEf`.

**Input Format:**
* The keystrokes, read after the prompt `Enter keystrokes:`

**Output Format:**
Print `Final text:` followed by the final text, or `Final text: EMPTY` if the text is empty.

**Constraints:**
* The line has 1 to 100 characters, using only letters (`a-z`, `A-Z`) and `^`, `<`, with no spaces.
* Letters are case-sensitive.
* `^` may appear any number of times, even several in a row.
* `<` does nothing when the text is empty.
* The final text may be empty.

### Sample Outputs

```
Sample Output 1
Enter keystrokes: ab^cD<e^f
Final text: abCEf

Sample Output 2
Enter keystrokes: Hi^^<<xY
Final text: xY

Sample Output 3
Enter keystrokes: abc<<<<
Final text: EMPTY
```

## ANSWER 3

```python
keystrokes = input("Enter keystrokes: ")
caps_lock = False
result = []

for ch in keystrokes:
    if ch == '^':
        caps_lock = not caps_lock
    elif ch == '<':
        if result:
            result.pop()
    else:
        result.append(ch.swapcase() if caps_lock else ch)

final_text = "".join(result)
print(f"Final text: {final_text if final_text else 'EMPTY'}")
```

---

# QUESTION 4

**Thermostat Switch Count**

**Problem Statement:**<br>
A heater starts off. A thermostat with low limit L and high limit H takes N readings in order. After each reading:
- If the heater is off and the reading is **below L**, it turns on.
- If the heater is on and the reading is **above H**, it turns off.
- Otherwise, nothing changes.

Count the times it turns on, and the readings that leave it on.

For example, take **L = 18, H = 22** and the readings `20 17 19 21 23 20 16 22 18 23`:
- 20: the heater stays off.
- 17: below 18, the heater turns on (turned on: 1, readings with heater on: 1).
- 19: the heater stays on (readings with heater on: 2).
- 21: the heater stays on (readings with heater on: 3).
- 23: above 22, the heater turns off.
- 20: the heater stays off.
- 16: below 18, the heater turns on (turned on: 2, readings with heater on: 4).
- 22: not above 22, the heater stays on (readings with heater on: 5).
- 18: the heater stays on (readings with heater on: 6).
- 23: above 22, the heater turns off.
- So the heater turns on **2** times and **6** readings leave it on.

**Input Format:**
* L and H, after `Enter L and H:`
* N, after `Enter N:`
* The N readings, after `Enter the readings:`

**Output Format:**
Two lines: `Turned on:` and the first count, then `Readings with heater on:` and the second.

**Constraints:**
* -50 < L < H < 50, 1 < N < 50, readings from -100 to 100.
* A reading equal to L or H never switches the heater.
* The heater may never turn on, or stay on to the end.

### Sample Outputs

```
Sample Output 1
Enter L and H: 18 22
Enter N: 10
Enter the readings: 20 17 19 21 23 20 16 22 18 23
Turned on: 2
Readings with heater on: 6
```

## ANSWER 4

```
L, H = map(int, input("Enter L and H: ").split())
N = int(input("Enter N: "))
readings = list(map(int, input("Enter the readings: ").split()))

heater_on = False
turned_on_count = 0
readings_on_count = 0

for temp in readings:
    if not heater_on and temp < L:
        heater_on = True
        turned_on_count += 1
    elif heater_on and temp > H:
        heater_on = False
    
    if heater_on:
        readings_on_count += 1

print(f"Turned on: {turned_on_count}")
print(f"Readings with heater on: {readings_on_count}")
```

---

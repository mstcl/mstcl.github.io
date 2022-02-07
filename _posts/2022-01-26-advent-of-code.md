---
layout: post
title: "Advent of Code 2021 'short' write-up (ongoing)"
date: 2022-01-26 23:05:00 0000
tag:
  - python
  - programming
---

## Before I start…

This will be a long one, I don't know why I feel like doing this, but I think
it'll help me consolidate a few things I've learned. I
hope the reader will bear with my stupidity as we go along. Not sure how
educational it is for anyone but me, I'm self-documenting this for my own learning.

As of writing this, I have done 16 days now, with the second part of day 16
needing a bit of polishing. The solutions can be found in this
[repo](https://github.com/mstcl/AoC21).

Anyhow, here's the plan, I'll be looking back through the code for each day and
explain what the hell I wrote. I started this before the event was done, I
think it was on day 18 or 19, but obviously I did it at my own pace and in my
own time, _sans_ time pressure.

## Day 1

### Part 1

Quite straight forward. I did this in O(N) time and O(1) space complexity.

```python
with open("day1-input", "r") as puzzle_input:
    previous, COUNTER = int(next(puzzle_input).strip("\n")), 0
    for line in puzzle_input:
        if int(line.strip("\n")) > previous:
            COUNTER += 1
        previous = int(line.strip("\n"))
print(COUNTER)
```

### Part 2

This time, we want several elements of the list, but we're really only
interested in the sum of that list. So the structure is the same as Part 1.

```python
with open("day1-input", "r") as puzzle_input:
    depths = [int(depth) for depth in puzzle_input.readlines()]
    previous, COUNTER = int(sum(depths[0:3])), 0
    for value in range(len(depths)):
        if int(sum(depths[value : value + 3])) > previous:
            COUNTER += 1
        previous = int(sum(depths[value : value + 3]))
print(COUNTER)
```

## Day 2

### Part 1

One variable to keep track of the current location, and updating it for each
line of instruction. This utilises basic `if else` statements. This is O(N)
time and O(1) space.

```python
coordinate = [0, 0]
with open("day2-input", "r") as commands:
    for line in commands.readlines():
        if line.split()[0] == "forward":
            coordinate[0] += int(line.split()[1])
        elif line.split()[0] == "down":
            coordinate[1] += int(line.split()[1])
        else:
            coordinate[1] -= int(line.split()[1])
print(coordinate[0] * coordinate[1])
```

### Part 2

We were only asked to add or modify a few things for our `if else` statements.
There is a third coordinate as well, so we need to modify our current location
list to take an extra element.

```python
coordinate = [0, 0, 0]
with open("day2-input", "r") as commands:
    for line in commands.readlines():
        if line.split()[0] == "forward":
            coordinate[0] += int(line.split()[1])
            coordinate[1] += coordinate[2] * int(line.split()[1])
        elif line.split()[0] == "down":
            coordinate[2] += int(line.split()[1])
        else:
            coordinate[2] -= int(line.split()[1])
print(coordinate[0] * coordinate[1])
```

## Day 3

### Part 1

It makes sense to transpose the puzzle input after making them into a 2D list
since we are looking at the columns. We can use `numpy` for this. Then we can
perform row operations. Then, I used the `Counter` object and its built-in
function `most_common()` to extract the most and least common elements for each
row, and append it to a list, either `gamma` or `epsilon`. List comprehension
can do all this in two lines. After this, I converted back into decimal.

```python
from collections import Counter
import numpy as np

with open("day3-input", "r") as diagnostics:
    table = np.array(
        [list(line.strip("\n")) for line in diagnostics.readlines()]
    ).T.tolist()
gamma = [Counter(bit).most_common()[0][0] for bit in table]
epsilon = [Counter(bit).most_common()[-1][0] for bit in table]
print(int("".join(gamma), 2) * int("".join(epsilon), 2))
```

### Part 2

This is considerably more complicated. But we already have the skeleton code.
We want to transpose the puzzle input again, as we are operating on columns,
but be careful, we need to eliminate the column and not the row.

First, we see that we can reduce this problem down to many sub-problems, for
each sub problem, we are incrementing the rows, eliminating columns that do not
fit the criteria, and then recursively run this again. Again, we can use a
`Counter` object to tally up the most common bit, and `numpy` to transpose the
input.

The criteria, if you've read the problem, requires us to consider either `1`
for oxygen rating or `0` for C02 rating if there are equal values of each. We
can handle this by allowing our function to take an integer, either `0` for
oxygen or `1` for C02. This way, we know which value our target bit is by
taking 1 away from the criteria.

Eliminating columns that do not match can also be done with a list
comprehension. Essentially we append all the 1s and 0s in that number which is
split into a list, if the first value matches the target.

After elimination is recursion, however, our base case to return is when the
length of the table is 1 (the value we want), otherwise, repeat this, but for
the next row.

We perform this recursive function twice, once for the oxygen rating and once
for the C02 rating. Convert each returned result into binary and that's the
answer.

```python
from collections import Counter
import numpy as np

def get_rating(rating_table, criteria, bit_max_position):
    """calculate final rating and return final binary"""

    # Transpose table and find our target bit
    target_bit = np.array(rating_table).T.tolist()[-bit_max_position]

    # Non-equal number of 1s and 0s
    if "".join(target_bit).count("1") != "".join(target_bit).count("0"):
        target = Counter(target_bit).most_common()[criteria][0]

    # Equal number of 1s and 0s
    else:
        target = str(1 - criteria)

    # Eliminate columns that don't start with target bit
    rating_table = [
        sub_bit for sub_bit in rating_table if sub_bit[-bit_max_position] == target
    ]

    # Return the binary number if only one binary number remains
    # else call the function again
    return (
        rating_table
        if len(rating_table) == 1
        else get_rating(rating_table, criteria, bit_max_position - 1)
    )

with open("day3-input", "r") as diagnostics:
    table = [list(line.strip("\n")) for line in diagnostics.readlines()]

OXYGEN_RATING = get_rating(table, 0, len(table[0]))
CO2_RATING = get_rating(table, 1, len(table[0]))
print(int("".join(OXYGEN_RATING[0]), 2) * int("".join(CO2_RATING[0]), 2))
```

## Day 4

### Part 1

My implementation for this day was a bit lengthy, but it worked fine. We were
asked to do some convoluted stuff for the final score, but what is most
important is the output of our functions. We want the very last winning number,
and the board that won, with all the unmarked (numbers not part of the winning
column or row) numbers. The structure of the board is not important, only the
sum of those unmarked numbers matters.

With what we want at the end out of the way, we first need to parse the numbers
that get called out, our `draws`. Then we need to parse the bingo boards with
list comprehension or otherwise. These are our constants.

Then I declared two lists, one called `board` that will become each board as we
iterate through all the boards, and one which contains information about the
winning board, called `top_score`. Not only do we need the winning score of
each board, provided that it wins (otherwise returns something negative or
`None`), we need to compare all the boards to see what is our winning board.
Thus, the first index contains the score of boards that win, and the second
contains the number of steps it took to win.

Our driver code then iterates through all the boards and each time it call a
function `play_bingo`. We have to pass our board and the draws into this
function. Here, we declare `steps = 0` to keep count of the number of steps
we're taking, since we have to return this later. It then iterates through each
value of `draws`, and then for each row in our board, if the number exists, we
set that value to -1. If you thought of doing this the other way around; if we
iterate through the board and check if each value is in the draws instead, this
ruins our order and order matters because that's literally the game of bingo.
Anyhow, we exit the loop that goes through each row of the board, and increment
`steps` by 1. Then pass our board into another function, `is_bingo`. As the
name reveals, it checks if we have a winning board. This serves to halt the
computation early if the board won so that we don't have waste precious time
and resources.

The function `is_bingo` is simple, since we set values that are part of `draws`
to -1, we can sum each row, if it's -5, then return `True`. We then transpose
the board and perform the same check, and return `True` if it's -5. Otherwise,
it will return `False`.

Back to `play_bingo`. If `is_bingo` returns `True`, then we now compute score.
Since we are still in the outermost loop which iterates through the values in
`draws`, our current iterating variable is the number. We can do this with list
comprehension, and return our score and `steps` in a list. If a board does not
win, which is our worst case scenario computationally, we simply return `[-1,
steps]` for the score.

Back in our driver code, we are still in a loop going through each board. Every
time `play_bingo` returns a list and that board does win, we compare the number
of steps (index 1 of our returned value) to `top_score[1]`. If the board's
score is larger, we replace it, if not, we don't bother.

After going through all the boards, we get our result.

```python
import numpy as np

def is_bingo(board_bingo):
    """check bingo"""
    for row in board_bingo:
        if sum(row) == -5:
            return True
    for row in np.array(board_bingo).T.tolist():
        if sum(row) == -5:
            return True
    return False


def play_bingo(board_play, draws_play):
    """play bingo"""
    steps = 0
    for number in draws_play:
        for row_num, row in enumerate(board_play):
            if number in row:
                board_play[row_num][row.index(number)] = -1
        steps += 1
        if is_bingo(board_play):
            return [
                number
                * sum(
                    element for line in board_play for element in line if element != -1
                ),
                steps,
            ]
    return [-1, steps]


with open("day4-input", "r") as puzzle_input:
    draws = list(map(int, list(next(puzzle_input).split(","))))
    board_parser = [
        line.strip("\n").split()
        for line in list(puzzle_input.readlines()[1:])
        if len(line.strip("\n").split()) != 0
    ]

board, top_score = [], [0, 1000000]
for i in range(0, len(board_parser), 5):
    board = [list(map(int, line)) for line in board_parser[i : i + 5]]
    result = play_bingo(board, draws)
    if result is not None and result[1] < top_score[1]:
        top_score = result

print(top_score[0])
```

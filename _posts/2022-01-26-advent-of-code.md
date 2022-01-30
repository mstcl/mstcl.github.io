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
own time, sans time pressure.

## Day 1

### Part 1

Quite straight forward. I did this in O(N) time and O(1) space complexity.

```python
COUNTER = 0
for line in puzzle_input:
    if int(line.strip("\n")) > previous:
        COUNTER += 1
    previous = int(line.strip("\n"))
```

### Part 2

This time, we want several elements of the list, but we're really only
interested in the sum of that list. So the structure is the same as Part 1.

```python
previous, COUNTER = int(sum(depths[0:3])), 0
for value in range(len(depths)):
    if int(sum(depths[value : value + 3])) > previous:
        COUNTER += 1
    previous = int(sum(depths[value : value + 3]))
```

## Day 2

### Part 1

One variable to keep track of the current location, and updating it for each
line of instruction. This utilises basic `if else` statements. This is O(N)
time and O(1) space.

```python
coordinate = [0, 0]
for line in commands.readlines()
    if line.split()[0] == "forward":
        coordinate[0] += int(line.split()[1])
    elif line.split()[0] == "down":
        coordinate[1] += int(line.split()[1])
    else:
        coordinate[1] -= int(line.split()[1])
```

### Part 2

We were only asked to add or modify a few things for our `if else` statements.
There is a third coordinate as well, so we need to modify our current location
list to take an extra element.

```python
coordinate = [0, 0, 0]
for line in commands.readlines():
    if line.split()[0] == "forward":
        coordinate[0] += int(line.split()[1])
        coordinate[1] += coordinate[2] * int(line.split()[1])
    elif line.split()[0] == "down":
        coordinate[2] += int(line.split()[1])
    else:
        coordinate[2] -= int(line.split()[1])
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
answer. Here's the main function.

```python
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
```

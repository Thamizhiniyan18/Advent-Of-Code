---
description: Advent of Code 2023 Day 2  solution by Thamizhiniyan C S
---

# Day 2

{% embed url="https://adventofcode.com/2023/day/2" %}

## Part One

You play several games and record the information from each game (your puzzle input). Each game is listed with its ID number (like the `11` in `Game 11: ...`) followed by a semicolon-separated list of subsets of cubes that were revealed from the bag (like `3 red, 5 green, 4 blue`).

For example, the record of a few games might look like this:

```
Game 1: 3 blue, 4 red; 1 red, 2 green, 6 blue; 2 green
Game 2: 1 blue, 2 green; 3 green, 4 blue, 1 red; 1 green, 1 blue
Game 3: 8 green, 6 blue, 20 red; 5 blue, 4 red, 13 green; 5 green, 1 red
Game 4: 1 green, 3 red, 6 blue; 3 green, 6 red; 3 green, 15 blue, 14 red
Game 5: 6 red, 1 blue, 3 green; 2 blue, 1 red, 2 green
```

In game 1, three sets of cubes are revealed from the bag (and then put back again). The first set is 3 blue cubes and 4 red cubes; the second set is 1 red cube, 2 green cubes, and 6 blue cubes; the third set is only 2 green cubes.

The Elf would first like to know which games would have been possible if the bag contained _only 12 red cubes, 13 green cubes, and 14 blue cubes_?

In the example above, games 1, 2, and 5 would have been _possible_ if the bag had been loaded with that configuration. However, game 3 would have been _impossible_ because at one point the Elf showed you 20 red cubes at once; similarly, game 4 would also have been _impossible_ because the Elf showed you 15 blue cubes at once. If you add up the IDs of the games that would have been possible, you get _`8`_.

Determine which games would have been possible if the bag had been loaded with only 12 red cubes, 13 green cubes, and 14 blue cubes. _What is the sum of the IDs of those games?_

{% code lineNumbers="true" %}
```python
import re

red = 12
green = 13
blue = 14

valid_games = []

for line in open("../test.txt", "r"):
    game_id = re.findall(r'Game\s(\d+):', line)
    is_red_valid = False
    is_green_valid = False
    is_blue_valid = False

    red_occurrences = re.findall(r'(\d+)\sred', line)
    for each in red_occurrences:
        if int(each) <= red:
            is_red_valid = True
        else:
            is_red_valid = False
            break

    green_occurrences = re.findall(r'(\d+)\sgreen', line)
    for each in green_occurrences:
        if int(each) <= green:
            is_green_valid = True
        else:
            is_green_valid = False
            break

    blue_occurrences = re.findall(r'(\d+)\sblue', line)
    for each in blue_occurrences:
        if int(each) <= blue:
            is_blue_valid = True
        else:
            is_blue_valid = False
            break

    if is_red_valid and is_green_valid and is_blue_valid:
        valid_games.append(int(game_id[0]))

print(sum(valid_games))
```
{% endcode %}

***

## Part Two

As you continue your walk, the Elf poses a second question: in each game you played, what is the _fewest number of cubes of each color_ that could have been in the bag to make the game possible?

Again consider the example games from earlier:

```
Game 1: 3 blue, 4 red; 1 red, 2 green, 6 blue; 2 green
Game 2: 1 blue, 2 green; 3 green, 4 blue, 1 red; 1 green, 1 blue
Game 3: 8 green, 6 blue, 20 red; 5 blue, 4 red, 13 green; 5 green, 1 red
Game 4: 1 green, 3 red, 6 blue; 3 green, 6 red; 3 green, 15 blue, 14 red
Game 5: 6 red, 1 blue, 3 green; 2 blue, 1 red, 2 green
```

* In game 1, the game could have been played with as few as 4 red, 2 green, and 6 blue cubes. If any color had even one fewer cube, the game would have been impossible.
* Game 2 could have been played with a minimum of 1 red, 3 green, and 4 blue cubes.
* Game 3 must have been played with at least 20 red, 13 green, and 6 blue cubes.
* Game 4 required at least 14 red, 3 green, and 15 blue cubes.
* Game 5 needed no fewer than 6 red, 3 green, and 2 blue cubes in the bag.

The _power_ of a set of cubes is equal to the numbers of red, green, and blue cubes multiplied together. The power of the minimum set of cubes in game 1 is `48`. In games 2-5 it was `12`, `1560`, `630`, and `36`, respectively. Adding up these five powers produces the sum _`2286`_.

For each game, find the minimum set of cubes that must have been present. _What is the sum of the power of these sets?_

{% code lineNumbers="true" %}
```python
import re

powers = []

for line in open("../test.txt", "r"):
    red_occurrences = sorted(list(map(int, re.findall(r'(\d+)\sred', line))))

    green_occurrences = sorted(list(map(int, re.findall(r'(\d+)\sgreen', line))))

    blue_occurrences = sorted(list(map(int, re.findall(r'(\d+)\sblue', line))))

    powers.append(red_occurrences[-1] * green_occurrences[-1] * blue_occurrences[-1])

print(sum(powers))
```
{% endcode %}

---
description: Advent of Code 2023 Day 1  solution by Thamizhiniyan C S
---

# Day 1

{% embed url="https://adventofcode.com/2023/day/1" %}

## Part One

The newly-improved calibration document consists of lines of text; each line originally contained a specific _calibration value_ that the Elves now need to recover. On each line, the calibration value can be found by combining the _first digit_ and the _last digit_ (in that order) to form a single _two-digit number_.

For example:

```
1abc2
pqr3stu8vwx
a1b2c3d4e5f
treb7uchet
```

In this example, the calibration values of these four lines are `12`, `38`, `15`, and `77`. Adding these together produces _`142`_.

Consider your entire calibration document. _What is the sum of all of the calibration values?_

{% code lineNumbers="true" %}
```python
import re

total = 0

for line in open("../test.txt", "r"):
    numbers = re.findall(r'(\d)', line)
    if len(numbers) > 1:
        total += int(numbers[0] + numbers[-1])
    else:
        total += int(numbers[0] * 2)

print(total)
```
{% endcode %}

***

## Part Two

Your calculation isn't quite right. It looks like some of the digits are actually _spelled out with letters_: `one`, `two`, `three`, `four`, `five`, `six`, `seven`, `eight`, and `nine` _also_ count as valid "digits".

Equipped with this new information, you now need to find the real first and last digit on each line. For example:

```
two1nine
eightwothree
abcone2threexyz
xtwone3four
4nineeightseven2
zoneight234
7pqrstsixteen
```

In this example, the calibration values are `29`, `83`, `13`, `24`, `42`, `14`, and `76`. Adding these together produces _`281`_.

_What is the sum of all of the calibration values?_

{% code lineNumbers="true" %}
```python
import re

total = 0

arr = ["one", "two", "three", "four", "five", "six", "seven", "eight", "nine"]

combos = "|".join([i + j[1:] for i in arr for j in arr if j.startswith(i[-1])])

numbers_dict = {
    "one": "1",
    "two": "2",
    "three": "3",
    "four": "4",
    "five": "5",
    "six": "6",
    "seven": "7",
    "eight": "8",
    "nine": "9",
    "oneight": "18",
    "twone": "21",
    "threeight": "38",
    "fiveight": "58",
    "sevenine": "79",
    "eightwo": "82",
    "eighthree": "83",
    "nineight": "98",
}

for line in open("../test.txt", "r"):
    matches = re.findall(rf'({combos}|one|two|three|four|five|six|seven|eight|nine|\d)', line)

    numbers = "".join([numbers_dict[each] if each in numbers_dict.keys() else each for each in matches])

    if len(numbers) > 1:
        total += int(numbers[0] + numbers[-1])
    else:
        total += int(numbers[0] * 2)

print(total)
```
{% endcode %}

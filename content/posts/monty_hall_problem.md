---
title: "Monty Hall Problem"
date: 2020-03-08T13:20:11+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "curiosity-jar"]
---

[Source Code](https://github.com/nat236919/ThinkTank/tree/master/MontyHallProblem)

## What is Monty Hall Problem

**The Monty Hall Problem** is named for its similarity to the Let's Make a Deal television game show hosted by Monty Hall. The problem is stated as follows. Assume that a room is equipped with three doors. Behind two are goats, and behind the third is a shiny new car. You are asked to pick a door, and will win whatever is behind it. Let's say you pick door 1. Before the door is opened, however, someone who knows what's behind the doors (Monty Hall) opens one of the other two doors, revealing a goat, and asks you if you wish to change your selection to the third door (i.e., the door which neither you picked nor he opened). The Monty Hall problem is deciding whether you do.

The correct answer is that you do want to switch. If you do not switch, you have the expected 1/3 chance of winning the car, since no matter whether you initially picked the correct door, Monty will show you a door with a goat. But after Monty has eliminated one of the doors for you, you obviously do not improve your chances of winning to better than 1/3 by sticking with your original choice. If you now switch doors, however, there is a 2/3 chance you will win the car (counterintuitive though it seems).

## Experimental Procedures

1. Create a game engine (game_engine.py)

```python
"""
FILE: game_engine.py
DESCRIPTION: The file contains the Monty Hall game mechanism
AUTHOR: Nuttaphat Arunoprayoch
DATE: 23-Feb-2020
"""
# Import libraries
import random
import numpy as np
from typing import List, Dict, Any


# Monty Hall mechanism
class MontyHall():
    """ Monty Hall Game Mechanism
        1. There are 3 doors to be selected, and one of the doors contains a reward. The rest is empty
        2. Choose 1 door of the three doors
        3. Reveal ONLY 1 empty door: (1 opened, 2 left unopened)
        4. Give a choice to a player whether to swtich or not
        5. Open all the doors
        6. Summarise the result (reward found = 1, empty = 0)
    """
    def play(self, automatic_swap: bool = False) -> bool:
        """ Play a game with an option to swap automatically """
        self.doors = list(range(3)) # doors: [0] [1] [2]
        self.award_door = random.choice(self.doors)
        user_selection = random.choice(self.doors)

        # Determine the result (Swap)
        if automatic_swap:
            if user_selection != self.award_door:
                return True
            else:
                return False
        
        # Determine the result (Stay)
        result =  bool(user_selection == self.award_door)

        return result
```

2. Create main function (main.py)

```python
"""
PROJECT: Monty Hall Problem
DESCRIPTION: The experiment to prove Monty Hall Problem probability
AUTHOR: Nuttaphat Arunoprayoch
DATE: 23-Feb-2020
"""
# Import libraries
import random
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from typing import List
from game_engine import MontyHall


# Main function
def main() -> None:
    # Inititate the experiment
    rounds = 1000
    monty_hall = MontyHall()
    swap_result = [monty_hall.play(automatic_swap=True) for _ in range(rounds)]
    stay_result = [monty_hall.play() for _ in range(rounds)]
    stats_result = lambda results: len([result for result in results if result]) / len(results)

    # Show result
    print('--- Monty Hall Problem ---')
    print(f'Plays: {rounds} rounds')
    print(f'Swap: Chance of winning {stats_result(swap_result)}')
    print(f'Stay: Chance of winning {stats_result(stay_result)}')

    # Visualization
    df = pd.DataFrame({'swap': stats_result(swap_result),
                        'stay': stats_result(stay_result)}, index=['Result'])
    df.plot.bar()
    plt.show()

    return None


if __name__ == '__main__':
    main()
```

3. Run the code to start an experiment

```console
python main.py
```

4. Result

```console
--- Monty Hall Problem ---
Plays: 1000 rounds
Swap: Chance of winning 0.669
Stay: Chance of winning 0.324
```

{{< image src="/img/blog/monty_hall_problem/fig_1.png" alt="figure 1" position="center">}}

## Remarks

There are 2 goats behind the doors, so it's intuitive that you will first SELECT a goat instead of a car. Therefore,
2/3 chance of winning if you switch, and only 1/3 chance of winning if you stay.

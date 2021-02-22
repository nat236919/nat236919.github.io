---
title: "Unconventional Russian Roulette"
date: 2020-11-27T12:57:57+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "curiosity-jar"]
---

[Source Code](https://github.com/nat236919/curiosity-jar/tree/master/UnconventionalRussianRoulette)

___

## What is Unconventional Russian Roulette?

**Unconventional Russian Roulette** introduces similarities as its origin [**Russian Roulette**](https://en.wikipedia.org/wiki/Russian_roulette). The difference is that instead of using one bullet, it uses **three bullets** where they are loaded *adjacently*.

Let's imagine this scenario:

Three bullets are loaded into the cylinder adjacently, **Player A** has taken a shot and survived. We, as **Player B**, need to decide whether to take a shot or not. For the sake of simplicity, the game ends this round and here are the conditions:

* If we took a shot, and it was empty -> **WE WON 1 MILLION DOLLARS**
* If we chose to stay, and it was loaded -> **WE WON 1 MILLION DOLLARS**

> **Q:** If the main goal here is to be alive and to take **1 MILLION DOLLARS** back home. Should Player B take a shot or not? and Why?

Indeed, there is a way to increase our chances of going back home alive with money.

___

## The Winning Strategy

A cylinder consists of 6 rounds: 3 loaded, 3 empty. That's *3/6* and *3/6* or *50%* equal chance of surviving. However, the game begins the second round for us (as **Player B**). After the first round has been played out and **Player A** has survived, the chance of winning is dimming since an empty round has been shot; that leaves us *3/5* or *60%* of catching a bullet.

{{< image src="https://i.ibb.co/9rc7QfJ/3a.png" alt="loaded cylinder" position="center">}}

A right-minded person would choose to **pass** since there's a *60%* chance of bullet existing in this round. However, this is not the case. By having three adjacent bullets loaded gives us a huge advantage of earning that lovely **1 million dollars** and going home safely by taking a shot.

Considering this, **Player A** has survived the first round which means an empty round was fired; furthermore, the bullets are loaded *adjacently*. Therefore, there's *2/3* chance that the cylinder is currently empty. The figure below depicted the aforementioned scenario in which the cylinder rotates in clockwise.

{{< image src="https://i.ibb.co/HhZMY4Q/urr-fig-2.png" alt="loaded cylinder" position="center">}}

* Green stars represent the possible first round shots

We can see that *2/3* will be empty and *1/3* will be a bullet. So, if we had to be choose between **taking a shot** or **passing** to get **1 MILLION DOLLARS**, we'd have a better chance with **taking a shot**.

___

## Experimental Procedures

Let's create a simple script to simulate the game for us to prove the concept.

* Create a GameEngine to simulate the game

```python
# Import libs
import random
from typing import List, Dict, Any


# Core Game Engine
class GameEngine:
    """ Unlike its original rules, Unconventional Russian Roulett puts 2 bullets
        in the cylinder (n=6) **adjacently**. The game proceeds as usual in which the cylinder is initially spun,
        and each player takes turn to pull the trigger. The survival wins, obviously.
        Assume, we were "Player B" starting second round. After the first round had been played by Player A, and he/she survived.
        Now it is out turn to play; however, you were given a choice to take a shot or to stay. Here are rules
            - If we took a shot, and it was empty -> WE WON 1 MILIION DOLLARS
            - If we chose to stay, and it was loaded -> WE WON 1 MILIION DOLLARS
            
        The question raised here is that
            - Should we take a shot or not? and why?
            - Assuming that our main goal was to get 1 million dollars at all costs
    """

    def __init__(self) -> None:
        """ Generate cylinders (6 rounds)
            0   -> Empty (n=3)
            1   -> Loaded (n=3)
        """
        self._load_cylinder()
    
    def _load_cylinder(self) -> None:
        self.loaded_cylinder = [1, 1, 1] + [0 for _ in range(3)] # 3 loaded; 3 empty
    
    def _spin_cylinder(self) -> None:
        """ Simulate spinning cylinder """
        random_spin_num = random.randrange(1, 7) # random 1 - 6
        self.loaded_cylinder = self.loaded_cylinder[random_spin_num:] + self.loaded_cylinder[:random_spin_num]
    
    def _play(self, take_a_shot: bool = False) -> bool:
        """ Play the game
            1.) Player A will guarantee to survive the first round
            2.) Player B will need to choose to whether take a shoot or stay
            3.) The game will stop at the Player B's first round for simplicity
        """
        # Keep spinning to make sure the first round is not loaded
        self._load_cylinder()
        while self.loaded_cylinder[0] == 1:
            self._spin_cylinder()
    
        # Skip the first round since it is guaranteed to be empty for Player A
        has_player_won_million = False
        for bullet in self.loaded_cylinder[1:]:
            if take_a_shot and bullet == 0:
                has_player_won_million = True
            elif not take_a_shot and bullet == 1:
                has_player_won_million = True
            break
        return has_player_won_million

    def start_experiment(self, n: int = 1000) -> Dict[str, List[bool]]:
        experimental_results = {'game_num': n, 'take_a_shot_won': [], 'pass_won': []}
        if not isinstance(n, int):
            return experimental_results

        experimental_results['take_a_shot_won'] = sum([self._play(take_a_shot=True) for _ in range(n)])
        experimental_results['pass_won'] = sum([self._play(take_a_shot=False) for _ in range(n)])
        return experimental_results
```

* Create a main function to play multiple games (*n=1000*) and visualize the result

```python
# Import libraries
import matplotlib.pyplot as plt
import pandas as pd
import plotly.express as px

from game_engine import GameEngine

# Load Core GameEngine
game_engine = GameEngine()


# Main Function
def main() -> None:
    # Experimental Results
    experimental_res = game_engine.start_experiment(n=1000)

    # Prepare DataFrame
    df = pd.DataFrame({}, columns=['cat', 'num_of_winning'])
    df = df.append({'cat': 'take_a_shot_won', 'num_of_winning': experimental_res.get('take_a_shot_won')}, ignore_index=True)
    df = df.append({'cat': 'pass_won', 'num_of_winning': experimental_res.get('pass_won')}, ignore_index=True)
    print(df)

    # Visualisation
    fig = px.pie(df, values='num_of_winning', names='cat', hole=.3)
    fig.show()

    return None


if __name__ == '__main__':
    main()
```

___

## Results

By experimenting 1000 games with a **taking-a-shot** method and a **passing** method, the results showed that **taking-a-shot** won 658 games; on the other hand, **passing** only won 312 games. The results matched our assumption.

```console
               cat num_of_winning
0  take_a_shot_won            658
1         pass_won            312
```

{{< image src="https://i.ibb.co/S3Fd8rh/newplot.png" alt="loaded cylinder" position="center">}}

___

## Summary

**Unconventional Russian Roulette** offers something fresh and interesting to explore. We can always add more peculiar rules to make things complicated, yet insert hidden solutions to take advantages.

Finally, I do hope that this post sparks your interest in programming and data exploration. Cheers.

___

### References

* [Russian Roulette](https://en.wikipedia.org/wiki/Russian_roulette)
* [Liar Game - Revival Round II](https://liarsgame.fandom.com/wiki/Revival_Round_II)

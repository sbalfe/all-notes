# Dice game

- Bottom of page $\to$ some code to check the **submission requirements**

---

- Writing a **agent** to play some simple dice game
  - Start with **0 points**
  - *Roll* with a **fair 6 sided dice**
  - SELECT $\to$ **ONE** OF THE FOLLOWING:
    1. **STICK** - To **accept** the **values** shown
       - If there are **2 or more** dice that **show** the **same values** $\to$ then **all 3 dice** are *flipped* opposite sides
         - 1 $\to$ 6, etc.
       - Total $\to$ added to the **points** and **this** is the **final score**
    2. **Reroll** - Choose to **hold** a **combination** of the **dice** on the **current value shown**
       - COST = **1 POINT** 
         - Potential for a **negative score**
       - This choice can *be repeated*

- The **HIGHEST POSSIBLE SCORE** = 18
  - Roll **three 1's** on the **first roll**. 

---

- The **Reroll** is to **prevent** from **reaching 18** 
  - If the **value** of the **current dice** *is greater* than the **expected value** of **rerolling** $\to$ taking into consideration the **penalty**
    - Must **STICK** preferably.

---

- The **optimal decision** $\to$ is *independent* on your **current score**
  - does **not matter** if its **your first roll** on score $0$ or **twentieth** on score $-19$ 
    - This means **a positive end score** is *not possible*
  - Either case $\to$ **rolling three 6's** $\to$ *sticking* $\to$ adds to $3$ only 
    - End up getting a **better score** via the **reroll** and **taking a penalty.**
      - nearly any score beats therefore its the choice of **maximal score**

---

- **OBVIOUS** $\to$ *sticking* on *three ones* and **REROLL** on three 6's
  - Should you **hold** any of the **6's** when you **reroll**.

---

- Do **not know** what numbers may **come up** when you **roll** 
  - we do **know** the **probability** of **a given roll** exactly. 
    - This is the purpose of **probabilistic reasoning** in this unit. 
- Model **true probabilities** $\to$ calculate an **optimal policy**

---

## Submission requirements

- Code supports **playing the game** with **modifications**

  - Change **dice number**
  - Change **dice values**
  - make **biased dice** $\to$ more likely to **roll certain values**

- Submit

  - The **implementation** for some *agent* which **can play the game**
    - Code subject to **automated testing** $\to$ grades are assigned on **how well it plays**
    - The **code speed** determines **higher grade**. 
    - Sample tests and **skeleton code**

  - TXT file to **explain** the **approach taken** and the decisions made in **your own words** - readMe file.
  - Marks are awarded for good **academic presentation**

---

### Choice of algorithm

- Example agents provided which **dont pass** $\to$ use as structure

---

- The concept of **value iteration** was covered in the unit
  - Use this technique here **to produce a strategy** for this game $\to$ the **agent** then simply follows
    - I consider the **ideal parameters** in order to **suit** this value iteration algorithm to **maximise the score**

---

- Consider the efficiency aspect of the algorithm

---

- Agent should be able to deal with **modified versions of the game**. 

---

## Dice Game Class

- A class **called** *DiceGame* is within *dice_game.py*  to **use** in the **solution**
  - When you **create** some **DiceGame** object $\to$ default obtain the **rules** as stated above.
- Creates a game with **3 normal 6 sided dice**
  - Can modify the game mechanics:

```python
game = DiceGame(dice = 4, sides = 3, values = [1,2,6], bias = [0.1, 0.1, 0.8], penalty = 2)
```

- Creates the game where you **roll 4 dice** 
  - Each with **3 sides**
  - Values [1,2,6] and bias with the **altered weights**.
  - penalty now 2
- Games with **unusual values** and **sides** 
  - When there are **duplicates**
    - value[i] $\to$ value[-i]
  - With **odd sided dice** $\to$ the middle value **flips** onto **oneself.**

- When created
  - The **DiceGame** object is **run** within **two different modes**
    - Simulation
    - Analysis.
      - Use this mode for the agent. 

---

### Simulation

- Object provides **methods** required to **simulate** playing some game.
  - Used in **testing** or **playing the game yourself**.

- Current dice obtain from **get_dice_state()** $\to$ listed in **ascending order**

---

- To roll $\to$  call the **roll method** taking **one parameter** $\to$ the **tuple** representing **which dice to hold** 

  - Numbered from zero.

- We **rolled** a 2,3,4 $\to$ say we want to **hold 2** $\to$ pass the tuple $(0,)$ into the **roll** method.

  - Add comma to make sure python knows its a tuple.

- **roll** method returns a **tuple containing**

  - the **reward** of this action

  - The **new state**
  - Whether the game is over

```python
reward, new_state, game_over = game.roll((0,))
print(reward) // -1
print(new_state) // (2,2,5)
print(game_over) // False
```



- If you are happy and want to **stick** and obtain final score $\to$ roll and supply the **tuple** of **all three dice**

```python
reward, new_state, game_over = game.roll((0, 1, 2))
print(reward) // True
print(new_state) // (5,5,5)
print(game_over) // True
```

- The **return** is just the **reward** for **said action**
  - In this case is **obtaining 15**
- Final score $\to$ game.score. 
  - Rerolled once therefore obtain a score of 14.

- Play again $\to$ game.reset

---

### Analysis Mode

- You are **not playing**
  - Ask for **all possible outcomes of certain actions**
- Know that **all the possible states** $\to$ and **all possible actions** are **stored inside the object**
  - Change the **game mechanics** on the first line $\to$ add **dice = 2** to the **constructor** $\to$ see how **other information updated**

```python
game = DiceGame()
print(f"The first 5 of {len(game.states)} possible dice rolls are: {game.states[0:5]}")
print(f"The possible actions on any given turn are: {game.actions}")
```

- first **5** of **56 possible dice rolls**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210214210158876.png)

- Possible actions any turn:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210214210215273.png)

---

- **get_next_states(action, dice_state)** 
  - Enables us to **obtain** the **possible resulting states** for **some given state** and **action**
- Earlier $\to$ roll of 2,3,4 $\to$ decided to **hold the 2**
  - Game can **calculate** the **possible outcomes** for us.
    - Gives us the **probability** of **each state occurring**

```python
game = DiceGame()
states, game_over, reward, probabilities = game.get_next_states((0,), (2, 3, 4))
for state, probability in zip(states, probabilities):
    print(f"Would get roll of {state} with probability {probability}")
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210214210500297.png)

- The method **works consistently** when all the dice are held
  - reporting this action $\to$ causes the **game** to **be over** and **giving the reward**
    - The *list* of *states* returned contains **None**
      - This is the **terminals state** $\to$ **no further actions** would be allowed
  - Game does **not return final dice here** $\to$ as its **another valid state** $\to$ from which there are **still actions available**

---

- Reward $\to$ value is **not the same** as the **final score** of **any given game** 
  - it **does not include** the **previous penalties** of rolling.

```python
states, game_over, reward, probabilities = game.get_next_states((0, 1, 2), (2, 2, 5))
print(states)
print(game_over)
print(reward)
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210214211047020.png)

---

#### Part one - Example agents

- There are 2 where **they dont play well**
  1. Always holds immediately.
  2. Other keeps rolling till **they obtain the best possible dice**.
     - Ignoring the **massive penalty** from re-rolling.
- Neither are considering the **probabilities** are involved in the game.

- Function that runs the game with some given instance of a game agent

```python
from abc import ABC, abstractmethod


class DiceGameAgent(ABC):
    def __init__(self, game):
        self.game = game
    
    @abstractmethod
    def play(self, state):
        pass


class AlwaysHoldAgent(DiceGameAgent):
    def play(self, state):
        return (0, 1, 2)


class PerfectionistAgent(DiceGameAgent):
    def play(self, state):
        if state == (1, 1, 1) or state == (1, 1, 6):
            return (0, 1, 2)
        else:
            return ()
        
        
def play_game_with_agent(agent, game, verbose=False):
    state = game.reset()
    
    if(verbose): print(f"Testing agent: \n\t{type(agent).__name__}")
    if(verbose): print(f"Starting dice: \n\t{state}\n")
    
    game_over = False
    actions = 0
    while not game_over:
        action = agent.play(state)
        actions += 1
        
        if(verbose): print(f"Action {actions}: \t{action}")
        _, state, game_over = game.roll(action)
        if(verbose and not game_over): print(f"Dice: \t\t{state}")

    if(verbose): print(f"\nFinal dice: {state}, score: {game.score}")
        
    return game.score


if __name__ == "__main__":
    # random seed makes the results deterministic
    # change the number to see different results
    # or delete the line to make it change each time it is run
    np.random.seed(1)
    
    game = DiceGame()
    
    agent1 = AlwaysHoldAgent(game)
    play_game_with_agent(agent1, game, verbose=True)
    
    print("\n")
    
    agent2 = PerfectionistAgent(game)
    play_game_with_agent(agent2, game, verbose=True)
```

<img src="https://mathworld.wolfram.com/images/eps-gif/Die_1000.gif" alt="Image result for dice opposite sides" style="zoom:200%;" />
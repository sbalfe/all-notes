# The python data model

- A *description* of python as a **framework**
  - Formalizing interfaces of **building blocks** of the *language itself*
    - Such as **sequences / iterators / functions /classes / context managers etc**

---

- Use **==special methods==** $\to$ have *double underscores* (*dunders*) such as `__getitem__`

```python
my_collection.__getitem__(key) # my_collection[key]
```

- Special  method names use cases:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220222222322835.png)

## Example - Pythonic Card Deck

```python
import collections
from random import choice

'''
    > collections.namedtuple > constructs simple class to represent individual cards
    > use this to build classes of objects that are just bundles of attributes with no
      custom methods
'''
Card = collections.namedtuple('Card', ['rank', 'suit'])


class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits
                       for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]


if __name__ == '__main__':
    beerCard = Card(rank='7', suit='diamonds')
    print(beerCard)

    # French desk response to len()
    deck = FrenchDeck()
    len(deck)

    # read specific cards provided by __getitem__
    print(deck[0])
    print(deck[-1])

    # random instance
    print("random choice: ", choice(deck))

    '''
        > user dont memorise arbitrary names for standard operations such as .size() / .length()
        
        > benefit always from python library using random.choice function.
        
    '''


```


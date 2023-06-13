## Python Data Model

## Dunder / Magic methods

- Dunder means the **double underscore** *before and after*

## Pythonic Card Deck

```python
import collections

# construct simple class to represent indivdual cards, no custom methods, creates a clean representation
# for our cards.
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
```

- demonstrate two magic methods at the end.

```python
>>> beer_card = Card('7', 'diamonds')
>>> beer_card
Card(rank='7', suit='diamonds') # representation using namedTuple
```

- Magic methods make sure that `len()` is defined on the class, alongside `[x] - indexing operator`

```python
>>> deck = FrenchDeck()
>>> len(deck)
52
```

```python
>>> deck[0]
Card(rank='2', suit='spades')
>>> deck[-1]
Card(rank='A', suit='hearts')
```

- Obtain random card from this

```python
>>> from random import choice
>>> choice(deck)
Card(rank='3', suit='hearts')
>>> choice(deck)
Card(rank='K', suit='spades')
>>> choice(deck)
Card(rank='2', suit='clubs')
```

- Users of said class dont need to recall basic stuff such as the number of items in `.size()` and `.length()`
- Always favour using the python libraries from import.

---

- Deck supports slicing as of `__getitem__` also.

```python
>>> deck[:3]
[Card(rank='2', suit='spades'), Card(rank='3', suit='spades'),
Card(rank='4', suit='spades')]
>>> deck[12::13]
# start at index 12, skip 13 spaces at a time.
[Card(rank='A', suit='spades'), Card(rank='A', suit='diamonds'),
Card(rank='A', suit='clubs'), Card(rank='A', suit='hearts')]
```

- It also makes the deck **iterable**

```python
>>> for card in deck:  # doctest: +ELLIPSIS
...   print(card)
Card(rank='2', suit='spades')
Card(rank='3', suit='spades')
Card(rank='4', suit='spades')
...
```

- iterated in reverse:

```python
>>> for card in reversed(deck):  # doctest: +ELLIPSIS
...   print(card)
Card(rank='A', suit='hearts')
Card(rank='K', suit='hearts')
Card(rank='Q', suit='hearts')
...
```

##### Ellipsis in doctests

>  Whenever possible, the Python console listings in this book were extracted from doctests to ensure accuracy. When the output was too long, the elided part is marked by an ellipsis (`...`) like in the last line in the preceding code. In such cases, we used the `# doctest: +ELLIPSIS` directive to make the doctest pass. If you are trying these examples in the interactive console, you may omit the doctest directives altogether.

- iteration often implicit 
  - If some collection has no `__contains__` method
    - The in operator does  a sequential scan.
- `in` works because its iterable.

```python
>>> Card('Q', 'hearts') in deck
True
>>> Card('7', 'beasts') in deck
False
```

- Choose a sorting rule

```python
suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)

def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank)
    return rank_value * len(suit_values) + suit_values[card.suit]
```

- List the deck in order of increasing rank, using sorted
  - Takes in the item to sort, and a predicate value to order them by.

```python
>>> for card in sorted(deck, key=spades_high):  # doctest: +ELLIPSIS
...      print(card)
Card(rank='2', suit='clubs')
Card(rank='2', suit='diamonds')
Card(rank='2', suit='hearts')
... (46 cards omitted)
Card(rank='A', suit='diamonds')
Card(rank='A', suit='hearts')
Card(rank='A', suit='spades')
```

- We leverage the data model and not from the inherited object of python.
- implementing **special methods** makes the class behave like normal python sequences.
  - Thus use python core features as shown above.
    - Along with standard library features.

##### Shuffling

> As implemented so far, a `FrenchDeck` cannot be shuffled, because it is *immutable*: the cards, and their positions cannot be changed, except by violating encapsulation and handling the `_cards` attribute directly. In [Chapter 13](https://learning.oreilly.com/library/view/fluent-python-2nd/9781492056348/ch13.html#ifaces_prot_abc), we will fix that by adding a one-line `__setitem__` method.

## How special methods are used

- Meant to be called by the interpreter.
- writing `len(object)` > interpreted as `object.__len__()`

---

- Python variable sized collections written in C such as `list` 
  - use a struct called `PyVarObject` 
    1. `ob_size` field holding number of items in the collection.

- Therefore if our object is an instance of the built ins which call the ob_size underlying c field
  - This is much faster than a method.

---

- Often implicit special method calls
  - `for i in x: ` causes `iter(x)` > `x.__iter()__`
    - Backup method > `__getitem__`

---

- Should rarely be invoking the methods , but rather implementing.
- Special method used by user is `__init__` 
  - This invoke the initializer of the superclass in your own `__init__` implementation.

---

- Prefer the `built in` methods such as `len()` 

### Emulating Numeric Types

- Class for 2D vectors.

###### Tip

> The built-in `complex` type can be used to represent two-dimensional vectors, but our class can be extended to represent *n*-dimensional vectors. We will do that in [Chapter 17](https://learning.oreilly.com/library/view/fluent-python-2nd/9781492056348/ch17.html#iterables2generators).

```python
import math

class Vector:

    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __repr__(self):
        return f'Vector({self.x!r}, {self.y!r})'

    def __abs__(self):
        # calcuclate the absolute value
        return math.hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def __add__(self, other):        
        # other is a reference to the other operator passed
        x = self.x + other.x
        y = self.y + other.y
        return Vector(x, y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
```

- Operator overloading python shown here.

- Each one returns a new instance of `Vector`. 

##### Warning 

> As implemented, [Example 1-2](https://learning.oreilly.com/library/view/fluent-python-2nd/9781492056348/ch01.html#ex_vector2d) allows multiplying a `Vector` by a number, but not a number by a `Vector`, which violates the commutative property of scalar multiplication. We will fix that with the special method `__rmul__` in [Chapter 16](https://learning.oreilly.com/library/view/fluent-python-2nd/9781492056348/ch16.html#operator_overloading).

### String Representation

- `__repr__` called by the `repr` built in to get string version of the object for inspection
- Without this $\to$ instances of our class : `<Class object at 0x10e1000070>`

---

- `repr` console / debugger on the results expressions evaluated.
  - `%r` place holder in class formatting with `%` operator
  - `!r` conversion field in the new Format string syntax used in *f-strings* the `str.format` method.

---

- The *f string* 
  - uses !r to obtain the standard representation of the attributes to be displayed.
    - Shows difference of passing numbers / strings to the class constructor.

---

- String returned should be clear and unique and **match the source code** required to *create* the **represented object**
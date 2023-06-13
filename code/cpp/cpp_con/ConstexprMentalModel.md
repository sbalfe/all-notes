# Mental model of constexpr

- Move computation > runtime > compile time.
- This is **not metaprogramming**

---

- C++11 > single return statements only
- constexpr members $\implies$ const.
- non virtual members.
- no switching or branching $\to$ except form **ternaries**.
- Almost turing complete.
- Very minimal stdlib support.

---

- C++14 > multiple statements allowed
- constexpr member no longer const
- lambdas not allowed at the moment
- definitely turing complete.

---

- C++17 > much more stdlib
- lambdas automatically constexpr enabled

---

- C++20 > constexpr dynamic allocations.
- dynamic allocations cannot leave constexpr
- Other constexpr shit
  - Destructors
  - vector
  - string
  - std algorithms.

---

## What can be done at compile time with constexpr

- Almost anything.
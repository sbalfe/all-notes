- Allow scalability of increasing in size.
- Create a single state store that is passed around rather than a state binded to many react components.]
- update occur to state of which is passed down

---

### Why redux

- Good for managing large states
- useful for sharing data between components.

- predictable state management using the 3 principles.
  1. Single source of truth
  2. State is read only
  3. Changes using pure functions
     - same input = same output

- Action > Root Reducer > Store > Dom changes.
  - Funnels all actions to a pure function which modifies the state thus changing the dom specifically on this store update.

#### Flux pattern

- action > dispatcher > store > view
- ensure problems are solved in a logical sense

---

- Want to reduce prop drilling where states are passed down the components for no need.
- This can become worse as the same component may have deeper nesting in our application.
- Reduce duplicate states being created amongst the application, alongside the issue of duplicate code.
- ![Redux+flow+diagram](D:\University\Notes\DiscreteMaths\Resources\Redux+flow+diagram-1626362834412.png)

- Reduces are just parts of the entire application state to be directed to the components that require its data.

- Each specific reducer has its data that it requires combined into root reducer to be used in each one.

- Component > fire action > has type: value > update a reducer of that the correct state to manage e.g. User State.


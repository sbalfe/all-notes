# Effect / Side Effect

-  Main Job > Render UI and React to User Input
  - Evaluate and Render JSX
  - Manage state and props
  - react to user events and inputs
  - evaluate component upon state and prop changes.
- Baked into react via the tools and features covered later
  - useState(), Hook, Props
- Everything else is a **side effects**.
  - Store data in browser storage
  - send http requests to backend servers
  - set and manage timers.

- These tasks must occur **outside of the normal component evaluation** and render cycle
  - As they can be **blocking** such as a **API request**.

- Dont want to create endless loop of API data > state change > re render  > request again...

---

### Use effect Hook

```react

// param 1 > function that should be executed after every component evaluation
// if specfied dependencies changed.

// param 2 > the dependencies is an array, runs this function if they are they are changed.
useEffect(() => {...}, [dependencies]);
```

- Only run the effect when the dependencies themselves change.

---

What to add & Not to add as Dependencies

In the previous lecture, we explored `useEffect()` dependencies.

You learned, that you should add "everything" you use in the effect function as a dependency - i.e. all state variables and functions you use in there.

That is correct, but there are a **few exceptions** you should be aware of:

- You **DON'T need to add state updating functions** (as we did in the last lecture with `setFormIsValid`): React guarantees that those functions never change, hence you don't need to add them as dependencies (you could though)
- You also **DON'T need to add "built-in" APIs or functions** like `fetch()`, `localStorage` etc (functions and features built-into the browser and hence available globally): These browser APIs / global functions are not related to the React component render cycle and they also never change
- You also **DON'T need to add variables or functions** you might've **defined OUTSIDE of your components** (e.g. if you create a new helper function in a separate file): Such functions or variables also are not created inside of a component function and hence changing them won't affect your components (components won't be re-evaluated if such variables or functions change and vice-versa)

So long story short: You must add all "things" you use in your effect function **if those "things" could change because your component (or some parent component) re-rendered.** That's why variables or state defined in component functions, props or functions defined in component functions have to be added as dependencies!

Here's a made-up dummy example to further clarify the above-mentioned scenarios:

```react
import { useEffect, useState } from 'react';
 
let myTimer;
 
const MyComponent = (props) => {
  const [timerIsActive, setTimerIsActive] = useState(false);
 
  const { timerDuration } = props; // using destructuring to pull out specific props values
 
  useEffect(() => {
    if (!timerIsActive) {
      setTimerIsActive(true);
      myTimer = setTimeout(() => {
        setTimerIsActive(false);
      }, timerDuration);
    }
  }, [timerIsActive, timerDuration]);
};
```

In this example:

- `timerIsActive` is **added as a dependency** because it's component state that may change when the component changes (e.g. because the state was updated)
- `timerDuration` is **added as a dependency** because it's a prop value of that component - so it may change if a parent component changes that value (causing this MyComponent component to re-render as well)
- `setTimerIsActive` is **NOT added as a dependency** because it's that **exception**: State updating functions could be added but don't have to be added since React guarantees that the functions themselves never change
- `myTimer` is **NOT added as a dependency** because it's **not a component-internal variable** (i.e. not some state or a prop value) - it's defined outside of the component and changing it (no matter where) **wouldn't cause the component to be re-evaluated**
- `setTimeout` is **NOT added as a dependency** because it's **a built-in API** (built-into the browser) - it's independent from React and your components, it doesn't change

---

## useReducer() for state management

- May have more complex states
  - Example $\to$ if there are multiple states, multiple ways of changing it or dependencies to other states.

- useState for this case becomes **hard or error prone**
  - Easy to write , bad and inefficient buggy code in such scenarios

- use this hook when there is a useState hook that depends on multiple other states also.

```react
const [state, dispatchFm] = useReducer(reduceFn, initialState, initFn);

// reduceFn > a function triggered automatically once an action is dispatched via dispatchFn()
// receives the latest state snapshot and should return the new updated state.
```

- state snapshot used in the component re-render / re-evaluation cycle
- a dispatch function used to dispatch a new action (such as triggering the update of some state.)

- `initFn` used to set the initial state if more steps are required.

## Adding nested properties as deps to useEffect

- can pass any value , even nested values to useEffect to obtain specificity.+

```react
useEffect(() => {
    // code using someProperty
}, [object.someProperty]) // make to ensure this is specific , and not just object itself so it doesnt re run too many times.
```

## useState vs useReducer

- useReducer is clear when there are many use States that have dependencies between one another which can just be bounded into one hook
  - Of which passed to useEffect to run specific functions on the update of a complex specific value changing.

---

- use state > main / independent pieces of state and data/ if state updates are easy and limited to few kinds of updates.
- use reducer  > more power > related pieces of state/data > more complex state updates and changes.

## Context API

- need to access data from other components in other areas of the application.

- reduce complex and longwinded passing props up and down , through components who dont fucking give a shit about them, smelly ass props

---

- context not optimal for high frequency changes.

---

## Hook rules

- only call in react functions > components / custom hooks
- dont call in block functions / lambdas / callback etc.
- Not allow in if statements
- call in the top level always
- add everything within the useEffect within the dependency > anything that is likely to be dynamic and definitely change.

```react
<A.Provider value={AValue}> 
    <B.Provider value={BValue}>
        <App /> 
    </B.Provider>
</A.Provider>

// providing input from two differe
```


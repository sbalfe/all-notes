# How React works notes

- React is a JS library for building UI
- ReactDOM is the interface to the web.

- Components transferred to the real DOM for websites to be rendered.
- React cares about props passed to components, child parent communication
  - Also, context and state amongst others for holding internal and component wide data.

- Virtual DOM holds a snapshot of actual DOM and revaluates a component
  - This may not re render the DOM.
- real DOM only changes if state has changed since the component has updated
  - Massive performance increase.

## Closer look at child component revaluation.

- Default, a child component of some function would be re rendered even if the props passed in are not changed.

- Use **reactMemo**  to prevent components from rendering when its props have not changed
- Only works for **class based components**
- This does have performance cost is higher, must be weighed out in terms of whether re renders are gonna be commonly unnecessary.

- if very likely to change always then its most likely never gonna be really useful and affect overall performance.

- a function if passed to a component is recreated on each render therefore if a component with memo on it has a function passed to it
  - it will re render unlike what you expect

- functions / arrays etc , even if have the same elements are still recognised as different 

---

https://reactjs.org/docs/hooks-reference.html#usecallback - as you can see use callback is good for optimized components that need to only re render on a new function being passed to them.

---

## Components and state

- state management and component are the main part of react and are managed by react itself

---

- State changes are scheduled rather than occurring instantly.

- 2 synchronous calls to state calls for example $\to$ batches to **one state update** 
  - Changes **2 states** in **one process**

---

## useMemo

https://medium.com/@jan.hesters/usecallback-vs-usememo-c23ad1dc60#:~:text=useCallback%20and%20useMemo%20both%20expect,function%20and%20returns%20the%20result.

- use memo stores a function memoized to an array of dependencies and returns the result of the function when called
- use callback returns the actual memoized object itself.
- use memo can apply memoization to any object also.

---

## Error boundaries

- 

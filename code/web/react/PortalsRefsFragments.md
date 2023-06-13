# JSX limitations

- Adjacent root level elements in a return of a component, this is an error as only one element can be returned.
- such as two tags that are not nested within each other.

```html
<h1>fuck</h1>
<p>you</p>
```

- wrapping them with a div is the solution to this of course as this counts as one item being returned.

- can also return within an array

```react
return (
	[
        <div></div>
        <p>
    ]
)
```

- must have a key on each of the jsx elements when being passed around as a property or return value of a render.
- issue becomes having div soup where there are many nested div elements that are taking many properties between.

- only exist because of the requirement and add **no semantics**.
- must render the elements also , and its essentially unnecessary. 

---

## Wrapper Component

```react
<Wrapper>
      {error && (
        <ErrorModal
          title={error.title}
          message={error.message}
          onConfirm={errorHandler}
        />
      )}
      <Card className={classes.input}>
        <form onSubmit={addUserHandler}>
          <label htmlFor="username">Username</label>
          <input
            id="username"
            type="text"
            value={enteredUsername}
            onChange={usernameChangeHandler}
          />
          <label htmlFor="age">Age (Years)</label>
          <input
            id="age"
            type="number"
            value={enteredAge}
            onChange={ageChangeHandler}
          />
          <Button type="submit">Add User</Button>
        </form>
      </Card>
    </Wrapper>
```

```react
const Wrapper = props => {
  return props.children;
};

export default Wrapper;
```

## Introducing fragments

```react

// This does not work.
return(
	<React.fragment>
        <h2>test</h2>
        <h3>test3</h3>
    </React.fragment>  
);
```

### Portals

- Certain elements such as modals $\to$ which overlay the entire page can cause issues in clean code
  - if its nested in some other code it seems wrong.

- Similar to styling a div like a button and adding an event listener > works > but not good practice


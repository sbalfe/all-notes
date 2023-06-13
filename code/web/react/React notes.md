## React notes

- React can be injected into any div container part of our website and rendered there.

```javascript

// renders the app where the id of root is in our html file.
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

```

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```



- React library enables the actually html syntax to be created
- React DOM actually manages the updating and interacting of the website.
  - This interacts with the actual dom.

---

## Classes

- State 
  - Javasript object with properties.

```js
 this.state = {
            string: 'Hello Simons'
       }
```

- State set in the class can be accessed from the html returned to update the values.

- This can be duplicated multiple times.
- Component gives us set state method to modify state elements.

---

```c++
    render (){
        return (
            /* className is a jsx element, we are setting the classname of the jsx class that this html is part of*/
            <div className="App">
                <header className="App-header">
                    <img src={logo} className="App-logo" alt="logo" />
                    {/*render javascript between braces*/}
                    <p>
                        {this.state.string}
                    </p>
                    {/* on click call the set state provided by the component class */}
                    {/*
                        render is called again when the set state is activated, for unidirectional flow.

                        onClick is also just a jsx attribute

                        idea is we are breaking the components into virtual dom which when rendered update based on various
                        states that are changing.
                    */}
                    <button onClick = {() => this.setState({string: 'Hello Cunt'})}>
                        fuck off
                    </button>
                </header>
            </div>
        );
    }
}

export default App;
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708004914266.png)

- Lifecycle methods > called at different stages of component rendering.

- Mount > place component onto the dom.

- state of the app is trickled down into each component via props
- multiple components require key attributes to differentiate.

- re renders on the new prop information, trickles down like a tree to re render any changes on dependent components of one another

```react
import React, { Component } from 'react';
import {CardList } from './components/card-list/card-list.component';
import './App.css';

// keep the functionality of whatever was in the React.Component
class App extends Component {
   constructor(){
       //super is used to call constructor of component
       super();
       this.state = {
            monsters: [],
            searchField: ''
       };
   }

   //https://reactjs.org/docs/react-component.html
   componentDidMount(){
       // fetch data and update the monsters array to be the users that are fetched from the api response.
       fetch('https://jsonplaceholder.typicode.com/users')
           .then(response => response.json())
           .then(users => this.setState({monsters: users}))
   }
    // render is overridden from react component extension
    render (){
		
        const {monsters,  searchField } = this.state;
        const filteredMonsters = monsters.filter(monster =>
            monster.name.toLowerCase().includes(searchField.toLowerCase())
        )
        return (
            <div className="App">
                {/* passing the set state to handler of onChange*/}
                <input type = 'password'
                       placeholder = 'searh monsters'
                       onChange = {e =>
                           /* this is an asynchronous function*/
                           this.setState({searchField: e.target.value})
                       }/>
                {/*
                    associate each monster with some unique key

                    must know which value in the array to update if something changes, this is the use of a unique field.
                    does not need to re render everything.
                */}
                < CardList monsters = {filteredMonsters}/>
            </div>
        );
    }
}

export default App;

```

- https://felixgerschau.com/react-rerender-components/ > how rendering works in react.
- Make sure to place where the state is so the tree can access what it requires in the unidirectional data flow method.
- event occurs > trigger react events > create handler > handle change to change state and it tells component above it to notify this
  - this trickles down to other components.

- binding functions javascript $\to$ https://stackoverflow.com/questions/2236747/what-is-the-use-of-the-javascript-bind-method

- js function declared as arrow in a class, takes the context of this from the class , which binds automatically the references of this inside.

```react
import React, { Component } from 'react';
import {CardList } from './components/card-list/card-list.component';
import {SearchBox} from "./components/search-box/search-box.component";
import './App.css';



class App extends Component {
   constructor(props){
       super(props);
       this.state = {
            monsters: [],
            searchField: ''
       };
       /* pass this from the constructor into this method to access the state*/
       //this.handleChange = this.handleChange.bind(this);
   }
   componentDidMount(){
       fetch('https://jsonplaceholder.typicode.com/users')
           .then(response => response.json())
           .then(users => this.setState({monsters: users}))
   }

   /* this function does set the context of this implicit */
   /* changing to arrow function sets context of this from whatever called it in the first place*/
    /*https://hackernoon.com/javascript-es6-arrow-functions-and-lexical-this-f2a3e2a5e8c4*/

   handleChange = (e) => {
       this.setState({searchField: e.target.value})
   }

    render (){
       console.log("re rendering")

        const {monsters,  searchField } = this.state;
        const filteredMonsters = monsters.filter(monster =>
            monster.name.toLowerCase().includes(searchField.toLowerCase())
        )
        return (
            <div className="App">

                <SearchBox
                    placeholder = 'search monsters'
                    handleChange= {this.handleChange}
                />
                < CardList monsters = {filteredMonsters}/>
            </div>
        );
    }
}

export default App;

```

https://www.youtube.com/watch?v=dn4mmfbletg&t=50s - how to deploy react apps.

---

## React and reactDOM

- Children in the DOM form a tree like structure where the parent are the upper nodes of the children embedded.
- Diagram showing how the views  > passed to actions > updates state > updates views ....

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210713144256902.png)

## Lifecycle methods

https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210713154408940.png)

- Mounting , where the component is place on the DOM for the first time.
  - Calls the constructor of our class extending the react component class.
  - super() gives access to the lifecycle methods and render from the react component class.

- render is when the component notifies JS what they want to display in terms of html. also where props value are evaluated to be displayed as html.
- after render > update DOM, updates component.
  - Knows what update looks like as of the render, know the state on the class component as of the constructor.

- After mount > componentDidMount lifecycle method is fired.

---

- updating phase
  - diagram shows how the new props / state changes affect the render and dom automatically.

- Does no re mount, just re render the components on updating.
- only changes what is updated, on the props.
- all the children are re rendered even if they dont need to
  - use the `shouldComponentUpdate` that takes previous props and state and use to determine whether the update is actually called.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210713161227802.png)

- Full list.

---

- Make sure to place the state correctly as to only re render the components that depend on it.

- we are lifting the state when we activate a handler and pass information to it.


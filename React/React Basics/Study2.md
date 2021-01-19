## 27. Single Page Application

- SPA is communicating with back-end initially
- No need to go back and forth with server
- JS requests to re-render the DOM instead of requesting pages to the server every time
- Communicating with different APIs/end point/server such as Google  back and forth to receive data
- Dynamic, no hard coding - when user open/click something we communicate with the outside servers

## 28. Fetching Content

- Life cycle methods
    - methods that are called  in different stages when this component gets rendered
    - automatically by React
    - componentDidMount is called when react put the components on the page, renders its DOM for the first time

    ```jsx
    import './App.css';
    import { Component } from 'react';

    class App extends Component {
      constructor(){
        super();
        this.state= {
          monsters: []
        };
      }

      componentDidMount(){
        fetch('https://jsonplaceholder.typicode.com/users')
          .then(res => res.json()) // convert it to json format
          .then(users => this.setState({monsters: users}));
      }

      render() {
        return (
          <div className="App">
            {
              this.state.monsters.map(monster => 
                <h1 key={monster.id}>{monster.name}</h1>)
            }
          </div>
        );
      }
    }

    export default App;
    ```

### 💚Promises

- the eventual completion or failure of an asynchronous operation ans its resulting value
- A proxy for a value not necessarily known when the promise is created
- Make asnyc methods return value like synchrounous methods
- asnyc methods returns a *promise* to give us the value in the future (response)
    - Promise states: pending, fulfilled, rejected
- Avoid callback hell, increase code readability

```jsx
const myPromise = new Promise((resolve, reject) => {
	if(true) {
			setTimeout(()=>{
			resolve('I have succeeded');
		}, 1000);
	}else {
		reject('I have failed')
	}
});

// whenever promise resovles the value, it goes to then and print value
myPromise.then(value => console.log(value))
				.catch(rejectValue => consle.log(rejectValue));

// Chaning
myPromise.then(value => value + '!!!!')
				.then(newValue => console.log(newValue))
				.catch(rejectValue => consle.log(rejectValue));
```

- when we call APIs, we could get successful response or failed response. we can use a promise in this case.

## 30. Architecting Our App

### Custom Component

- Name should start with capital letter
- Functional component / arrow function component, Class component
    - always return JSX
- file extension could be both .jsx or .js
    - at the end, through Webpack and Babel convert jsx to js file

    babel? webpack? - different purposes  but show up together 

    **babel**: a transpiler. It can translate all high version script such ECMAScript into ES5 which is widely supported by browsers.

    **webpack**: webpack works on top of JS, so after babel do their job, webpack packages modules with complex dependency relationships into bundles. (compiling bundle for web app)

- Organizing files in different directories is important as architecture point of view\

## 31. Card List Component

### props

- props object is used for children objects, no children, it will be null
- Children is what it gets called between brackets

```jsx
// App.js
import './App.css';
import { Component } from 'react';
import { CardList } from './components/card-list/card-list.component'

class App extends Component {
  constructor(){
    super();
    this.state= {
      monsters: []
    };
  }

  componentDidMount(){
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(users => this.setState({monsters: users}));
  }

  render() {
    return (
      <div className="App">
        <CardList name = "Jessie"> //my custom component with props name
          { // this is props children 
            this.state.monsters.map(monster => 
              <h1 key={monster.id}>{monster.name}</h1>)
          }
        </CardList>
      </div>
    );
  }
}

export default App;
```

```jsx
//card-list.component.jsx
//functional arrow component 
import React from 'react';
import './card-list.styles.css';

//functional arrow component 
export const CardList = props => {
    return <div className="card-list">{props.children}</div>;
}
```

- We can access props in App component using this.props

string interpolation using ` (back tick) 

## 33/34. Card Component,  Breaking into Components

```jsx
// app.js
render() {
    return (
      <div className="App">
        <CardList monsters = {this.state.monsters}/> //pass an attribute
      </div>
    );
}

// CardList
export const CardList = props => (
     <div className="card-list">
        {
            props.monsters.map(monster =>
                <Card key={monster.id} monster={monster}></Card>
            )
        }
    </div>
);

// Card
export const Card = (props) => (
    <div className ='card-container'>
        <img alt="monster" src={`https://robohash.org/${props.monster.id}?set=set2&size=180x180`}></img>
        <h2>{props.monster.name}</h2>    
        <p>{props.monster.email}</p>
    </div>
);
```

When do we break things down into components?

- increase flexibility, reusability
- each component do only one thing and could be used in different places

## 35. State vs Props

- when App component pass down the state as an attribute to CardList component, CardList component receives as a props
- State
    - it is changed/updated by user interaction.
    - it lives in one location
    - it trickles down to child components as a props

key is very important. When one of value is changed, react internally checks the key and changes the specific key value only. not all the values. It is faster and efficient.


- when state changes, react has to run render functions again where components has props trickled down from state

## 36. SearchField State

- SearchField: Storing the input as a state and filter it out
- onChange={}
    - fire onChange synthetic event whenever input changes
    - native event
    - get target

```jsx
import './App.css';
import { Component } from 'react';
import { CardList } from './components/card-list/card-list.component';

class App extends Component {
  constructor(){
    super();
    this.state= {
      monsters: [],
      searchField: ''
    };
  }

  componentDidMount(){
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(users => this.setState({monsters: users}));
  }

  render() {
    return (
      <div className="App">
        <input 
          type='search' 
          placeholder='Search Monsters' 
          onChange={e => this.setState({ searchField: e.target.value })}></input>
        <CardList monsters={this.state.monsters}></CardList>
      </div>
    );
  }
}

export default App;
```

- this.setState(updater, [callback])
    - Due to user's actions, it will be changed
    - async function call - JS doesn't know when it finishes
    - second argument of callback called after setState sets the state and it will run the function

```jsx
 render() {
    return (
      <div className="App">
        <input 
          type='search' 
          placeholder='Search Monsters' 
          onChange={e => this.setState({ searchField: e.target.value }, () => 
							console.log(this.state)
						)
				}/>
        <CardList monsters={this.state.monsters}></CardList>
      </div>
    );
  }
```

## 37. React Events

- SyntheticEvent - onChange
    - When the DOM event happens such user types input or user clicks, DOM onchange event notify something is changed
    - React intercept the something is changed and communicate the react app is called SyntheticEvent
- When we do setState (Declarative) , react bot knows when is the best time to update the DOM and make an schedule. If there is multiple updates, react bot optimize them.

## 38. Filtering State

- Destructor

```jsx
const { monsters, searchField } = this.state;

// same as
const monsters = this.state.monsters;
const searchField = this.state.searchField;

```

```jsx
render() {
    const { monsters, searchField } = this.state;
    const filteredMonsters = monsters.filter(monster => 
        monster.name.toLowerCase().includes(searchField.toLowerCase())
      ); 
    return (
      <div className="App">
        <input 
          type='search' 
          placeholder='Search Monsters' 
          onChange={e => this.setState({ searchField: e.target.value })}></input>
        <CardList monsters={filteredMonsters}></CardList>
      </div>
    );
  }
```

- whenever setState() is called, react re-renders component
    1. Itrigering setState by user input 
    2. setting state value of searchField 
    3. caused re-rendering the component by recalling render function
    4. refilters out monsters
    5. passing out filteredMonsters to CardList Component
    6. Re-render CardList Component
- Dynamically update card list
- React takes a control what to render what to rerender for us

fillter() function
-  /bureturn true or false 
- when it is true, it keep the value 

include(value, index) function
- return true or false
- when it includes the value, it returns true

JS primitive types
: string, boolean, null, undefined, number, symbol in JS memory bank
*Anything is not primitive type is an Object such as array and Object has own unique object memory

```jsx
// JS doesn't compare values, it compares where it is pointing at (memory reference)

var a = 3; var b = 3;
a == b; // true because it is primitive type located in JS memory bank
var c = a; 
a = 5; // c is still 3

var obj1 = {id: 1}; var obj2 = {id: 1}
obj1 == obj2; // false becuase it is not primitive type located in own unique object memory
var obj3 = obj1;
obj1.id = 2 // obj3.id = 2
```

## 40. Search Box Component

- A functional component is unlike a class component, it can't access a constructor, internal state, and life cycle methods. when we just need to render methods, we use a functional component.
    - easier to read and test
- reusable search box component

```jsx
// App component
render() {
    const { monsters, searchField } = this.state;
    const filteredMonsters = monsters.filter(monster => 
        monster.name.toLowerCase().includes(searchField.toLowerCase())
      );
    return (
      <div className="App">
        <SearchBox 
          placeholder = 'Search Monsters'
          handleChange = {e => this.setState({searchField: e.target.value})}
        ></SearchBox>
        <CardList monsters={filteredMonsters}></CardList>
      </div>
    );
  }

//search-box component
export const SearchBox = ({placeholder, handleChange}) => (
    <input 
        className='search'
        type='search' 
        placeholder= {placeholder}
        onChange={handleChange}></input>

);
```

## 41. Where To Put State?

- Lifting State Up
    - Keep the state in the highest level of component which uses the state, so we could use it in the other components as well.
    - EX) we need searchField state in the both CardList and SearchBox  components. So it should be located in App component

## 42. Class Methods and Arrow Functions

### super()

- extends functionality existing in Component from React such as life cycle methods and render method

### this keyword

- reference the state in this class component
- *this* keyword sets the context of the class component
- when *this* keyword inside the context of app such as Life Cycle methods and render method, *this* keyword references the state in the class component
- JS by default doesn't set its scope of *this* on functions. we have to explicitly state to be in the app component - constructor()

```jsx
class App extends Component {
  constructor(){
    super();
    this.state= {
      monsters: [],
      searchField: ''
    };

    this.handleChange = this.handleChange.bind(this);
  }

  componentDidMount(){
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(users => this.setState({monsters: users}));
  }

  handleChange(e){
    this.setState({searchField: e.target.value})
  }

  render() {
    const { monsters, searchField } = this.state;
    const filteredMonsters = monsters.filter(monster => 
        monster.name.toLowerCase().includes(searchField.toLowerCase())
      );
    return (
      <div className="App">
        <SearchBox 
          placeholder = 'Search Monsters'
          handleChange = {this.handleChange}
        ></SearchBox>
        <CardList monsters={filteredMonsters}></CardList>
      </div>
    );
  }
}
```

- But, using arrow function, it automatically bind *this* keyword to set the context which is the current class component

```jsx
handleChange = e => {
    this.setState({searchField: e.target.value})
  }
```

[JavaScript Scope](https://www.notion.so/JavaScript-Scope-9dd7ef4ee1fe4705b85829a58831cf2a)

## 43/44. Event Binding

```jsx
class App extends React.Component {
	constructor(){
		super();
		this.handleClick2 = this.handleClick1.bind(this);
	}

	handleClick1(){
		console.log('button 1 clicked');
	}

	handleClick3 = () => console.log('button 4 clicked');

	render(){
		return(
			<div>
				// when component is rendered, print 'button 1 clicked'
				<button onClick = {this.handleClick1()}>button 1 </button> 
				// print 'button 1 clicked' 
				<button onClick = {this.handleClick1}>button 2 </button>
				// print 'button 1 clicked' 
				<button onClick = {this.handleClick2}>button 3 </button>
				// print 'button 4 clicked'
				<button onClick = {this.handleClick3}>button 4 </button>
			</div>
		)
	}
}
```

- Use arrow functions on any class methods I define which are not part of React such render() and life cycle methods

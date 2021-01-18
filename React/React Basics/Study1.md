## 19/20. Create React App 1, 2

```jsx

nvm use 14.15.4
npx create-react-app monsters-rolodex
yarn start
// yarn is faster because of concurently install
// npx 패키지 인스톨 할 필요 없이 최신 버전 실행가능

```

- build folder is to deploy our application
- src folder is our playground.
- public folder is for browser. Becuase the browsers only can understand an older version of html codes.
    - npm run build  → do the job to move all the codes from src  to public after converting
- React App is JSX which is HTML codes in Javascript
- ReactDOM is a robot who interact with DOM.
    - ReactDOM grabs an element from HTML and insert App into it.


## 23/24 Class Component, JSX

- by using that class component instead of function, we can access state
- state is a JS object with properties we can access inside of the class using constructor method

```jsx
import logo from './logo.svg';
import './App.css';
import { Component } from 'react';

class App extends Component {
  constructor(){
    super();
    this.state= {
      string: 'Hello Jessie hi'
    }
  }
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            {this.state.string}
          </p>
          <button onClick={() => this.setState({string: 'Hello you'})}>
            Change Text
            </button>
        </header>
      </div>
    );
  }
}

export default App;
```

- as soon as state is changed, React intercepts the changes. That means it render the component again with new state and creates virtual dom

## 25. Dynamic Content

```jsx
import logo from './logo.svg';
import './App.css';
import { Component } from 'react';

class App extends Component {
  constructor(){
    super();
    this.state= {
      monster: [
        {
          name: 'Frankenstein',
          id: 'abc1'
        },
        {
          name: 'Dracula',
          id: 'abc2'
        },
        {
          name: 'Zombi',
          id: 'abc3'
        }
      ]
      
    }
  }
  render() {
    return (
      <div className="App">
        {
          this.state.monster.map(monster => 
            <h1>{monster.name}</h1>)
        }
        
      </div>
    );
  }
}

export default App;
```

- inside curl-braces, we can use JS. Using map we can access individual element
- map - Each element require unique key/id, so react knows which element is updated and renders the specific element only. (Performance-wise, really efficient)

### Map

- map, filter, reduce, find, includes
- An object that iterates its elements in order - for ... of loop
- key and value pairs

```jsx
var array = [2, 3, 4, 5];
array.map(el => console.log(el+10));
// 12
// 13
// 14
// 15

// CRUD a map object 
let contacts = new Map()
contacts.set('Jessie', {phone: "213-555-1234", address: "123 N 1st Ave"})
contacts.has('Jessie') // true
contacts.get('Hilary') // undefined
contacts.set('Hilary', {phone: "617-555-4321", address: "321 S 2nd St"})
contacts.get('Jessie') // {phone: "213-555-1234", address: "123 N 1st Ave"}
contacts.delete('Raymond') // false
contacts.delete('Jessie') // true
console.log(contacts.size) // 1
```
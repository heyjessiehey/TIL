## 1. Don't touch the DOM. I'll do it
- JS manipulate DOM
- Before react DOM, programming is called Imperative(giving an authoritative command).
    - directly change individual parts of the app
    - It responds to various events.
- React uses Declarative approach.
    - Developer states data
    - based on the states, react make those changes.
    - we don't need to order one by one any more. React does it automatically for us.
    - VirtualDom is one big Javascript object that describes how the app should look
    - less complexity, better code quality, faster dev time

## 2. Build websites like lego blocks
- Reusable Components
- state: any data that describes our app
- components: JS function/class that receives some input or attribute (props) return html inside JS (JSX)
- Based on state, we builds our component and add it to our page. Reuse as much as I want.

## 3. Unidirectional Data flow
- function React(state, components){} creates JS version of virtualDOM based on state and compnents
- VirtualDOM: JS Object (App) - blue print how our DOM should look like.
- React robot look at the virtual DOM and modifies DOM for us
- whenever the DOM is changed by user like click the button, the state is changed and then react combine new state with components and update the DOM.
- Data only flows one way. (yellow boxes are different componets)

## 4. UI, The rest is up to you

- Main react library which is virtual DOM is everywhere.
- cross platform
- when we build our app, we imports two libraries which were React and ReactDOM(Robot - knows browser)

---
## The Job of a React Developer
1. Decide on Components
2. Decide the State and where it lives
3. What changes when state changes
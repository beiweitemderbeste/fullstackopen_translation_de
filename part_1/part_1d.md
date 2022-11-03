# part 1d

> table of contents

## Inhaltsverzeichnis

- [A note on React version](#A-note-on-React-version)
- [Complex state](#Complex-state)
- [Handling arrays](#Handling-arrays)
- [Conditional rendering](#Conditional-rendering)
- [Old React](#Old-React)
- [Rules of Hooks](#Rules-of-Hooks)
- [Event Handling Revisited](#Event-Handling-Revisited)
- [Function that returns a function](#Function-that-returns-a-function)
- [Passing Event Handlers to Child Components](#Passing-Event-Handlers-to-Child-Components)
- [Do Not Define Components Within Components](#Do-Not-Define-Components-Within-Components)
- [Useful Reading](#Useful-Reading)
- [Exercises](#Exercises)

## A note on React version

> Version 18 of React was released late March 2022. The code in this course should work with the new React version. However, some libraries might not yet be compatible with React 18. At the moment of writing (4th April) at least the Apollo client used in part 8 does not yet work with most recent React.

> In case you end up in a situation where your application breaks because of library compatibility problems, downgrade to the older React by changing the file package.json as follows:

```javascript
{
  "dependencies": {
    "react": "^17.0.2",    "react-dom": "^17.0.2",    "react-scripts": "5.0.0",
    "web-vitals": "^2.1.4"
  },
  // ...
}
```

> After the change is made, reinstall dependencies by running

```javascript
npm install
```

> Note that also the file index.js needs to be changed a bit. For React 17 it looks like

```javascript
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

> but for React 18 the correct form is

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

## Complex state

> In our previous example the application state was simple as it was comprised of a single integer. What if our application requires a more complex state?

> In most cases the easiest and best way to accomplish this is by using the useState function multiple times to create separate "pieces" of state.

> In the following code we create two pieces of state for the application named left and right that both get the initial value of 0:

```javascript
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)

  return (
    <div>
      {left}
      <button onClick={() => setLeft(left + 1)}>
        left
      </button>
      <button onClick={() => setRight(right + 1)}>
        right
      </button>
      {right}
    </div>
  )
}
```

> The component gets access to the functions setLeft and setRight that it can use to update the two pieces of state.

> The component's state or a piece of its state can be of any type. We could implement the same functionality by saving the click count of both the left and right buttons into a single object:

```javascript
{
  left: 0,
  right: 0
}
```

> In this case the application would look like this:

```javascript
const App = () => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })

  const handleLeftClick = () => {
    const newClicks = { 
      left: clicks.left + 1, 
      right: clicks.right 
    }
    setClicks(newClicks)
  }

  const handleRightClick = () => {
    const newClicks = { 
      left: clicks.left, 
      right: clicks.right + 1 
    }
    setClicks(newClicks)
  }

  return (
    <div>
      {clicks.left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {clicks.right}
    </div>
  )
}
```

> Now the component only has a single piece of state and the event handlers have to take care of changing the entire application state.

> The event handler looks a bit messy. When the left button is clicked, the following function is called:

```javascript
const handleLeftClick = () => {
  const newClicks = { 
    left: clicks.left + 1, 
    right: clicks.right 
  }
  setClicks(newClicks)
}
```

> The following object is set as the new state of the application:

```javascript
{
  left: clicks.left + 1,
  right: clicks.right
}
```

> The new value of the left property is now the same as the value of left + 1 from the previous state, and the value of the right property is the same as value of the right property from the previous state.

> We can define the new state object a bit more neatly by using the object spread syntax that was added to the language specification in the summer of 2018:

```javascript
const handleLeftClick = () => {
  const newClicks = { 
    ...clicks, 
    left: clicks.left + 1 
  }
  setClicks(newClicks)
}

const handleRightClick = () => {
  const newClicks = { 
    ...clicks, 
    right: clicks.right + 1 
  }
  setClicks(newClicks)
}
```

> The syntax may seem a bit strange at first. In practice { ...clicks } creates a new object that has copies of all of the properties of the clicks object. When we specify a particular property - e.g. right in { ...clicks, right: 1 }, the value of the right property in the new object will be 1.

> In the example above, this:

```javascript
{ ...clicks, right: clicks.right + 1 }
```

> creates a copy of the clicks object where the value of the right property is increased by one.

> Assigning the object to a variable in the event handlers is not necessary and we can simplify the functions to the following form:

```javascript
const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })
```

> Some readers might be wondering why we didn't just update the state directly, like this:

```javascript
const handleLeftClick = () => {
  clicks.left++
  setClicks(clicks)
}
```

> The application appears to work. However, it is forbidden in React to mutate state directly, since it can result in unexpected side effects. Changing state has to always be done by setting the state to a new object. If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object, and setting that as the new state.

> Storing all of the state in a single state object is a bad choice for this particular application; there's no apparent benefit and the resulting application is a lot more complex. In this case storing the click counters into separate pieces of state is a far more suitable choice.

> There are situations where it can be beneficial to store a piece of application state in a more complex data structure. The official React documentation contains some helpful guidance on the topic.

## Handling arrays

> Let's add a piece of state to our application containing an array allClicks that remembers every click that has occurred in the application.

```javascript
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {    
    setAll(allClicks.concat('L'))    
    setLeft(left + 1)  
  }

  const handleRightClick = () => {    
    setAll(allClicks.concat('R'))    
    setRight(right + 1)  
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>    
    </div>
  )
}
```

> Every click is stored into a separate piece of state called allClicks that is initialized as an empty array:

```javascript
const [allClicks, setAll] = useState([])
```

> When the left button is clicked, we add the letter L to the allClicks array:

```javascript
const handleLeftClick = () => {
  setAll(allClicks.concat('L'))
  setLeft(left + 1)
}
```

> The piece of state stored in allClicks is now set to be an array that contains all of the items of the previous state array plus the letter L. Adding the new item to the array is accomplished with the concat method, that does not mutate the existing array but rather returns a new copy of the array with the item added to it.

> As mentioned previously, it's also possible in JavaScript to add items to an array with the push method. If we add the item by pushing it to the allClicks array and then updating the state, the application would still appear to work:

```javascript
const handleLeftClick = () => {
  allClicks.push('L')
  setAll(allClicks)
  setLeft(left + 1)
}
```

> However, don't do this. As mentioned previously, the state of React components like allClicks must not be mutated directly. Even if mutating state appears to work in some cases, it can lead to problems that are very hard to debug.

> Let's take a closer look at how the clicking is rendered to the page:

```javascript
const App = () => {
  // ...

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>    
    </div>
  )
}
```

> We call the join method on the allClicks array that joins all the items into a single string, separated by the string passed as the function parameter, which in our case is an empty space.

## Conditional rendering
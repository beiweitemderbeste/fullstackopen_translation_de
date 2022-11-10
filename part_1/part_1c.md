# part 1c

> table of contents

## Inhaltsverzeichnis

- [Component helper functions](#Component-helper-functions)
- [Destructuring](#Destructuring)
- [Page re-rendering](#Page-re-rendering)
- [Stateful component](#Stateful-component)
- [Event handling](#Event-handling)
- [Event handler is a function](#Event-handler-is-a-function)
- [Passing state to child components](#Passing-state-to-child-components)
- [Changes in state cause rerendering](#Changes-in-state-cause-rerendering)
- [Refactoring the components](#Refactoring-the-components)

> Let's go back to working with React.

Arbeiten wir weiter mit React.

> We start with a new example:

Fangen wir mit einem neuen Beispiel an:

```javascript
const Hello = (props) => {
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  )
}

const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <div>
      <h1>Greetings</h1>
      <Hello name="Maya" age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

## Component helper functions

> Let's expand our Hello component so that it guesses the year of birth of the person being greeted:

Erweitern wir den Komponenten Hello so, dass er das Geburtsjahr der Person, die gegrüßt wird, errät.

```javascript
const Hello = (props) => {
  const bornYear = () => {    
    const yearNow = new Date().getFullYear()    
    return yearNow - props.age  
  }

  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
      <p>
        So you were probably born in {bornYear()}
      </p>    
    </div>
  )
}
```

> The logic for guessing the year of birth is separated into a function of its own that is called when the component is rendered.

Die Logik hinter dem Erraten des Geburtsjahres steckt in einer eigenen Funktion, die aufgerufen wird, wenn der Komponent gerendert wird.

> The person's age does not have to be passed as a parameter to the function, since it can directly access all props that are passed to the component.

Das Alter der Person muss nicht als Parameter an die Funktion übergeben werden, da die Funktion direkten Zugang zu allen props bekommt, die an den Komponenten übergeben werden.

> If we examine our current code closely, we'll notice that the helper function is actually defined inside of another function that defines the behavior of our component. In Java programming, defining a function inside another one is complex and cumbersome, so not all that common. In JavaScript, however, defining functions within functions is a commonly-used technique.

Wenn wir unseren Code genauer anschauen, bemerken wir, das die Hilfsfunktion eigentlich in einer anderen Funktion definiert ist, die das Verhalten unseres Komponenten definiert. Wenn man mit Java programmiert ist die Definition einer Funktion innerhalb einer anderen Funktion komplex und mühsam, also nicht allzu gewöhnlich, während es in Javascript sehr gebräuchlich ist.

## Destructuring

> Before we move forward, we will take a look at a small but useful feature of the JavaScript language that was added in the ES6 specification, that allows us to destructure values from objects and arrays upon assignment.

Bevor wir weitermachen, schauen wir uns noch ein kleines aber nützliches Feature von Javascript an, das mit der ES6-Spezifikation hinzugefügt wurde: das Destrukturieren von Werten von Objekten und Arrays in Variablen.

> In our previous code, we had to reference the data passed to our component as props.name and props.age. Of these two expressions we had to repeat props.age twice in our code.

In unserem vorangegangenen Code haben wir auf die Daten, die an unseren Komponenten übergeben wurden, mit props.name und props.age verwiesen. Von diesen beiden Ausdrücken mussten wir props.age zweimal verwenden.

> Since props is an object

Da props ein Objekt ist

```javascript
props = {
  name: 'Arto Hellas',
  age: 35,
}
```
> we can streamline our component by assigning the values of the properties directly into two variables name and age which we can then use in our code:

können wir unseren Komponenten optimieren, in dem wir die Werte der props direkt zwei Variablen "name" und "age" zuweisen, die wir in unserem Code verwenden:

```javascript
const Hello = (props) => {
  const name = props.name  
  const age = props.age

  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>      
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

> Note that we've also utilized the more compact syntax for arrow functions when defining the bornYear function. As mentioned earlier, if an arrow function consists of a single expression, then the function body does not need to be written inside of curly braces. In this more compact form, the function simply returns the result of the single expression.

Hinweis: Wir haben die kompaktere Syntax von Pfeilfunktionen benutzt, um die Funktion bornYear zu definieren. Wie bereits erwähnt, muss der Körper der Funktion nicht in geschweiften Klammen stehen, wenn die Pfeilfunktion aus nur einem Ausdruck besteht. In dieser kompakteren Form gibt die Funktion lediglich das Ergebnis des einzigen Ausdrucks aus.

> To recap, the two function definitions shown below are equivalent:

Zur Wiederholung: die zwei Funktionsdefinitionen sind gleich:

```javascript
const bornYear = () => new Date().getFullYear() - age

const bornYear = () => {
  return new Date().getFullYear() - age
}
```

> Destructuring makes the assignment of variables even easier, since we can use it to extract and gather the values of an object's properties into separate variables:

Das Destrukturieren macht das Zuweisen von Variablen noch einfacher, da wir es verwenden können, um die Werte eines Objekts in verschiedenen Variablen zu speichern:

```javascript
const Hello = (props) => {
  const { name, age } = props  
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

> If the object we are destructuring has the values

Wenn das Objekt, das wir destrukturieren, folgende Werte hat

```javascript
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

> the expression const { name, age } = props assigns the values 'Arto Hellas' to name and 35 to age.

dann weist der Ausdruck const { name, age } = props die Werte "Arto Hellas" an "name" und "35" an "age" zu.

> We can take destructuring a step further:

Wir können mit dem Destrukturieren auch einen Schritt weitergehen:

```javascript
const Hello = ({ name, age }) => {  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>
        Hello {name}, you are {age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

> The props that are passed to the component are now directly destructured into the variables name and age.

Die props, die an den Komponenten weitergegeben werden, werden jetzt direkt in die Variablen "name" und "age" destrukturiert.

> This means that instead of assigning the entire props object into a variable called props and then assigning its properties into the variables name and age

Das bedeutet, dass anstatt das ganze props-Objekts einer Variablen "props" zugewiesen wird und dann seine Eigenschaften an die Variablen "name" und "age" zugewiesen werden

```javascript
const Hello = (props) => {
const { name, age } = props
```

> we assign the values of the properties directly to variables by destructuring the props object that is passed to the component function as a parameter:

weisen wir die Werte der Eigenschaft direkt den Variablen zu, indem wir das props-Objekt, das an den Komponeten als Parameter übergeben wird, destrukturieren:

```javascript
const Hello = ({ name, age }) => {
```

## Page re-rendering

> So far all of our applications have been such that their appearance remains the same after the initial rendering. What if we wanted to create a counter where the value increased as a function of time or at the click of a button?

Bis jetzt waren alle unsere Anwendungen in ihrer Erscheinung gleich, alle veränderten sich nicht nach dem ersten Rendern. Was aber, wenn wir einen Zähler verwenden wollen, dessen Zählerstand sich bei einem Klick auf einen Button oder einer Funktion nach Zeit erhöht?

> Let's start with the following. File App.js becomes:

Fangen wir mit folgendem an. Die Datei App.js wird zu:

```javascript
const App = (props) => {
  const {counter} = props
  return (
    <div>{counter}</div>
  )
}

export default App
```

> And file index.js becomes:

Und die Datei index.js wird zu:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

let counter = 1

ReactDOM.createRoot(document.getElementById('root')).render(
  <App counter={counter} />
)
```

> The App component is given the value of the counter via the counter prop. This component renders the value to the screen. What happens when the value of counter changes? Even if we were to add the following

Der Komponent App bekommt den Wert des Zählers über die prop "counter". Dieser Komponent zeigt den Wert auf dem Bildschirm an. Was passiert, wenn sich der Zählerstand ändert? Was wenn wir das folgende hinzufügen?

```javascript
counter += 1
```

Der Komponent wird nicht erneut gerendert. Wir bekommen den Komponenten dazu, indem wir die Methode render ein zweites Mal aufrufen, z.B. so:

> the component won't re-render. We can get the component to re-render by calling the render method a second time, e.g. in the following way:

```javascript
let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}

refresh()
counter += 1
refresh()
counter += 1
refresh()
```

> The re-rendering command has been wrapped inside of the refresh function to cut down on the amount of copy-pasted code.

Der Befehl zum erneuten rendern wurde in die Funktion refresh eingebaut, um zu verhindern, dass zuviel Code kopiert wird.

> Now the component renders three times, first with the value 1, then 2, and finally 3. However, the values 1 and 2 are displayed on the screen for such a short amount of time that they can't be noticed.

Jetzt wird der Komponent dreimal gerendert, das erste Mal mit dem Wert 1, dann 2 und schließlich 3. Allerdings werden die Werte 1 und 2 nur so kurz am Bildschirm ausgegeben, dass man es nicht bemerken kann.

> We can implement slightly more interesting functionality by re-rendering and incrementing the counter every second by using setInterval:

Wir können eine leicht interessantere Funktionalität einbauen, indem wir den Zähler mit jeder Sekunde erneut rendern und erhöhen:

```javascript
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

> Making repeated calls to the render method is not the recommended way to re-render components. Next, we'll introduce a better way of accomplishing this effect.

Erneute Aufrufe der Methode render ist nicht der empfohlene Weg, um in React Komponenten erneut zu rendern. Als nächstes führen wir eine bessere Möglichkeit ein, um das zu erreichen.

## Stateful component

> All of our components up till now have been simple in the sense that they have not contained any state that could change during the lifecycle of the component.

Bis jetzt waren unsere Komponenten sehr einfach gestaltet, d.h. sie enthalten keinen State, der sich im Lebenszyklus eines Komponenten ändern könnte.

> Next, let's add state to our application's App component with the help of React's state hook.

Als nächste erweitern wir unseren Komponenten App mithilfe des React State Hooks um einen State.

> We will change the application as follows. index.js goes back to

Änderdt die Anwendung wie folgt ab. index.js wird zu

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

> and App.js changes to the following:

```javascript
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(    
    () => setCounter(counter + 1),    
    1000  
  )

  return (
    <div>{counter}</div>
  )
}

export default App
```

> In the first row, the file imports the useState function:

In der ersten Zeile importiert die Datei die Funktion useState:

```javascript
import { useState } from 'react'
```

> The function body that defines the component begins with the function call:

Der Körper der Funktion, der den Komponenten definiert, beginnt mit einem Funktionsaufruf:

```javascript
const [ counter, setCounter ] = useState(0)
```

> The function call adds state to the component and renders it initialized with the value of zero. The function returns an array that contains two items. We assign the items to the variables counter and setCounter by using the destructuring assignment syntax shown earlier.

Der Funktionsaufruf fügt einen State an den Komponenten und zeigt ihn mit einem Anfangswert von 0 an. Die Funktion gibt ein Array mit 2 Elementen zurück. Diese weisen wir den Variablen counter und setCounter zu, indem wir die Destrukturierung aus den vorangegangenen Kapiteln nutzen.

> The counter variable is assigned the initial value of state which is zero. The variable setCounter is assigned to a function that will be used to modify the state.

Der Variablen counter wird der Startwert von State (der 0 ist) zugewiesen. Der Variablen setCounter wird eine Funktion zugewiesen, die dafür genutzt wird den State zu ändern.

> The application calls the setTimeout function and passes it two parameters: a function to increment the counter state and a timeout of one second:

Die Anwendung ruft die Funktion setTimeout auf und übergibt ihr zwei Parameter: eine Funktion, um den State des Zählers zu erhöhen und einen Timeout von einer Sekunde:

```javascript
setTimeout(
  () => setCounter(counter + 1),
  1000
)
```

> The function passed as the first parameter to the setTimeout function is invoked one second after calling the setTimeout function

Die Funktion, die die Funktion setTimeout als ersten Parameter übergeben bekommt, wird eine Sekunde nachdem Funtionsaufruf von setTimeout gestartet.

```javascript
() => setCounter(counter + 1)
```

> When the state modifying function setCounter is called, React re-renders the component which means that the function body of the component function gets re-executed:

Wenn die Funktion setCounter, mit der man den State verändern kann, aufgerufen wird, wird der Komponent erneut gerendert, was bedeutet das der Körper der Komponentenfunktion erneut ausgeführt wird:

```javascript
() => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}
```

> The second time the component function is executed it calls the useState function and returns the new value of the state: 1. Executing the function body again also makes a new function call to setTimeout, which executes the one second timeout and increments the counter state again. Because the value of the counter variable is 1, incrementing the value by 1 is essentially the same as an expression setting the value of counter to 2.

Wenn die Komponentenfunktion das zweite Mal ausgeführt wird, ruft sie die Funktion useState auf und gibt den neuen Wert von State aus: 1. Das erneute Ausführen des Funktionskörpers des Komponenten, führt zu einem zweiten Timeout und erhöht den Zählerstand erneut. Weil der Wert der Zählervariable 1 ist, ist es grundlegend dasselbe, wie ein Ausdruck der den Wert des Zählerstands auf 2 setzt.

```javascript
() => setCounter(2)
```

> Meanwhile, the old value of counter - "1" - is rendered to the screen.

Währenddessen wird der alte Wert vom Zähler - "1" - am Bildschirm ausgegeben.

> Every time the setCounter modifies the state it causes the component to re-render. The value of the state will be incremented again after one second, and this will continue to repeat for as long as the application is running.

Jedes Mal, wenn setCounter den State verändert, wird dadurch ein erneutes Rendern des Komponenten erzeugt. Der Wert des States wird wieder nach einer Sekunde erhöht, was dazuführt das alles wiederholt wird, solange die Anwendung läuft.

> If the component doesn't render when you think it should, or if it renders at the "wrong time", you can debug the application by logging the values of the component's variables to the console. If we make the following additions to our code:

Wenn euch der Komponent nicht das anzeigen sollte, was ihr denkt er sollte, oder wenn es "zur falschen Zeit" angezeigt wird, könnt ihr die Anwendung auf Fehler untersuchen, indem ihr euch die Werte der Variablen in der Konsole ausgeben lasst. Übernehmt die folgenden Änderungen in euren Code:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  console.log('rendering...', counter)

  return (
    <div>{counter}</div>
  )
}
```

> It's easy to follow and track the calls made to the App component's render function:

Somit lassen sich die Funktionsaufrufe des Komponenten App leicht folgen:

!["fullstack content"](./images/part1c_image1.png?raw=true)

## Event Handling

> We have already mentioned event handlers that are registered to be called when specific events occur a few times in part 0. E.g. a user's interaction with the different elements of a web page can cause a collection of various different kinds of events to be triggered.

> Let's change the application so that increasing the counter happens when a user clicks a button, which is implemented with the button element.

> Button elements support so-called mouse events, of which click is the most common event. The click event on a button can also be triggered with the keyboard or a touch screen despite the name mouse event.

> In React, registering an event handler function to the click event happens like this:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const handleClick = () => {   
    console.log('clicked')  
  }
  return (
    <div>
      <div>{counter}</div>
      <button onClick={handleClick}>        
        plus      
      </button>    
    </div>
  )
}
```

> We set the value of the button's onClick attribute to be a reference to the handleClick function defined in the code.

> Now every click of the plus button causes the handleClick function to be called, meaning that every click event will log a clicked message to the browser console.

> The event handler function can also be defined directly in the value assignment of the onClick-attribute:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => console.log('clicked')}>        
        plus
      </button>
    </div>
  )
}
```

> By changing the event handler to the following form

```javascript
<button onClick={() => setCounter(counter + 1)}>
  plus
</button>
```

> we achieve the desired behavior, meaning that the value of counter is increased by one and the component gets re-rendered.

> Let's also add a button for resetting the counter:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => setCounter(counter + 1)}>
        plus
      </button>
        <button onClick={() => setCounter(0)}>         
          zero      
        </button>    
      </div>
  )
}
```

> Our application is now ready!

## Event handler is a function

> We define the event handlers for our buttons where we declare their onClick attributes:

```javascript
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

> What if we tried to define the event handlers in a simpler form?

```javascript
<button onClick={setCounter(counter + 1)}> 
  plus
</button>
```

> This would completely break our application:

!["fullstack content"](./images/part1c_image2.png?raw=true)

> What's going on? An event handler is supposed to be either a function or a function reference, and when we write:

```javascript
<button onClick={setCounter(counter + 1)}>
```

> the event handler is actually a function call. In many situations this is ok, but not in this particular situation. In the beginning the value of the counter variable is 0. When React renders the component for the first time, it executes the function call setCounter(0+1), and changes the value of the component's state to 1. This will cause the component to be re-rendered, React will execute the setCounter function call again, and the state will change leading to another rerender...

> Let's define the event handlers like we did before:

```javascript
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

> Now the button's attribute which defines what happens when the button is clicked - onClick - has the value () => setCounter(counter + 1). The setCounter function is called only when a user clicks the button.

> Usually defining event handlers within JSX-templates is not a good idea. Here it's ok, because our event handlers are so simple.

> Let's separate the event handlers into separate functions anyway: 

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)    
  
  const setToZero = () => setCounter(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={increaseByOne}>        
        plus
      </button>
      <button onClick={setToZero}>        
        zero
      </button>
    </div>
  )
}
```

> Here, the event handlers have been defined correctly. The value of the onClick attribute is a variable containing a reference to a function:

```javascript
<button onClick={increaseByOne}> 
  plus
</button>
```

## Passing state to child components

> It's recommended to write React components that are small and reusable across the application and even across projects. Let's refactor our application so that it's composed of three smaller components, one component for displaying the counter and two components for buttons.

> Let's first implement a Display component that's responsible for displaying the value of the counter.

> One best practice in React is to lift the state up in the component hierarchy. The documentation says:

> Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.

> So let's place the application's state in the App component and pass it down to the Display component through props:

```javascript
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

> Using the component is straightforward, as we only need to pass the state of the counter to it:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>      
      <button onClick={increaseByOne}>
        plus
      </button>
      <button onClick={setToZero}> 
        zero
      </button>
    </div>
  )
}
```

> Everything still works. When the buttons are clicked and the App gets re-rendered, all of its children including the Display component are also re-rendered.

> Next, let's make a Button component for the buttons of our application. We have to pass the event handler as well as the title of the button through the component's props:

```javascript
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

> Our App component now looks like this:

```javascript
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)  
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      <Button        
        onClick={increaseByOne}        
        text='plus'      
      />      
      <Button        
        onClick={setToZero}        
        text='zero'      
      />           
      <Button        
        onClick={decreaseByOne}        
        text='minus'      
      />               
    </div>
  )
}
```

> Since we now have an easily reusable Button component, we've also implemented new functionality into our application by adding a button that can be used to decrement the counter.

> The event handler is passed to the Button component through the onClick prop. The name of the prop itself is not that significant, but our naming choice wasn't completely random. React's own official tutorial suggests this convention.

## Changes in state cause rerendering

> Let's go over the main principles of how an application works once more.

> When the application starts, the code in App is executed. This code uses a useState hook to create the application state, setting an initial value of the variable counter. This component contains the Display component - which displays the counter's value, 0 - and three Button components. The buttons all have event handlers, which are used to change the state of the counter.

> When one of the buttons is clicked, the event handler is executed. The event handler changes the state of the App component with the setCounter function. Calling a function which changes the state causes the component to rerender.

> So, if a user clicks the plus button, the button's event handler changes the value of counter to 1, and the App component is rerendered. This causes its subcomponents Display and Button to also be re-rendered. Display receives the new value of the counter, 1, as props. The Button components receive event handlers which can be used to change the state of the counter.

## Refactoring the components

> The component displaying the value of the counter is as follows:

```javascript
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

> The component only uses the counter field of its props. This means we can simplify the component by using destructuring, like so:

```javascript
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

> The function defining the component contains only the return statement, so we can define the function using the more compact form of arrow functions:

```javascript
const Display = ({ counter }) => <div>{counter}</div>
```

> We can simplify the Button component as well.

```javascript
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

> We can use destructuring to get only the required fields from props, and use the more compact form of arrow functions:

```javascript
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
```

> We can simplify the Button component once more by declaring the return statement in just one line:

```javascript
const Button = ({ onClick, text }) => <button onClick={onClick}>{text}</button>
```

> However, be careful to not oversimplify your components, as this makes adding complexity a more tedious task down the road.

[back to Part 1b](part_1b.md)

[next Part 1d](part_1d.md)
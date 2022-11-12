# part 2a

> table of contents

## Inhaltsverzeichnis

- [console.log](#console-log)
- [Protip: Visual Studio Code snippets](#Protip-Visual-Studio-Code-snippets)
- [Javascript Arrays](#Javascript-Arrays)
- [event handlers revisited](#event-handlers-revisited)
- [rendering collections](#rendering-collections)
- [key-attribute](#key-attribute)
- [map](#map)
- [anti-pattern: array indexes as keys](#anti-pattern:-array-indexes-as-keys)
- [refactoring modules](#refactoring-modules)
- [when the application breaks](#when-the-application-breaks)
- [exercises](#exercises)

> Before starting a new part, let's recap some of the topics that proved difficult last year.

## console.log

> What's the difference between an experienced JavaScript programmer and a rookie? The experienced one uses console.log 10-100 times more.

> Paradoxically, this seems to be true even though a rookie programmer would need console.log (or any debugging method) more than an experienced one.

> When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging.

> NB As explained in part 1, when you use the command console.log for debugging, don't concatenate things 'the Java way' with a plus. Instead of writing:

```javascript
console.log('props value is ' + props)
```

separate the things to be printed with a comma:

```javascript
console.log('props value is', props)
```

If you concatenate an object with a string and log it to the console (like in our first example), the result will be pretty useless: 

```javascript
props value is [object Object]
```

On the contrary, when you pass objects as distinct arguments separated by commas to console.log, like in our second example above, the content of the object is printed to the developer console as strings that are insightful. If necessary, read more about debugging React-applications.

## Protip: Visual Studio Code snippets

> With Visual Studio Code it's easy to create 'snippets', i.e., shortcuts for quickly generating commonly re-used portions of code, much like how 'sout' works in Netbeans.

> Instructions for creating snippets can be found here.

> Useful, ready-made snippets can also be found as VS Code plugins, in the marketplace.

> The most important snippet is the one for the console.log() command, for example, clog. This can be created like so: 

```javascript
{
  "console.log": {
    "prefix": "clog",
    "body": [
      "console.log('$1')",
    ],
    "description": "Log output to console"
  }
}
```

> Debugging your code using console.log() is so common that Visual Studio Code has that snippet built in. To use it, type log and hit tab to autocomplete. More fully featured console.log() snippet extensions can be found in the marketplace.

## JavaScript Arrays

> From here on out, we will be using the functional programming methods of the JavaScript array, such as find, filter, and map - all of the time. They operate on the same general principles as streams do in Java 8, which have been used during the last few years in both the 'Ohjelmoinnin perusteet' and 'Ohjelmoinnin jatkokurssi' courses at the university's department of Computer Science, and also in the programming MOOC.

> If functional programming with arrays feels foreign to you, it is worth watching at least the first three parts of the YouTube video series Functional Programming in JavaScript:

> - Higher-order functions
> - Map
> - Reduce basics

## Event Handlers Revisited

> Based on last year's course, event handling has proved to be difficult.

> It's worth reading the revision chapter at the end of the previous part event handlers revisited, if it feels like your own knowledge on the topic needs some brushing up.

> Passing event handlers to the child components of the App component has raised some questions. A small revision on the topic can be found here.

## Rendering Collections

> We will now do the 'frontend', or the browser-side application logic, in React for an application that's similar to the example application from part 0

> Let's start with the following (the file App.js):

```javascript
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        <li>{notes[0].content}</li>
        <li>{notes[1].content}</li>
        <li>{notes[2].content}</li>
      </ul>
    </div>
  )
}

export default App
```

> The file index.js looks like:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

const notes = [
  {
    id: 1,
    content: 'HTML is easy',
    date: '2019-05-30T17:30:31.098Z',
    important: true
  },
  {
    id: 2,
    content: 'Browser can execute only JavaScript',
    date: '2019-05-30T18:39:34.091Z',
    important: false
  },
  {
    id: 3,
    content: 'GET and POST are the most important methods of HTTP protocol',
    date: '2019-05-30T19:20:14.298Z',
    important: true
  }
]

ReactDOM.createRoot(document.getElementById('root')).render(
  <App notes={notes} />
)
```

> Every note contains its textual content and a timestamp, as well as a boolean value for marking whether the note has been categorized as important or not, and also a unique id.

> The example above works due to the fact that there are exactly three notes in the array.

> A single note is rendered by accessing the objects in the array by referring to a hard-coded index number:

```javascript
<li>{notes[1].content}</li>
```

> This is, of course, not practical. We can improve on this by generating React elements from the array objects using the map function.

```javascript
notes.map(note => <li>{note.content}</li>)
```

> The result is an array of li elements.

```javascript
[
  <li>HTML is easy</li>,
  <li>Browser can execute only JavaScript</li>,
  <li>GET and POST are the most important methods of HTTP protocol</li>,
]
```

> Which can then be placed inside ul tags:

```javascript
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>        {notes.map(note => <li>{note.content}</li>)}      </ul>    </div>
  )
}
```

> Because the code generating the li tags is JavaScript, it must be wrapped in curly braces in a JSX template just like all other JavaScript code.

> We will also make the code more readable by separating the arrow function's declaration across multiple lines:

```javascript
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li>            
            {note.content}          
          </li>        
        )}
      </ul>
    </div>
  )
}
```
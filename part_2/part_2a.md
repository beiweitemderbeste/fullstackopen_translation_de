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

> On the contrary, when you pass objects as distinct arguments separated by commas to console.log, like in our second example above, the content of the object is printed to the developer console as strings that are insightful. If necessary, read more about debugging React-applications.

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

## key-attribut

> Even though the application seems to be working, there is a nasty warning in the console:

```fullstack content```

> As the linked React page in the error message suggests; the list items, i.e. the elements generated by the map method, must each have a unique key value: an attribute called key.

> Let's add the keys:

```javascript
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>            
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

> And the error message disappears.

> React uses the key attributes of objects in an array to determine how to update the view generated by a component when the component is re-rendered. More about this in the React documentation.

## Map

> Understanding how the array method map works is crucial for the rest of the course.

> The application contains an array called notes:

```javascript
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
```

> Let's pause for a moment and examine how map works.

> If the following code is added to, let's say, the end of the file:

```javascript
const result = notes.map(note => note.id)
console.log(result)
```

> [1, 2, 3] will be printed to the console. map always creates a new array, the elements of which have been created from the elements of the original array by mapping: using the function given as a parameter to the map method.

> The function is

```javascript
note => note.id
```

> Which is an arrow function written in compact form. The full form would be: 

```javascript
(note) => {
  return note.id
}
```

> The function gets a note object as a parameter, and returns the value of its id field.

> Changing the command to:

```javascript
const result = notes.map(note => note.content)
```

> > The function gets a note object as a parameter, and returns the value of its id field.

> Changing the command to:

```javascript
notes.map(note =>
  <li key={note.id}>{note.content}</li>
)
```

> which generates an li tag containing the contents of the note from each note object.

> Because the function parameter passed to the map method - 

```javascript
note => <li key={note.id}>{note.content}</li>
```

>  - is used to create view elements, the value of the variable must be rendered inside curly braces. Try to see what happens if the braces are removed.

> The use of curly braces will cause some pain in the beginning, but you will get used to them soon enough. The visual feedback from React is immediate.

## Anti-pattern: Array Indexes as Keys

> We could have made the error message on our console disappear by using the array indexes as keys. The indexes can be retrieved by passing a second parameter to the callback function of the map method: 

```javascript
notes.map((note, i) => ...)
```

> When called like this, i is assigned the value of the index of the position in the array where the note resides.

> As such, one way to define the row generation without getting errors is:

```javascript
<ul>
  {notes.map((note, i) => 
    <li key={i}>
      {note.content}
    </li>
  )}
</ul>
```

> This is; however, not recommended and can create undesired problems even if it seems to be working just fine.

> Read more about this in this article.

## Refactoring Modules

> Let's tidy the code up a bit. We are only interested in the field notes of the props, so let's retrieve that directly using destructuring: 

```javascript
const App = ({ notes }) => {  
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

> If you have forgotten what destructuring means and how it works, please review the section on destructuring.

> We'll separate displaying a single note into its own component Note: 

```javascript
const Note = ({ note }) => {  
  return (    
    <li>{note.content}</li>  
  )
}

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note =>           
          <Note key={note.id} note={note} />        
        )}      
      </ul>
    </div>
  )
}
```

> Note that the key attribute must now be defined for the Note components, and not for the li tags like before.

> A whole React application can be written in a single file. Although that is, of course, not very practical. Common practice is to declare each component in their own file as an ES6-module.

> We have been using modules the whole time. The first few lines of the file index.js:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'
```

> import three modules, enabling them to be used in that file. The module React is placed into the variable React, the module react-dom into the variable ReactDOM, and the module that defines the main component of the app is placed into the variable App

> Let's move our Note component into its own module.

> In smaller applications, components are usually placed in a directory called components, which is in turn placed within the src directory. The convention is to name the file after the component.

> Now, we'll create a directory called components for our application and place a file named Note.js inside. The contents of the Note.js file are as follows: 

```javascript
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

export default Note
```

> The last line of the module exports the declared module, the variable Note.

> Now the file that is using the component - App.js - can import the module: 

```javascript
import Note from './components/Note'

const App = ({ notes }) => {
  // ...
}
```

> The component exported by the module is now available for use in the variable Note, just as it was earlier.

> Note that when importing our own components, their location must be given in relation to the importing file:

```javascript
'./components/Note'
```

> The period - . - in the beginning refers to the current directory, so the module's location is a file called Note.js in the components sub-directory of the current directory. The filename extension .js can be omitted.

> Modules have plenty of other uses other than enabling component declarations to be separated into their own files. We will get back to them later in this course.

> The current code of the application can be found on GitHub.

> Note that the main branch of the repository contains the code for a later version of the application. The current code is in the branch part2-1:

!["fullstack content"](./images/part2a_image1.png?raw=true)

> If you clone the project, run the command npm install before starting the application with npm start.

## When the Application Breaks
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

Bevor wir den neuen Abschnitt anfangen, schauen wir uns noch einige Themen an, die sich letztes Jahr als schwierig erwiesen haben.

## console.log

> What's the difference between an experienced JavaScript programmer and a rookie? The experienced one uses console.log 10-100 times more.

Was ist der Unterschied zwischen einem erfahrenen Javascriptprogrammierer und einem Anfänger? Der Programmierer nutzt console.log 10-100 mal öfter.

> Paradoxically, this seems to be true even though a rookie programmer would need console.log (or any debugging method) more than an experienced one.

Paradoxerweise scheint das wahr zu sein, obwohl ein Anfänger console.log (oder eine andere Art zu debuggen) öfter nutzen müsste als ein Profi.

> When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging.

Wenn etwas nicht funktioniert, ratet nicht, was falsch läuft. Benutzt stattdessen console.log oder etwas anderes zum Debuggen.

> NB As explained in part 1, when you use the command console.log for debugging, don't concatenate things 'the Java way' with a plus. Instead of writing:

Hinweis: Wie wir es schon im ersten Abschnitt angesprochen haben, sollte ihr "nicht den Java weg gehen", wenn es um das Benutzen von console.log geht:

```javascript
console.log('props value is ' + props)
```

> separate the things to be printed with a comma:

trennt die Elemente mit einem Komma:

```javascript
console.log('props value is', props)
```

> If you concatenate an object with a string and log it to the console (like in our first example), the result will be pretty useless: 

Wenn ihr ein Objekt mit einem String verbindet und es in der Konsole ausgebt (wie in unserem ersten Beispiel), nützt euch das Ergbnis nicht viel.

```javascript
props value is [object Object]
```

> On the contrary, when you pass objects as distinct arguments separated by commas to console.log, like in our second example above, the content of the object is printed to the developer console as strings that are insightful. If necessary, read more about debugging React-applications.

Wenn ihr im Gegensatz dazu Objekte als verschiedene, mit Komata separierte Elemente, in der Konsole ausgebt (wie in unserem zweiten Beispiel), seht ihr den Inhalt des Objekts als String. Wenn nötig, könnt ihr mehr über "debugging React-applications" lesen.

## Protip: Visual Studio Code snippets

> With Visual Studio Code it's easy to create 'snippets', i.e., shortcuts for quickly generating commonly re-used portions of code, much like how 'sout' works in Netbeans.

Mit Visual Studio Code ist es einfach "snippets" zu erstellen, z.B. Tastenkürzel um schnell wiederbenutzbaren Code zu geneieren, ähnlich wie "sout" in Netbeans funktioniert.

> Instructions for creating snippets can be found here.

Anleitungen, wie man Snippets erstellt, können [hier](hier) gefunden werden.

> Useful, ready-made snippets can also be found as VS Code plugins, in the marketplace.

Vorgefertigte Snippets gibt es auch als VSCode plugins.

> The most important snippet is the one for the console.log() command, for example, clog. This can be created like so: 

Das wichtigste Snippet ist das für den Befehl console.log(). Das kann man z.B. so erstellen:

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

Mit console.log zu debuggen ist so verbreitet, dass VSCode dafür Snippets eingebaut hat. Um es zu benutzen, gebt log und drückt Tabulator für autocomplete. Mehr Erweiterungen für console.log()-Snippets findet ihr im marketplace von VSCode.

## JavaScript Arrays

> From here on out, we will be using the functional programming methods of the JavaScript array, such as find, filter, and map - all of the time. They operate on the same general principles as streams do in Java 8, which have been used during the last few years in both the 'Ohjelmoinnin perusteet' and 'Ohjelmoinnin jatkokurssi' courses at the university's department of Computer Science, and also in the programming MOOC.

Ab diesem Zeitpunkt werden wir die ganze Zeit Methoden der funktionialen Programmierung für Javascript [Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - wie "find", "filter" und "map" - benutzen. Wir arbeiten nach den gleichen allgemeinen Prinzipien wie Streams in Java 8. Diese wurden in den letzten Jahren in den beiden Kursen "Ohjelmoinnin perusteet" und "Ohjelmoinnin jatkokurssi" der Informatikfakultät und in den Programmier-MOOC benutzt.

> If functional programming with arrays feels foreign to you, it is worth watching at least the first three parts of the YouTube video series Functional Programming in JavaScript:

Wenn sich funktionales Programmieren mit Arrays für euch fremd anfühlt, solltet ihr mindestens die ersten drei Teile der folgenden Youtube-Reihe [Functional Programming in JavaScript](https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84) anschauen:

> - Higher-order functions
> - Map
> - Reduce basics

- [higher-order functions](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84)
- [map](https://www.youtube.com/watch?v=bCqtb-Z5YGQ&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=2)
- [reduce basics](https://www.youtube.com/watch?v=Wl98eZpkp-c&t=31s)

## Event Handlers Revisited

> Based on last year's course, event handling has proved to be difficult.

Event Handling hat sich letztes Jahr im Kurs als schwierig erwiesen.

> It's worth reading the revision chapter at the end of the previous part event handlers revisited, if it feels like your own knowledge on the topic needs some brushing up.

Wenn ihr noch Wissenslücken bei diesem Thema habt, empfehlen wir euch, nochmal das Thema [event handling revisited](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps#event-handling-revisited) es letztens Abschnitts zu lesen.

> Passing event handlers to the child components of the App component has raised some questions. A small revision on the topic can be found here.

Wie man Event Handler an Unterkomponenten weitergibt, hat einige Fragen aufgeworfen. Eine kleine Wiederholung zu diesem Thema kann [hier](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps#passing-event-handlers-to-child-components) gefunden werden.

## Rendering Collections

> We will now do the 'frontend', or the browser-side application logic, in React for an application that's similar to the example application from part 0

Wir beginnen jetzt mit dem Frontend oder besser gesagt der browserseitigen Anwendungslogik in React für eine Anwendung, die vergleichbar mit der Beispielanwendung aus Abschnitt 0 ist.

> Let's start with the following (the file App.js):

Fangen wir mit dem Folgenden an (die Datei App.js):

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

Die Datei index.js sieht so aus:

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

Jede Notiz enthält einen Text, einen Zeitstempel, einen Boolean-Wert, um zu markieren, ob die Notiz als wichtig kategorisiert wurde und eine einzigartige ID.

> The example above works due to the fact that there are exactly three notes in the array.

Das obige Beispiel funktioniert, weil es exakt drei Notizen in dem Array gibt.

> A single note is rendered by accessing the objects in the array by referring to a hard-coded index number:

Eine einzige Notiz wird dargestellt, indem auf die Objekte im Array mit einer hartkodierten Index-Nummer referenziert wird.

```javascript
<li>{notes[1].content}</li>
```

> This is, of course, not practical. We can improve on this by generating React elements from the array objects using the map function.

Das ist natürlich nicht praktikabel. Verbessern wir den Code, indem wir React-Elemente aus den Array-Objekten über die Funktion map generieren:

```javascript
notes.map(note => <li>{note.content}</li>)
```

> The result is an array of li elements.

Das Ergbnis ist ein Array aus li-Elementen.

```javascript
[
  <li>HTML is easy</li>,
  <li>Browser can execute only JavaScript</li>,
  <li>GET and POST are the most important methods of HTTP protocol</li>,
]
```

> Which can then be placed inside ul tags:

die wir dann innerhalb der ul-Tags platzieren:

```javascript
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>        
        {notes.map(note => <li>{note.content}</li>)}      
      </ul>    
    </div>
  )
}
```

> Because the code generating the li tags is JavaScript, it must be wrapped in curly braces in a JSX template just like all other JavaScript code.

Weil der Code, der die li-Tags generiert aus Javascript besteht, muss dieser in geschwungenen Klammer in einem JSX-Template stehen, wie auch der andere Javascript-Code.

> We will also make the code more readable by separating the arrow function's declaration across multiple lines:

Verbessern wir den Code so, dass er leserlicher wird. Dazu spalten wir die Pfeilfunktion auf mehrere Zeilen auf:

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

Obwohl die Anwendung zu funktionieren scheint, gibt es trotzdem eine ekelhafte Warnung in der Konsole:

!["fullstack content"](./images/part2a_image1.png?raw=true)

> As the linked React page in the error message suggests; the list items, i.e. the elements generated by the map method, must each have a unique key value: an attribute called key.

Wie die, in der Fehlermeldung verlinkte, React-Seite vorschlägt muss jedes Listenelement, z.B. die Elemente, die über die map-Methode generiert wurden, ein einzigartiges Attritbut "key" besitzen.

> Let's add the keys:

Fügen wir die Keys hinzu:

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

Damit verschwindet die Fehlermeldung.

> React uses the key attributes of objects in an array to determine how to update the view generated by a component when the component is re-rendered. More about this in the React documentation.

React benutzt die Key-Attribute der Objekte eines Arrays um festzulegen, wie die Ansicht eines Komponenten bei dessem erneuten Rendern generiert werden soll. Mehr dazu gibt es in der [React-Dokumentation](https://reactjs.org/docs/reconciliation.html#recursing-on-children).

## Map

> Understanding how the array method map works is crucial for the rest of the course.

Die Array-Methode map zu verstehen, ist wesentlich für den Rest dieses Kurses:

> The application contains an array called notes:

Die Anwendung enthält ein Array "notes":

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

Pausieren wir einen Moment und untersuchen, wie map genau funktioniert.

> If the following code is added to, let's say, the end of the file:

Sagen wir, dass der folgende Code an die letzte Zeile angehängt wird:

```javascript
const result = notes.map(note => note.id)
console.log(result)
```

> [1, 2, 3] will be printed to the console. map always creates a new array, the elements of which have been created from the elements of the original array by mapping: using the function given as a parameter to the map method.

[1, 2, 3] wird an der Konsole ausgegeben. "map" erstellt immer ein neues Array. Dessen Elemente werden aus den Elementen des originalen Arrays über mapping erstellt, d.h. über die Funktion, die map als Parameter übergeben wird.

> The function is

Die Funktion lautet

```javascript
note => note.id
```

> Which is an arrow function written in compact form. The full form would be: 

Was eine Pfeilfunktion in Kurzform geschrieben ist. Die vollständige Version würrde so aussehen:

```javascript
(note) => {
  return note.id
}
```

> The function gets a note object as a parameter, and returns the value of its id field.

Die Funktion erhält eine note-Objekt als Parameter und gibt den Wert von dessen id-Feld zurück.

> Changing the command to:

Ändern wir den Befehl so ab

```javascript
const result = notes.map(note => note.content)
```

> results in an array containing the contents of the notes.

erhalten wir ein Array, das die Inhalte der Notizen enthält.

> This is already pretty close to the React code we used:

Das ist schon sehr ähnlich zu dem React-Code, den wir benutzt haben:

```javascript
notes.map(note =>
  <li key={note.id}>{note.content}</li>
)
```

> which generates an li tag containing the contents of the note from each note object.

Dieser erstellt einen li-Tag mit den Inhalten einer Notiz für jedes note-Objekt.

> Because the function parameter passed to the map method - 

Weil die Funktion, die map als Parameter übergeben bekommt, 

```javascript
note => <li key={note.id}>{note.content}</li>
```

>  - is used to create view elements, the value of the variable must be rendered inside curly braces. Try to see what happens if the braces are removed.

genutzt wird, um view-Elemente zu erstellen, müssen die Werte der Variablen in geschwungene Klammern gesetzt werden. Schaut nach, was passiert, wenn ihr diese weglasst.

> The use of curly braces will cause some pain in the beginning, but you will get used to them soon enough. The visual feedback from React is immediate.

Das Benutzen von geschwungenen Klammern wird am Anfang für Kopfschmerzen sorgen, aber ihr werdet euch noch früh genug an sie gewöhnen. React gibt dafür direkt sichtbare Rückmeldeldungen.

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

> Early in your programming career (and even after 30 years of coding like yours truly), what often happens is that the application just completely breaks down. This is even more the case with dynamically typed languages, such as JavaScript, where the compiler does not check the data type. For instance, function variables or return values.

> A "React explosion" can, for example, look like this:

!["fullstack content"](./images/part2a_image2.png?raw=true)

> In these situations your best way out is the console.log method.

> The piece of code causing the explosion is this:

```javascript
const Course = ({ course }) => (
  <div>
    <Header course={course} />
  </div>
)

const App = () => {
  const course = {
    // ...
  }

  return (
    <div>
      <Course course={course} />
    </div>
  )
}
```

> We'll hone in on the reason for the breakdown by adding console.log commands to the code. Because the first thing to be rendered is the App component, it's worth putting the first console.log there: 

```javascript
const App = () => {
  const course = {
    // ...
  }

  console.log('App works...')

  return (
    // ..
  )
}
```

> To see the printing in the console, we must scroll up over the long red wall of errors.

!["fullstack content"](./images/part2a_image3.png?raw=true)

> When one thing is found to be working, it's time to log deeper. If the component has been declared as a single statement, or a function without a return, it makes printing to the console harder.

```javascript
const Course = ({ course }) => (
  <div>
    <Header course={course} />
  </div>
)
```

> The component should be changed to its longer form in order for us to add the printing: 

```javascript
const Course = ({ course }) => { 
  console.log(course)  
  
  return (
    <div>
      <Header course={course} />
    </div>
  )
}
```

> Quite often the root of the problem is that the props are expected to be of a different type, or called with a different name than they actually are, and destructuring fails as a result. The problem often begins to solve itself when destructuring is removed and we see what the props actually contains. 

```javascript
const Course = (props) => {  
  console.log(props)  
  const { course } = props
  return (
    <div>
      <Header course={course} />
    </div>
  )
}
```

> If the problem has still not been resolved, there really isn't much to do apart from continuing to bug-hunt by sprinkling more console.log statements around your code.

> I added this chapter to the material after the model answer for the next question exploded completely (due to props being of the wrong type), and I had to debug it using console.log.

## Exercises

> The exercises are submitted via GitHub, and by marking the exercises as done in the submission system.

> You can submit all of the exercises into the same repository, or use multiple different repositories. If you submit exercises from different parts into the same repository, name your directories well.

> The exercises are submitted One part at a time. When you have submitted the exercises for a part, you can no longer submit any missed exercises for that part.

> Note that this part has more exercises than the ones before, so do not submit before you have done all exercises from this part you want to submit.

> WARNING create-react-app makes the project automatically into a git-repository, if the project is not created inside of an already existing repository. You probably do not want the project to become a repository, so run the command rm -rf .git from its root. 

### 2.1 Course information step6

> Let's finish the code for rendering course contents from exercises 1.1 - 1.5. You can start with the code from the model answers. The model answers for part 1 can be found by going to the submission system, click on my submissions at the top, and in the row corresponding to part 1 under the solutions column click on show. To see the solution to the course info exercise, click on index.js under kurssitiedot ("kurssitiedot" means "course info").

> Note that if you copy a project from one place to another, you might have to delete the node_modules directory and install the dependencies again with the command npm install before you can start the application. Generally, it's not recommended that you copy a project's whole contents and/or add the node_modules directory to the version control system.

> Let's change the App component like so: 

```javascript
const App = () => {
  const course = {
    id: 1,
    name: 'Half Stack application development',
    parts: [
      {
        name: 'Fundamentals of React',
        exercises: 10,
        id: 1
      },
      {
        name: 'Using props to pass data',
        exercises: 7,
        id: 2
      },
      {
        name: 'State of a component',
        exercises: 14,
        id: 3
      }
    ]
  }

  return <Course course={course} />
}

export default App
```

> Define a component responsible for formatting a single course called Course.

> The component structure of the application can be, for example, the following: 

```
App
  Course
    Header
    Content
      Part
      Part
      ...
```

> Hence, the Course component contains the components defined in the previous part, which are responsible for rendering the course name and its parts.

> The rendered page can, for example, look as follows: 

!["fullstack content"](./images/part2a_image4.png?raw=true)

> You don't need the sum of the exercises yet.

> The application must work regardless of the number of parts a course has, so make sure the application works if you add or remove parts of a course.

> Ensure that the console shows no errors!

### 2.2: Course information step7

> Show also the sum of the exercises of the course. 

!["fullstack content"](./images/part2a_image5.png?raw=true)

### 2.3* Course information step8

> If you haven't done so already, calculate the sum of exercises with the array method reduce.

> Pro tip: when your code looks as follows:

```javascript
const total = 
  parts.reduce((s, p) => someMagicHere)
```

> and does not work, it's worth to use console.log, which requires the arrow function to be written in its longer form:

```javascript
const total = parts.reduce((s, p) => {
  console.log('what is happening', s, p)
  return someMagicHere 
})
```

> Not working? : Use your search engine to look up how reduce is used in an Object Array.

> Pro tip 2: There is a plugin for VS Code that automatically changes short form arrow functions into their longer form, and vice versa. 

!["fullstack content"](./images/part2a_image6.png?raw=true)

### 2.4 Course information step9

> Let's extend our application to allow for an arbitrary number of courses:

```javascript
const App = () => {
  const courses = [
    {
      name: 'Half Stack application development',
      id: 1,
      parts: [
        {
          name: 'Fundamentals of React',
          exercises: 10,
          id: 1
        },
        {
          name: 'Using props to pass data',
          exercises: 7,
          id: 2
        },
        {
          name: 'State of a component',
          exercises: 14,
          id: 3
        },
        {
          name: 'Redux',
          exercises: 11,
          id: 4
        }
      ]
    }, 
    {
      name: 'Node.js',
      id: 2,
      parts: [
        {
          name: 'Routing',
          exercises: 3,
          id: 1
        },
        {
          name: 'Middlewares',
          exercises: 7,
          id: 2
        }
      ]
    }
  ]

  return (
    <div>
      // ...
    </div>
  )
}
```

> The application can, for example, look like this: 

!["fullstack content"](./images/part2a_image7.png?raw=true)

###  2.5: separate module

> Declare the Course component as a separate module, which is imported by the App component. You can include all subcomponents of the course into the same module. 
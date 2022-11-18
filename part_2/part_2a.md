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

Was ist der Unterschied zwischen einem erfahrenen Javascriptprogrammierer und einem Anfänger? Der erfahrene Programmierer nutzt console.log 10-100 mal öfter.

> Paradoxically, this seems to be true even though a rookie programmer would need console.log (or any debugging method) more than an experienced one.

Paradoxerweise scheint das wahr zu sein, obwohl ein Anfänger console.log (oder eine andere Art zu debuggen) öfter nutzen müsste als ein Profi.

> When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging.

Wenn etwas nicht funktioniert, ratet nicht, was falsch läuft. Benutzt stattdessen console.log oder etwas anderes zum Debuggen.

> NB As explained in part 1, when you use the command console.log for debugging, don't concatenate things 'the Java way' with a plus. Instead of writing:

Hinweis: Wie wir es schon im ersten Abschnitt angesprochen haben, solltet ihr "nicht den Java weg gehen", wenn es um das Benutzen von console.log geht:

```javascript
console.log('props value is ' + props)
```

> separate the things to be printed with a comma:

trennt die Elemente stattdessen mit einem Komma:

```javascript
console.log('props value is', props)
```

> If you concatenate an object with a string and log it to the console (like in our first example), the result will be pretty useless: 

Wenn ihr ein Objekt mit einem String verbindet und es in der Konsole ausgebt (wie in unserem ersten Beispiel), nützt euch das Ergbnis nicht viel.

```javascript
props value is [object Object]
```

> On the contrary, when you pass objects as distinct arguments separated by commas to console.log, like in our second example above, the content of the object is printed to the developer console as strings that are insightful. If necessary, read more about debugging React-applications.

Wenn ihr im Gegensatz dazu Objekte als verschiedene, mit Komata separierte, Elemente in der Konsole ausgebt (wie in unserem zweiten Beispiel), seht ihr den Inhalt des Objekts als String. Wenn nötig, könnt ihr mehr über "debugging React-applications" lesen.

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

Event Handling hat sich letztes Jahr im Kurs als schwieriges Thema erwiesen.

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

Weil der Code, der die li-Tags generiert aus Javascript besteht, muss dieser in geschwungenen Klammern in einem JSX-Template stehen, wie auch anderer Javascript-Code.

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

Wie die in der Fehlermeldung verlinkte React-Seite vorschlägt, muss jedes Listenelement, z.B. die Elemente, die über die map-Methode generiert wurden, ein einzigartiges Attritbut "key" besitzen.

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

Die Array-Methode [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) zu verstehen, ist essenziel für den Rest dieses Kurses:

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

Unterbrechen wir einen Moment und untersuchen, wie map genau funktioniert.

> If the following code is added to, let's say, the end of the file:

Sagen wir, dass der folgende Code an die letzte Zeile angehängt wird:

```javascript
const result = notes.map(note => note.id)
console.log(result)
```

> [1, 2, 3] will be printed to the console. map always creates a new array, the elements of which have been created from the elements of the original array by mapping: using the function given as a parameter to the map method.

In der Konsole wird [1, 2, 3] ausgegeben. "map" erstellt immer ein neues Array. Dessen Elemente werden aus den Elementen des originalen Arrays über mapping erstellt, d.h. über die Funktion, die map als Parameter übergeben wird.

> The function is

Die Funktion lautet

```javascript
note => note.id
```

> Which is an arrow function written in compact form. The full form would be: 

Was eine Pfeilfunktion in Kurzform ist. Die vollständige Version würde so aussehen:

```javascript
(note) => {
  return note.id
}
```

> The function gets a note object as a parameter, and returns the value of its id field.

Die Funktion erhält ein note-Objekt als Parameter und gibt den Wert von dessen id-Feld zurück.

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

Das Benutzen von geschwungenen Klammern wird am Anfang für Kopfschmerzen sorgen, aber ihr werdet euch noch früh genug an sie gewöhnen. React gibt dafür direkt sichtbare Rückmeldungen aus.

## Anti-pattern: Array Indexes as Keys

> We could have made the error message on our console disappear by using the array indexes as keys. The indexes can be retrieved by passing a second parameter to the callback function of the map method: 

Wir hätten die Fehlermeldung in der Konsole auch verschwinden lassen können, indem wir die Indizes des Arrays als keys benutzen.

```javascript
notes.map((note, i) => ...)
```

> When called like this, i is assigned the value of the index of the position in the array where the note resides.

Wenn i so aufgerufen wird, wird i der Wert des Index von der Arrayposition, an dessen Position sich die Notiz befindet, zugewiesen.

> As such, one way to define the row generation without getting errors is:

So kann man auch die Liste generieren, ohne Fehlermeldungen zu erhalten:

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

Dieser Weg wird trotzdem nicht empfohlen und zu unerwünschten Problemen führen, auch wenn es gut zu funktionieren scheint.

Ihr könnt in diesem [Artikel](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318) mehr darüber lesen.

## Refactoring Modules

> Let's tidy the code up a bit. We are only interested in the field notes of the props, so let's retrieve that directly using destructuring: 

Räumen wir unseren Code ein bisschen auf. Uns interessiert nur das Feld "notes" der props, also holen wir sie uns direkt über "destructuring":

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

Wenn ihr vergessen habt, was "destructuring" bedeutet und wie es funktioniert, lest bitte erneut die Sektion über [destructuring](https://fullstackopen.com/en/part1/component_state_event_handlers#destructuring)

> We'll separate displaying a single note into its own component Note: 

Wir separieren die einzelnen Notizen in eigenen Komponenten:

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

Hinweis: Das Attribut "key" muss jetzt für den Komponenten Note defniert werden und nicht wie zuvor für die li-Tags.

> A whole React application can be written in a single file. Although that is, of course, not very practical. Common practice is to declare each component in their own file as an ES6-module.

Eine ganze React-Anwendung kann in eine einzige Datei geschrieben werden. Auch wenn das natürlich nicht sehr praktisch ist. Es ist üblich, jeden Komponenten in einer eigenen Datei als ein ES6-Modul zu definieren. 

> We have been using modules the whole time. The first few lines of the file index.js:

Wir haben die schon die ganze Zeit Module genutzt. Die ersten Zeilen der Datei index.js

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'
```

> import three modules, enabling them to be used in that file. The module React is placed into the variable React, the module react-dom into the variable ReactDOM, and the module that defines the main component of the app is placed into the variable App

[importieren](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) drei Module. Damit können die Module in dieser Datei verwendet werden. Das Modul React wird auf die Variable React festgelegt, das Modul react-dom auf die Variable ReactDOM und das Modul, das den Hauptkomponenten App definiert, auf die Variable App.

> Let's move our Note component into its own module.

Setzen wir den Komponenten Note in sein eigenes Modul.

> In smaller applications, components are usually placed in a directory called components, which is in turn placed within the src directory. The convention is to name the file after the component.

In kleineren Anwendungen sind Komponenten normalerweise in dem Verzeichnis "components", das selbst im Verzeichnis "src" ist. Die Konvention ist die Dateien jeweils nach den Komponenten zu benennen.

> Now, we'll create a directory called components for our application and place a file named Note.js inside. The contents of the Note.js file are as follows: 

Jetzt erstellen wir das Verzeichnis "components" für unsere Anwendung mit der Datei Note.js. Der Inhalt von Note.js sieht so aus:

```javascript
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

export default Note
```

> The last line of the module exports the declared module, the variable Note.

Die letzte Zeile des Moduls exportiert das Modul (die Variable Note).

> Now the file that is using the component - App.js - can import the module: 

Jetzt kann die Datei App.js, die den Komponenten verwendet, das Modul importieren.

```javascript
import Note from './components/Note'

const App = ({ notes }) => {
  // ...
}
```

> The component exported by the module is now available for use in the variable Note, just as it was earlier.

Der Komponent, der von dem Modul exportiert wird, ist jetzt durch die Variable Note verfügbar.

> Note that when importing our own components, their location must be given in relation to the importing file:

Bitte beachtet, dass beim Importieren eigener Komponenten, deren Verzeichnis relativ zur importierten Datei liegt:

```javascript
'./components/Note'
```

> The period - . - in the beginning refers to the current directory, so the module's location is a file called Note.js in the components sub-directory of the current directory. The filename extension .js can be omitted.

Der - . - am Anfang bezieht sich auf das aktuelle Verzeichnis, was bedeudet, dass sich das Modul in der Datei Note.js befindet, die selbst im Unterverzeichnis components im aktuellen Verzeichnis liegt. Die Dateinamenerweiterung .js kann weggelassen werden.

> Modules have plenty of other uses other than enabling component declarations to be separated into their own files. We will get back to them later in this course.

Module kann man für vieles verwenden, nicht nur für das Separieren von Komponenten in eigene Dateien. Wir kommen später in diesem Kurs noch darauf zurück.

> The current code of the application can be found on GitHub.

Der aktuelle Anwendungscode befindet sich auf [GitHub](https://github.com/fullstack-hy2020/part2-notes/tree/part2-1).

> Note that the main branch of the repository contains the code for a later version of the application. The current code is in the branch part2-1:

Bitte beachtet, dass Code von späteren Versionen der Anwendung im Branch Main enthalten sind. Der jetzige Code ist im Branch part2-1:

!["fullstack content"](./images/part2a_image2.png?raw=true)

> If you clone the project, run the command npm install before starting the application with npm start.

Wenn ihr das Projekt klonen wollt, führt den Befehl "npm install" aus, bevor ihr die Anwendung mit dem Befehl "npm start" startet.

## When the Application Breaks

> Early in your programming career (and even after 30 years of coding like yours truly), what often happens is that the application just completely breaks down. This is even more the case with dynamically typed languages, such as JavaScript, where the compiler does not check the data type. For instance, function variables or return values.

Am Beginn euer Programmiererkarriere (und auch nach 30 Jahrem professionellem Programmierens) wird es oft passieren, dass eure Anwendung einfach nicht funktioniert. Das ist vorallem bei dynamischen Sprachen wie Javascript der Fall, wo der Compiler keine Datentypen abfragt.

> A "React explosion" can, for example, look like this:

Eine "React-Explosion" kann z.B. so aussehen:

!["fullstack content"](./images/part2a_image3.png?raw=true)

> In these situations your best way out is the console.log method.

In einem solchen Fall hilft euch am besten der Befehl console.log.

> The piece of code causing the explosion is this:

Verantwortlich für die Explosion ist dieser Code:

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

Wir suchen nach dem Grund des Absturzes in dem console.log-Befehle in den Code setzen. Weil als erstes der Komponent App gerendert wird, setzen wir dort den ersten console.log-Befehl:

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

Um die Ausgabe in der Konsole zu sehen, müssen wir durch eine riesige rote Wand voller Fehler scrollen.

!["fullstack content"](./images/part2a_image4.png?raw=true)

> When one thing is found to be working, it's time to log deeper. If the component has been declared as a single statement, or a function without a return, it makes printing to the console harder.

Wenn die eine Sache zu funktionieren scheint, ist es an der Zeit woanders zu suchen. Wenn der Komponent als einziger Ausdruck definiert wird oder eine Funktion ohne Rückgabewert, ist die Ausgabe an die Konsole schwieriger:

```javascript
const Course = ({ course }) => (
  <div>
    <Header course={course} />
  </div>
)
```

> The component should be changed to its longer form in order for us to add the printing: 

Der Komponent sollte in seine längere Form geändert werden, um die Ausgabe an die Konsole hinzuzufügen:

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

Sehr oft sind die props eine Problem, von denen ein anderer Datentyp erwartet wird, oder sie werden mit einem anderen Namen, als sie eigentlich haben, aufgerufen und deswegen schlägt das "Destructuring" fehl. Meistens lösen sich die Probleme von selbst, wenn das Destructuring aufgehoben wird und wir tatsächlich sehen, was die props enthalten:

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

Wenn das Problem damit noch immer nicht behoben wurde, gibt es nicht wirklich viel zu tun abgesehen davon weiter nach Bugs zu suchen, indem ihr mehr console.log-Befehle in euren Code schreibt.

> I added this chapter to the material after the model answer for the next question exploded completely (due to props being of the wrong type), and I had to debug it using console.log.

Ich habe dieses Kapitel hinzugefügt, nachdem die Lösungsantwort für die nächste Frage "komplett explodiert" ist (weil die props den falschen Typ hatten) und ich mit console.log debuggen musste.

## Exercises

> The exercises are submitted via GitHub, and by marking the exercises as done in the submission system.

Die Aufgaben werden über GitHub eingereicht und müssen im Submissionsystem als erledigt markiert werden.

> You can submit all of the exercises into the same repository, or use multiple different repositories. If you submit exercises from different parts into the same repository, name your directories well.

Ihr könnt alle Aufgaben über ein Repository einreichen oder verschiedene Repositories benutzen. Bitte benennt eure Verzeichnisse korrekt, wenn ihr Aufgaben verschiedener Abschnitte im selben Repository einreicht. 

> The exercises are submitted One part at a time. When you have submitted the exercises for a part, you can no longer submit any missed exercises for that part.

Die Aufgaben werden Abschnitt für Abschnitt eingereicht. Wenn ihr eure Lösungen für einen Abschnitt eingereicht habt, könnt ihr keine weiteren Aufgaben für diesen Abschnitt einreichen.

> Note that this part has more exercises than the ones before, so do not submit before you have done all exercises from this part you want to submit.

Bitte beachtet, dass dieser Abschnitt mehr Aufgaben als die vorherigen hat, also ladet nichts hoch, bevor ihr nicht alle Aufgaben abgeschlossen habt, die ihr hochladen wollt.

> WARNING create-react-app makes the project automatically into a git-repository, if the project is not created inside of an already existing repository. You probably do not want the project to become a repository, so run the command rm -rf .git from its root. 

WARNUNG: create-react-app erstellt aus eurem Projekt ein git-Repository, es sei denn, ihr erstellt eure Anwendung in einem bereits bestehenden git-Repository. Sehr wahrscheinlich möchtet ihr nicht für jedes Projekt ein eigenes Repository, deswegen könnt ihr einfach den Befehlt rm -rf .git im Wurzelverzeichnis eurer Anwendung ausführen.

### 2.1 Course information step6

> Let's finish the code for rendering course contents from exercises 1.1 - 1.5. You can start with the code from the model answers. The model answers for part 1 can be found by going to the submission system, click on my submissions at the top, and in the row corresponding to part 1 under the solutions column click on show. To see the solution to the course info exercise, click on index.js under kurssitiedot ("kurssitiedot" means "course info").

Arbeiten wir an dem Code für das Anzeigen der Kursinhalte aus den Aufgaben 1.1 - 1.5 weiter. Ihr könnt mit dem Code der Modellantworten anfangen. Die Modellantworten für Abschnitt 1 findet ihr, indem ihr ins Submissionsystem geht, auf "my submissions" klickt und in der zugehörigen Reihe für Abschnitt 1 bei "solutions" auf "show" klickt. Um die Lösung für die Aufgabe "course info" zu sehen, müsst ihr auf "index.js" unter "kurssitiedot" klicken (kurssitiedot bedeutet course info).

> Note that if you copy a project from one place to another, you might have to delete the node_modules directory and install the dependencies again with the command npm install before you can start the application. Generally, it's not recommended that you copy a project's whole contents and/or add the node_modules directory to the version control system.

Bitte beachtet, dass, wenn ihr ein Projekt von einer Stelle zu einer anderen kopiert, ihr das Verzeichnis node_modules löscht und die Abhängigkeiten mit dem Befehl "npm install" installiert bevor ihr die Anwendung startet. Es wird im allgemeinen nicht empfohlen, dass man den kompletten Inhalt eines Projekts kopiert und/oder das Verzeichnis node_modules zur Versionskontrolle hinzufügt.

> Let's change the App component like so: 

Ändern wir den Komponenten App folgendermaßen ab:

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

Definiert einen Komponenten, der für das Anzeigen eines einzigen Kurses "Course" verantwortlich ist.

> The component structure of the application can be, for example, the following: 

Die Struktur der Anwendung könnte z.B. so aussehen:

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

Folglich enthält der Komponent Course die Komponenten, die im vorherigen Abschnitt definiert wurden und für das Anzeigen des Kursnamens und seiner Bestandteile verantwortlich sind.

> The rendered page can, for example, look as follows: 

Die Seite könnte zum Beispiel so aussehen:

!["fullstack content"](./images/part2a_image5.png?raw=true)

> You don't need the sum of the exercises yet.

Die Summe der Aufgaben ist jetzt noch nicht notwendig.

> The application must work regardless of the number of parts a course has, so make sure the application works if you add or remove parts of a course.

Die Anwendung muss unabhängig sein von der Anzahl der Abschnitte, die ein Kurs hat. Also stellt sicher, dass die Anwendung auch funktioniert, wenn ihr Abschnitte löscht oder hinzufügt.

> Ensure that the console shows no errors!

Stellt auch sicher, dass in der Konsole keine Fehler angezeigt werden.

### 2.2: Course information step7

> Show also the sum of the exercises of the course. 

Zeigt jetzt auch die Summe der Aufgaben an.

!["fullstack content"](./images/part2a_image6.png?raw=true)

### 2.3* Course information step8

> If you haven't done so already, calculate the sum of exercises with the array method reduce.

Wenn ihr es bis jetzt noch nicht getan habt, dann berechnet jetzt die Summe der Aufgaben mit der Array-Methode reduce.

> Pro tip: when your code looks as follows:

Profitipp: Wenn eurer Code so aussieht:

```javascript
const total = 
  parts.reduce((s, p) => someMagicHere)
```

> and does not work, it's worth to use console.log, which requires the arrow function to be written in its longer form:

und nicht funktioniert, lohnt es sich console.log zu nutzen, was voraussetzt, dass die Pfeilfunktion in ihrer Langform verwendet wird:

```javascript
const total = parts.reduce((s, p) => {
  console.log('what is happening', s, p)
  return someMagicHere 
})
```

> Not working? : Use your search engine to look up how reduce is used in an Object Array.

Funktioniert nicht? Nutzt eine Suchmaschine, um nachzusehen, wie reduce mit einem Array verwendet wird.

> Pro tip 2: There is a plugin for VS Code that automatically changes short form arrow functions into their longer form, and vice versa. 

Profitipp 2: Es gibt ein Plugin für VSCode, dass automatisch Pfeilfunktionen von ihrer Langform in ihre Kurzform und zurück verwandelt.

!["fullstack content"](./images/part2a_image7.png?raw=true)

### 2.4 Course information step9

> Let's extend our application to allow for an arbitrary number of courses:

Erweitern wir unsere Anwendung, um eine beliebige Zahl an Kursen zu erlauben:

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

Die Anwendung könnte zum Beispiel so aussehen:

!["fullstack content"](./images/part2a_image8.png?raw=true)

###  2.5: separate module

> Declare the Course component as a separate module, which is imported by the App component. You can include all subcomponents of the course into the same module. 

Definiert den Komponenten Course als abgetrenntes Modul, dass in den Komponenten App importiert wird. Ihr könnt alle Unterkomponenten des Kurses in das selbe Modul einfügen. 
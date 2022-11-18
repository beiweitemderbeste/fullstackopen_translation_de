# Abschnitt 2a

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

Bevor wir den neuen Abschnitt anfangen, schauen wir uns noch einige Themen an, die sich letztes Jahr als schwierig erwiesen haben.

## console.log

Was ist der Unterschied zwischen einem erfahrenen Javascriptprogrammierer und einem Anfänger? Der erfahrene Programmierer nutzt console.log 10-100 mal öfter.

Paradoxerweise scheint das wahr zu sein, obwohl ein Anfänger console.log (oder eine andere Art zu debuggen) öfter nutzen müsste als ein Profi.

Wenn etwas nicht funktioniert, ratet nicht, was falsch läuft. Benutzt stattdessen console.log oder etwas anderes zum Debuggen.

Hinweis: Wie wir es schon im ersten Abschnitt angesprochen haben, solltet ihr "nicht den Java weg gehen", wenn es um das Benutzen von console.log geht:

```javascript
console.log('props value is ' + props)
```

trennt die Elemente stattdessen mit einem Komma:

```javascript
console.log('props value is', props)
```

Wenn ihr ein Objekt mit einem String verbindet und es in der Konsole ausgebt (wie in unserem ersten Beispiel), nützt euch das Ergbnis nicht viel.

```javascript
props value is [object Object]
```

Wenn ihr im Gegensatz dazu Objekte als verschiedene, mit Komata separierte, Elemente in der Konsole ausgebt (wie in unserem zweiten Beispiel), seht ihr den Inhalt des Objekts als String. Wenn nötig, könnt ihr mehr über "debugging React-applications" lesen.

## Protip: Visual Studio Code snippets

Mit Visual Studio Code ist es einfach "snippets" zu erstellen, z.B. Tastenkürzel um schnell wiederbenutzbaren Code zu geneieren, ähnlich wie "sout" in Netbeans funktioniert.

Anleitungen, wie man Snippets erstellt, können [hier](hier) gefunden werden.

Vorgefertigte Snippets gibt es auch als VSCode plugins.

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

Mit console.log zu debuggen ist so verbreitet, dass VSCode dafür Snippets eingebaut hat. Um es zu benutzen, gebt log und drückt Tabulator für autocomplete. Mehr Erweiterungen für console.log()-Snippets findet ihr im marketplace von VSCode.

## JavaScript Arrays

Ab diesem Zeitpunkt werden wir die ganze Zeit Methoden der funktionialen Programmierung für Javascript [Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - wie "find", "filter" und "map" - benutzen. Wir arbeiten nach den gleichen allgemeinen Prinzipien wie Streams in Java 8. Diese wurden in den letzten Jahren in den beiden Kursen "Ohjelmoinnin perusteet" und "Ohjelmoinnin jatkokurssi" der Informatikfakultät und in den Programmier-MOOC benutzt.

Wenn sich funktionales Programmieren mit Arrays für euch fremd anfühlt, solltet ihr mindestens die ersten drei Teile der folgenden Youtube-Reihe [Functional Programming in JavaScript](https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84) anschauen:

- [higher-order functions](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84)
- [map](https://www.youtube.com/watch?v=bCqtb-Z5YGQ&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=2)
- [reduce basics](https://www.youtube.com/watch?v=Wl98eZpkp-c&t=31s)

## Event Handlers Revisited

Event Handling hat sich letztes Jahr im Kurs als schwieriges Thema erwiesen.

Wenn ihr noch Wissenslücken bei diesem Thema habt, empfehlen wir euch, nochmal das Thema [event handling revisited](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps#event-handling-revisited) es letztens Abschnitts zu lesen.

Wie man Event Handler an Unterkomponenten weitergibt, hat einige Fragen aufgeworfen. Eine kleine Wiederholung zu diesem Thema kann [hier](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps#passing-event-handlers-to-child-components) gefunden werden.

## Rendering Collections

Wir beginnen jetzt mit dem Frontend oder besser gesagt der browserseitigen Anwendungslogik in React für eine Anwendung, die vergleichbar mit der Beispielanwendung aus Abschnitt 0 ist.

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

Jede Notiz enthält einen Text, einen Zeitstempel, einen Boolean-Wert, um zu markieren, ob die Notiz als wichtig kategorisiert wurde und eine einzigartige ID.

Das obige Beispiel funktioniert, weil es exakt drei Notizen in dem Array gibt.

Eine einzige Notiz wird dargestellt, indem auf die Objekte im Array mit einer hartkodierten Index-Nummer referenziert wird.

```javascript
<li>{notes[1].content}</li>
```

Das ist natürlich nicht praktikabel. Verbessern wir den Code, indem wir React-Elemente aus den Array-Objekten über die Funktion map generieren:

```javascript
notes.map(note => <li>{note.content}</li>)
```

Das Ergbnis ist ein Array aus li-Elementen.

```javascript
[
  <li>HTML is easy</li>,
  <li>Browser can execute only JavaScript</li>,
  <li>GET and POST are the most important methods of HTTP protocol</li>,
]
```

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

Weil der Code, der die li-Tags generiert aus Javascript besteht, muss dieser in geschwungenen Klammern in einem JSX-Template stehen, wie auch anderer Javascript-Code.

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


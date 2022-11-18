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

## key-attribut

Obwohl die Anwendung zu funktionieren scheint, gibt es trotzdem eine ekelhafte Warnung in der Konsole:

!["fullstack content"](./images/part2a_image1.png?raw=true)

Wie die in der Fehlermeldung verlinkte React-Seite vorschlägt, muss jedes Listenelement, z.B. die Elemente, die über die map-Methode generiert wurden, ein einzigartiges Attritbut "key" besitzen.

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

Damit verschwindet die Fehlermeldung.

React benutzt die Key-Attribute der Objekte eines Arrays um festzulegen, wie die Ansicht eines Komponenten bei dessem erneuten Rendern generiert werden soll. Mehr dazu gibt es in der [React-Dokumentation](https://reactjs.org/docs/reconciliation.html#recursing-on-children).

## Map

Die Array-Methode [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) zu verstehen, ist essenziel für den Rest dieses Kurses:

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

Unterbrechen wir einen Moment und untersuchen, wie map genau funktioniert.

Sagen wir, dass der folgende Code an die letzte Zeile angehängt wird:

```javascript
const result = notes.map(note => note.id)
console.log(result)
```

In der Konsole wird [1, 2, 3] ausgegeben. "map" erstellt immer ein neues Array. Dessen Elemente werden aus den Elementen des originalen Arrays über mapping erstellt, d.h. über die Funktion, die map als Parameter übergeben wird.

Die Funktion lautet

```javascript
note => note.id
```

Was eine Pfeilfunktion in Kurzform ist. Die vollständige Version würde so aussehen:

```javascript
(note) => {
  return note.id
}
```

Die Funktion erhält ein note-Objekt als Parameter und gibt den Wert von dessen id-Feld zurück.

Ändern wir den Befehl so ab

```javascript
const result = notes.map(note => note.content)
```

erhalten wir ein Array, das die Inhalte der Notizen enthält.

Das ist schon sehr ähnlich zu dem React-Code, den wir benutzt haben:

```javascript
notes.map(note =>
  <li key={note.id}>{note.content}</li>
)
```

Dieser erstellt einen li-Tag mit den Inhalten einer Notiz für jedes note-Objekt.

Weil die Funktion, die map als Parameter übergeben bekommt, 

```javascript
note => <li key={note.id}>{note.content}</li>
```

genutzt wird, um view-Elemente zu erstellen, müssen die Werte der Variablen in geschwungene Klammern gesetzt werden. Schaut nach, was passiert, wenn ihr diese weglasst.

Das Benutzen von geschwungenen Klammern wird am Anfang für Kopfschmerzen sorgen, aber ihr werdet euch noch früh genug an sie gewöhnen. React gibt dafür direkt sichtbare Rückmeldeldungen.

Das Benutzen von geschwungenen Klammern wird am Anfang für Kopfschmerzen sorgen, aber ihr werdet euch noch früh genug an sie gewöhnen. React gibt dafür direkt sichtbare Rückmeldungen aus.

## Anti-pattern: Array Indexes as Keys

Wir hätten die Fehlermeldung in der Konsole auch verschwinden lassen können, indem wir die Indizes des Arrays als keys benutzen.

```javascript
notes.map((note, i) => ...)
```

Wenn i so aufgerufen wird, wird i der Wert des Index von der Arrayposition, an dessen Position sich die Notiz befindet, zugewiesen.

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

Dieser Weg wird trotzdem nicht empfohlen und zu unerwünschten Problemen führen, auch wenn es gut zu funktionieren scheint.

Ihr könnt in diesem [Artikel](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318) mehr darüber lesen.

## Refactoring Modules

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

Wenn ihr vergessen habt, was "destructuring" bedeutet und wie es funktioniert, lest bitte erneut die Sektion über [destructuring](https://fullstackopen.com/en/part1/component_state_event_handlers#destructuring)

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

Hinweis: Das Attribut "key" muss jetzt für den Komponenten Note defniert werden und nicht wie zuvor für die li-Tags.

Eine ganze React-Anwendung kann in eine einzige Datei geschrieben werden. Auch wenn das natürlich nicht sehr praktisch ist. Es ist üblich, jeden Komponenten in einer eigenen Datei als ein ES6-Modul zu definieren. 

Wir haben die schon die ganze Zeit Module genutzt. Die ersten Zeilen der Datei index.js

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'
```

[importieren](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) drei Module. Damit können die Module in dieser Datei verwendet werden. Das Modul React wird auf die Variable React festgelegt, das Modul react-dom auf die Variable ReactDOM und das Modul, das den Hauptkomponenten App definiert, auf die Variable App.

Setzen wir den Komponenten Note in sein eigenes Modul.

In kleineren Anwendungen sind Komponenten normalerweise in dem Verzeichnis "components", das selbst im Verzeichnis "src" ist. Die Konvention ist die Dateien jeweils nach den Komponenten zu benennen.

Jetzt erstellen wir das Verzeichnis "components" für unsere Anwendung mit der Datei Note.js. Der Inhalt von Note.js sieht so aus:

```javascript
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

export default Note
```

Die letzte Zeile des Moduls exportiert das Modul (die Variable Note).

Jetzt kann die Datei App.js, die den Komponenten verwendet, das Modul importieren.

```javascript
import Note from './components/Note'

const App = ({ notes }) => {
  // ...
}
```

Der Komponent, der von dem Modul exportiert wird, ist jetzt durch die Variable Note verfügbar.

Bitte beachtet, dass beim Importieren eigener Komponenten, deren Verzeichnis relativ zur importierten Datei liegt:

```javascript
'./components/Note'
```

Der - . - am Anfang bezieht sich auf das aktuelle Verzeichnis, was bedeudet, dass sich das Modul in der Datei Note.js befindet, die selbst im Unterverzeichnis components im aktuellen Verzeichnis liegt. Die Dateinamenerweiterung .js kann weggelassen werden.

Module kann man für vieles verwenden, nicht nur für das Separieren von Komponenten in eigene Dateien. Wir kommen später in diesem Kurs noch darauf zurück.

Der aktuelle Anwendungscode befindet sich auf [GitHub](https://github.com/fullstack-hy2020/part2-notes/tree/part2-1).

Bitte beachtet, dass Code von späteren Versionen der Anwendung im Branch Main enthalten sind. Der jetzige Code ist im Branch part2-1:

!["fullstack content"](./images/part2a_image2.png?raw=true)

Wenn ihr das Projekt klonen wollt, führt den Befehl "npm install" aus, bevor ihr die Anwendung mit dem Befehl "npm start" startet.

## When the Application Breaks

Am Beginn euer Programmiererkarriere (und auch nach 30 Jahrem professionellem Programmierens) wird es oft passieren, dass eure Anwendung einfach nicht funktioniert. Das ist vorallem bei dynamischen Sprachen wie Javascript der Fall, wo der Compiler keine Datentypen abfragt.

Eine "React-Explosion" kann z.B. so aussehen:

!["fullstack content"](./bilder/abschnitt2a_bild3.png?raw=true)

In einem solchen Fall hilft euch am besten der Befehl console.log.

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

Um die Ausgabe in der Konsole zu sehen, müssen wir durch eine riesige rote Wand voller Fehler scrollen.

!["fullstack content"](./bilder/abschnitt2a_bild4.png?raw=true)

Wenn die eine Sache zu funktionieren scheint, ist es an der Zeit woanders zu suchen. Wenn der Komponent als einziger Ausdruck definiert wird oder eine Funktion ohne Rückgabewert, ist die Ausgabe an die Konsole schwieriger:

```javascript
const Course = ({ course }) => (
  <div>
    <Header course={course} />
  </div>
)
```

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

Wenn das Problem damit noch immer nicht behoben wurde, gibt es nicht wirklich viel zu tun abgesehen davon weiter nach Bugs zu suchen, indem ihr mehr console.log-Befehle in euren Code schreibt.

Ich habe dieses Kapitel hinzugefügt, nachdem die Lösungsantwort für die nächste Frage "komplett explodiert" ist (weil die props den falschen Typ hatten) und ich mit console.log debuggen musste.

## Exercises

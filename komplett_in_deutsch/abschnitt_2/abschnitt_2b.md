-# Abschnitt 2b

## Inhaltsverzeichnis

- [controlled component](#controlled-component)
- [filtering displayed elements](#filtering-displayed-elements)
- [exercises](#exercises)

Erweitern wir unsere Anwendung, indem wir Benutzern erlauben neue Notizen hinzuzufügen. Ihr könnt den Code der jetzigen Anwendung [hier](https://github.com/fullstack-hy2020/part2-notes/tree/part2-1) einsehen.

Damit sich unsere Seite aktualisiert, wenn eine neue Notiz angehängt wird, ist es am besten die Notizen im State des Komponenten App zu speichern. Importieren wir die Funktion [useState](https://reactjs.org/docs/hooks-state.html) und nutzen sie, um einen State zu definieren, der initialisiert wird, wenn das anfängliche notes-Array an die props übergeben wird.

```javascript
import { useState } from 'react'
import Note from './components/Note'

const App = (props) => {  
  const [notes, setNotes] = useState(props.notes)

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

export default App 
```

Der Komponent verwendet die Funktion useState, um den State zu initialisieren, der in "notes" gespeichert ist. Das Array der "notes" wird dabei als props übergeben:

```javascript
const App = (props) => { 
  const [notes, setNotes] = useState(props.notes) 

  // ...
}
```

!["fullstack content"](./bilder/abschnitt2b_bild9.png?raw=true)

Wenn wir mit einer leeren Liste von Notizen anfangen wollten, würden wir den Startwert auf ein leeres Array setzen und da die props hier nicht benötigt werden, könnten wir den Parameter props in der Funktionsdefinition weglassen:

```javascript
const App = () => { 
  const [notes, setNotes] = useState([]) 

  // ...
}  
```

Wir starten aber vorläufig mit den props als Startwert.

Als nächstes erweitern wir den Komponenten um ein [HTML-Formular](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms), das für das Hinzufügen neuer Notizen genutzt wird.

```javascript
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)

  const addNote = (event) => {    
    event.preventDefault()   
    console.log('button clicked', event.target)  
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>        
        <input />        
        <button type="submit">save</button>      
      </form>       
    </div>
  )
}
```

Wir haben die Funktion addNote als Event Handler an das Formular gefügt. Diese wird aufgerufen, wenn das Formular über den submit-Button abgeschickt wird.

Wir definieren unseren Event Handler wie in Abschnitt 1:

```javascript
const addNote = (event) => {
  event.preventDefault()
  console.log('button clicked', event.target)
}
```

Der Parameter event ist das [Ereignis](https://reactjs.org/docs/handling-events.html), das den Aufruf der Event Handler-Funktion auslöst.

Der Event Handler ruft unmittelbar die Funktion event.preventDefault() auf, die die Standardaktion verhindert: das Abschicken des Formulars. Die Standardaktion würde [u.a.](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event) das Neuladen der Seite bewirken.

Das Ziel des Ereignisses, dass in event.target gespeichert ist, wird in der Konsole ausgegeben:

!["fullstack content"](./bilder/abschnitt2b_bild1.png?raw=true)

In diesem Fall ist das Ziel das Formular, das wir in unserem Komponenten definiert haben.

Wie können wir auf die Daten zugreifen, die im Eingabefeld des Formulars gespeichert sind?

## Controlled component

Es gibt viele Wege, um das zu erreichen. Den ersten Weg, den wir uns ansehen, ist das Benutzen von sogenannten [controlled components](https://reactjs.org/docs/forms.html#controlled-components).

```javascript
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState('a new note...')

  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => <Note key={note.id} note={note} />)}
      </ul>
      <form onSubmit={addNote}>
        <input value={newNote} />        
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```

Der Platzhaltertext, der als Startwert des States von newNote gespeichert ist, erscheint im Eingabefeld, aber der Eingabetext kann nicht bearbeitet werden. Die Konsole zeigt eine Warnung an, die uns darüber informiert, was falsch laufen könnte:

!["fullstack content"](./bilder/abschnitt2b_bild2.png?raw=true)

Da wir einen Teil des States des Komponenten App dem Wert des Eingabefelds zugewiesen haben, [steuert](https://reactjs.org/docs/forms.html#controlled-components) der Komponent App nun das Verhalten des Eingabefelds.

Um das Eingabefeld bearbeiten zu können, registrieren wir einen Event Handler, der Veränderungen an der Eingabe mit dem State des Komponenten synchronisiert:

```javascript
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  ) 

  // ...

  const handleNoteChange = (event) => {    
    console.log(event.target.value)    
    setNewNote(event.target.value)  
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input
          value={newNote}
          onChange={handleNoteChange}        
        />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```

Jetzt haben einen Event Handler für das Attribut onChange registriert:

```javascript
<input
  value={newNote}
  onChange={handleNoteChange}
/>
```

Der Event Handler wird jedesmal aufgerufen, wenn es eine Änderung im Eingabefeld gibt. Die Funktion des Event Handlers erhält das Event-Objekt als event-Parameter:

```javascript
const handleNoteChange = (event) => {
  console.log(event.target.value)
  setNewNote(event.target.value)
}
```

Das Ziel des Event-Objekts stimmt jetzt mit dem Eingabefeld überein und event.target.value bezieht sich auf den Eingabewert des Elements.

Beachtet, dass wir die Funktion event.preventDefault() nicht aufrufen mussten, wie wir es beim Event Handler onSubmit getan haben, da es keine Standardaktion für die Änderung des Eingabefelds gibt, im Gegensatz zum Abschicken eines Formulars.

Jetzt könnt ihr in der Konsole verfolgen, wie der Event Handler aufgerufen wird:

!["fullstack content"](./bilder/abschnitt2b_bild3.png?raw=true)

Ihr erinnert euch, dass ihr die [React Devtools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) installiert habt? Gut. Ihr könnt in den React Devtools direkt sehen, wie sich der State verändert:

!["fullstack content"](./bilder/abschnitt2b_bild4.png?raw=true)

 Jetzt spiegelt der State newNote des Komponenten App den aktuellen Wert der Eingabe, was bedeudet, dass wir die Funktion addNote für das Erstellen neuer Notizen vervollständigen können:

```javascript
const addNote = (event) => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    date: new Date().toISOString(),
    important: Math.random() < 0.5,
    id: notes.length + 1,
  }

  setNotes(notes.concat(noteObject))
  setNewNote('')
}
```

Zuerst erstellen wir für die Notiz ein neues Objekt noteObject, das seinen Inhalt vom State newNote des Komponenten erhält. Der einzigartige Identifier id wird auf Basis der Gesamtzahl der Notizen generiert. Das reicht für unsere Anwendung, da Notizen niemals gelöscht werden. Mithilfe der Funktion Math.random() hat unsere Notiz eine 50%ige Chance als wichtig markiert zu werden.

Die neue Notiz wird mit der Array-Funktion [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat), die in Abschnitt 1 eingeführt wurde, an die Notizenliste gehängt:

```javascript
setNotes(notes.concat(noteObject))
```
Diese Funktion verändert nicht das originale Array notes, sondern erstellt eine neue Kopie des Arrays mit einem neuen Element, dass am Ende angefügt wird. Das ist wichtig, da wir in React [niemals State direkt verändern](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly) dürfen!

Der Event Handler setzt ebenso den Wert des Eingabefelds zurück, in dem die Funktion setNewNote aufgerufen wird:

```javascript
setNewNote('')
```

Ihr finden den Quellcode für unsere aktuelle Anwendug in seiner Gesamtheit im Branch part2-2 von [diesem GitHub Repository](https://github.com/fullstack-hy2020/part2-notes/tree/part2-2).

## Filtering Displayed Elements

Erweitern wir unsere Anwendung um eine Funktionalität, die es uns erlaubt, nur die wichtigen Notizen zu sehen.

Wir erweitern unseren Komponenten App um einen State, der verfolgt, welche Notizen angezeigt werden sollen.

```javascript
const App = (props) => {
  const [notes, setNotes] = useState(props.notes) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)  

  // ...
}
```

Verändern wir den Komponenten so, dass er eine Liste mit allen Notizen, die angezeigt werden sollen, in der Variable notesToShow speichert:

```javascript
import { useState } from 'react'
import Note from './components/Note'

const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState('') 
  const [showAll, setShowAll] = useState(true)

  // ...

  const notesToShow = showAll    
  ? notes    
  : notes.filter(note => note.important === true)

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notesToShow.map(note =>          
          <Note key={note.id} note={note} />
        )}
      </ul>
      // ...
    </div>
  )
}
```

Die Definition der Variable notesToShow is recht eher kompakt:

```javascript
const notesToShow = showAll
  ? notes
  : notes.filter(note => note.important === true)
```

Die Definition benutzt den [Conditional](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) Operator, den es auch in vielen anderen Programmiersprachen gibt.

Der Operator funktioniert wie folgt:

```javascript
const result = condition ? val1 : val2
```

Die Variable result wird auf den Wert von val1 gesetzt, wenn die Bedingung wahr ist. Ist die Bedingung falsch, wird die Variable result auf den Wert von val2 gesetzt.

Wenn der Wert von showAll false ist, wird der Variable notesToShow eine Liste zugewiesen, die nur Notizen enthält, bei denen die Eigenschaft important auf true gesetzt wurde. Das Filtern geschieht mit Hilfe der Array-Methode [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

```javascript
notes.filter(note => note.important === true)
```

Der Vergleichsoperater ist tatsächlich redundant, da der Wert von note.important entweder true oder false ist, was bedeudet, das wir folgendermaßen verkürzen können:

```javascript
notes.filter(note => note.important)
```

Der Grund, warum wir den Vergleichsoperatur zuerst gezeigt haben, ist um ein wichtiges Detail hervorzuheben: in JavaScript funktioniert val1 == val2 nicht in allen Situation so, wie man es erwartet und es ist sicherer ausschließlich val1 === val2 bei Vergleichen zu verwenden. [Hier](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) könnt ihr mehr darüber lesen.

Ihr könnt die Filterfunktionalität testen, in dem ihr den Startwert des States showAll verändert.

Als nächstes fügen wir eine Funkionalität hinzu, die es Nutzern erlaubt, den State showAll der Anwendung im User Interface ein- und auszuschalten.

Die relevanten Änderungen werden hier gezeigt:

```javascript
import { useState } from 'react' 
import Note from './components/Note'

const App = (props) => {
  const [notes, setNotes] = useState(props.notes) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  // ...

  return (
    <div>
      <h1>Notes</h1>
      <div>        
        <button onClick={() => setShowAll(!showAll)}>         
          show {showAll ? 'important' : 'all' }        
        </button>      
      </div>      
      <ul>
        {notesToShow.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      // ...    
    </div>
  )
}
```

Die angezeigten Notizen (all oder important) werden mit einem Button kontrolliert. Der Event Handler des Buttons ist so einfach, dass er direkt als Attribut des Buttons definiert wurde. Der Event Handler schaltet den Wert von showAll von true zu false und zurück:

```javascript
() => setShowAll(!showAll)
```

Der Text des Buttons hängt vom Wert des States von showAll ab:

```javascript
show {showAll ? 'important' : 'all'}
```

Ihr findet den Quellcode für unsere aktuelle Anwendung in seiner Gesamtheit im Branch part2-3 von [diesem GitHub Repository](https://github.com/fullstack-hy2020/part2-notes/tree/part2-3).

## exercises

In der ersten Aufgabe beginnen wir an einer Anwendung zu arbeiten, die in späteren Aufgaben weiterentwickelt wird. Bei Aufgaben, die sich aufeinander beziehen, reicht es aus die endgültige Version der Anwendung abzugeben. Ihr könnt auch für jeden Teil der Aufgaben einen eigenen Commit erstellen, aber das ist nicht erforderlich.

WARNUNG: create-react-app macht aus eurem Projekt automatisch ein git-Repository, außer ihr habt eure Anwendung in einem bereits bestehendem git-Repository erstellt. Voraussichtlich wollt ihr das nicht tun, also führt einfach den Befehl "rm -rf .git" im Wurzelverzeichnis eurer Anwendung.

### 2.6: The Phonebook Step1

Erstellen wir ein einfaches Telefonbuch. In diesem Teil werden wir nur Namen hinzufügen.

Fangen wir an, in dem wir das Hinzufügen einer Person zum Telefonbuch implementieren.

Ihr könnt den folgenden Code als Ausgangspunkt für den Komponenten App benutzen:

```javascript
import { useState } from 'react'

const App = () => {
  const [persons, setPersons] = useState([
    { name: 'Arto Hellas' }
  ]) 
  const [newName, setNewName] = useState('')

  return (
    <div>
      <h2>Phonebook</h2>
      <form>
        <div>
          name: <input />
        </div>
        <div>
          <button type="submit">add</button>
        </div>
      </form>
      <h2>Numbers</h2>
      ...
    </div>
  )
}

export default App
```

Der State newName ist für das Kontrollieren des Eingabefelds des Formulars gedacht.

Manchmal kann es zum Debuggen nützlich sein, sich den State oder andere Variablen als Text anzusehen. Ihr könnt temporär das folgende Element im Komponenten hinzufügen:

```javascript
<div>debug: {newName}</div>
```

Es ist auch wichtig, das einzusetzen, was wir im Kapitel über das Debuggen von React-Anwendungen gelernt. Insbesondere die React Developer Tools sind unglaublich nützlich, um Änderung des States der Anwendung zu verfolgen.

Wenn ihr die Aufgabe erledigt habt, sollte eure Anwendung ungefähr so aussehen:

!["fullstack content"](./bilder/abschnitt2b_bild5.png?raw=true)

Beachtet das Benutzen der React Developer Tools im oberen Bild.

Hinweis:

- Ihr könnt den Namen einer Person als Wert für den Key benutzen.
- Denkt daran, die Standardaktion für das Abschicken von HTML-Formularen zu verhindern.

### 2.7: The Phonebook Step2

Verhindert, dass Benutzer Namen hinzufügen können, die es bereits im Telefonbuch gibt. JavaScript Arrays haben mehrere passende Funktionen, um diese Aufgabe zu erledigen.

Erzeugt eine Warnung mit dem Befehl alert, wenn sowas versucht wird:

!["fullstack content"](./bilder/abschnitt2b_bild6.png?raw=true)

Tipp: Wenn ihr Strings erstellt, die die Werte von Variablen enthalten, wird empfohlen Template Strings zu benutzen:

```javascript
`${newName} is already added to phonebook`
```

Wenn die Variable newName den Wert Arto Hellas hält, gibt der Template Strings den String zurück

```javascript
`Arto Hellas is already added to phonebook`
```

Das gleiche könnte auch mit der Java-ähnlich Methode mit dem Plus-Operater erreicht werden:

```javascript
newName + ' is already added to phonebook'
```

Das Benutzen von Template String ist die eher idiomatischere Option und das Zeichen eines wahren Javascript-Profis.

### 2.8: The Phonebook Step3

Erweitert eure Anwendung, in dem ihr es Benutzern erlaubt, Telefonnummern zum Telefonbuch hinzuzufügen. Ihr wert ein zweites Eingabefeld im Formular benötigen (zusammen mit seinem Event Handler):

```javascript
<form>
  <div>name: <input /></div>
  <div>number: <input /></div>
  <div><button type="submit">add</button></div>
</form>
```

Zu diesem Zeitpunkt könnte die Anwendung ungefähr so aussehen. Das Bild zeigt auch den State der Anwendung mithilfe der React Developer Tools:

!["fullstack content"](./bilder/abschnitt2b_bild7.png?raw=true)

### 2.9*: The Phonebook Step4

Implementiert ein Suchfeld, das genutzt werden kann, um die Liste der Leute nach Namen zu sortieren:

!["fullstack content"](./bilder/abschnit2b_bild8.png?raw=true)

Ihr könnt das Suchfeld als Eingabefeld, das außerhalb des Formulars platziert wird, implementieren.

Hinweis: Wenn ihr an einer neuen Funktionalität arbeitet, ist es oft nützlich Testdaten in eure Anwendung zu schreiben, z.B. so:

```javascript
const App = () => {
  const [persons, setPersons] = useState([
    { name: 'Arto Hellas', number: '040-123456', id: 1 },
    { name: 'Ada Lovelace', number: '39-44-5323523', id: 2 },
    { name: 'Dan Abramov', number: '12-43-234345', id: 3 },
    { name: 'Mary Poppendieck', number: '39-23-6423122', id: 4 }
  ])

  // ...
}
```

Das rettet euch davor, jedes Mal manuell Daten in eure Anwendung eingeben zu müssen, um die Anwendung zu testen.

### 2.10: The Phonebook Step5

Schreibt eure Anwendung um, wenn ihr sie als einzelnen Komponenten implementiert habt, indem ihr passende Teile in eigene Komponenten packt. Verwaltet den State der Anwendung und alle Event Handler im Komponenten App.

Es reicht aus, wenn ihr drei weitere Komponenten erstellt. Gute Kandidaten dafür wären z.B. der Suchfilter, das Formular für das Erstellen neuer Einträge, ein Komponent, der alle Einträge des Telefonbuchs anzeigt und ein Komponenten, der die Details einer einzelnen Person anzeigt.

Der Komponent App könnte nach dem Umschreiben ungefähr so aussehen.  Der untenstehende Komponent zeigt nur die Titel an und lässt die neuen Komponenten sich um den Rest kümmern.

```javascript
const App = () => {
  // ...

  return (
    <div>
      <h2>Phonebook</h2>

      <Filter ... />

      <h3>Add a new</h3>

      <PersonForm 
        ...
      />

      <h3>Numbers</h3>

      <Persons ... />
    </div>
  )
}
```

Hinweis: Ihr könntet Probleme bei dieser Aufgabe bekommen, wenn ihr eure Komponenten an der falschen Stelle definiert. Jetzt wäre ein guter Zeitpunkt, um das Kapitel "do not define a component in another component" des letzten Abschnitts zu wieder holen.
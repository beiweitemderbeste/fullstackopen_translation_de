# Abschnitt 1c

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

Arbeiten wir weiter mit React.

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

Die Logik hinter dem Erraten des Geburtsjahres steckt in einer eigenen Funktion, die aufgerufen wird, wenn der Komponent gerendert wird.

Das Alter der Person muss nicht als Parameter an die Funktion übergeben werden, da die Funktion direkten Zugang zu allen props bekommt, die an den Komponenten übergeben werden.

Wenn wir unseren Code genauer anschauen, bemerken wir, das die Hilfsfunktion eigentlich in einer anderen Funktion definiert ist, die das Verhalten unseres Komponenten definiert. Wenn man mit Java programmiert ist die Definition einer Funktion innerhalb einer anderen Funktion komplex und mühsam, also nicht allzu gewöhnlich, während es in Javascript sehr gebräuchlich ist.

## Destructuring

Bevor wir weitermachen, schauen wir uns noch ein kleines aber nützliches Feature von Javascript an, das mit der ES6-Spezifikation hinzugefügt wurde: das [Destrukturieren](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) von Teilen von Objekten und Arrays und ihr gleichzeitiges Zuweisen zu Variablen.

In unserem vorangegangenen Code haben wir auf die Daten, die an unseren Komponenten übergeben wurden, mit props.name und props.age verwiesen. Von diesen beiden Ausdrücken mussten wir props.age zweimal verwenden.

Da props ein Objekt ist

```javascript
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

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

Hinweis: Wir haben die kompaktere Syntax von Pfeilfunktionen benutzt, um die Funktion bornYear zu definieren. Wie bereits erwähnt, muss der Körper der Funktion nicht in geschweiften Klammen stehen, wenn die Pfeilfunktion aus nur einem Ausdruck besteht. In dieser kompakteren Form gibt die Funktion lediglich das Ergebnis des einzigen Ausdrucks aus.

Zur Wiederholung: die zwei folgenden Funktionsdefinitionen sind equivalent:

```javascript
const bornYear = () => new Date().getFullYear() - age

const bornYear = () => {
  return new Date().getFullYear() - age
}
```

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

```javascript
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

dann weist der Ausdruck const { name, age } = props die Werte "Arto Hellas" an "name" und "35" an "age" zu.

Wir können mit dem Destrukturieren auch einen Schritt weitergehen:

```javascript
const Hello = ({ name, age }) => {  
  const bornYear = () => new Date().getFullYear() - age

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

Die props, die an den Komponenten weitergegeben werden, werden jetzt direkt in die Variablen "name" und "age" destrukturiert.

Das bedeutet, das anstatt das ganze props-Objekts einer Variablen "props" zugewiesen wird, seine Eigenschaften an die Variablen "name" und "age" zugewiesen werden

```javascript
const Hello = (props) => {
const { name, age } = props
```

Weisen wir die Werte der Eigenschaft direkt den Variablen zu, indem wir das props-Objekt, das an den Komponeten als Parameter übergeben wird, destrukturieren:

```javascript
const Hello = ({ name, age }) => {
```

## Page re-rendering

Bis jetzt waren alle unsere Anwendungen in ihrer Erscheinung gleich, keine veränderte sich nach dem ersten Rendern. Was aber, wenn wir einen Zähler verwenden wollen, dessen Zählerstand sich bei einem Klick auf einen Button oder einer Funktion nach Zeit erhöht?

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

Der Komponent App bekommt den Wert des Zählers über die prop "counter". Dieser Komponent zeigt den Wert auf dem Bildschirm an. Was passiert, wenn sich der Zählerstand ändert? Was wenn wir das folgende hinzufügen?

```javascript
counter += 1
```

Der Komponent wird nicht erneut gerendert. Wir bekommen den Komponenten dazu, indem wir die Methode render ein zweites Mal aufrufen, z.B. so:

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

Der Befehl zum erneuten rendern wurde in die Funktion refresh eingebaut, um zu verhindern, dass zuviel Code hin und her kopiert wird.

Jetzt wird der Komponent dreimal gerendert, das erste Mal mit dem Wert 1, dann 2 und schließlich 3. Allerdings werden die Werte 1 und 2 nur so kurz am Bildschirm ausgegeben, dass man es nicht bemerken kann.

Wir können eine leicht interessantere Funktionalität einbauen, indem wir den Zähler mit jeder Sekunde erneut rendern und erhöhen. Dafür nutzen wir [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval):

```javascript
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

Erneute Aufrufe der Methode render ist nicht der empfohlene Weg, um in React Komponenten erneut zu rendern. Als nächstes führen wir eine bessere Möglichkeit ein, um das zu erreichen.

## Stateful component

Bis jetzt waren unsere Komponenten sehr einfach gestaltet, d.h. sie enthielten keinen State, der sich im Lebenszyklus eines Komponenten ändern könnte.

Als nächste erweitern wir unseren Komponenten App mithilfe des [State-Hooks](https://reactjs.org/docs/hooks-state.html) um einen State.

Ändert die Anwendung wie folgt ab: index.js wird zu

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

und App.js ändert sich so

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

In der ersten Zeile importiert die Datei die Funktion useState:

```javascript
import { useState } from 'react'
```

Der Körper der Funktion, der den Komponenten definiert, beginnt mit einem Funktionsaufruf:

```javascript
const [ counter, setCounter ] = useState(0)
```

Der Funktionsaufruf fügt einen State an den Komponenten und zeigt ihn mit einem Anfangswert von 0 an. Die Funktion gibt ein Array mit 2 Elementen zurück. Diese weisen wir den Variablen counter und setCounter zu, indem wir die Destrukturierung aus den vorangegangenen Kapiteln nutzen.

Der Variablen counter wird der Startwert von State (der 0 ist) zugewiesen. Der Variablen setCounter wird eine Funktion zugewiesen, die dafür genutzt wird den State zu ändern.

Die Anwendung ruft die Funktion [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) auf und übergibt ihr zwei Parameter: eine Funktion, um den Zählerstand zu erhöhen und einen Timeout von einer Sekunde:

```javascript
setTimeout(
  () => setCounter(counter + 1),
  1000
)
```

Die Funktion, die die Funktion setTimeout als ersten Parameter übergeben bekommt, wird eine Sekunde nachdem Funtionsaufruf von setTimeout gestartet.

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

Wenn die Komponentenfunktion das zweite Mal ausgeführt wird, ruft sie die Funktion useState auf und gibt den neuen Wert von State aus: 1. Das erneute Ausführen des Funktionskörpers des Komponenten, führt zu einem zweiten Timeout und erhöht den Zählerstand erneut. Weil der Wert der Zählervariable 1 ist, ist es grundlegend dasselbe, wie ein Ausdruck, der den Wert des Zählerstands auf 2 setzt.

```javascript
() => setCounter(2)
```

Währenddessen wird der alte Wert vom Zähler - "1" - am Bildschirm ausgegeben.

Jedes Mal, wenn setCounter den State verändert, wird dadurch ein erneutes Rendern des Komponenten erzeugt. Der Wert des States wird wieder nach einer Sekunde erhöht, was dazuführt das alles wiederholt wird, solange die Anwendung läuft.

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

Somit lassen sich die Funktionsaufrufe des Komponenten App leicht folgen:

!["fullstack content"](./bilder/abschnitt1c_bild1.png?raw=true)

## Event Handling

Wir haben bereits über Event Handlers in Abschnitt 0 gesprochen, sie werden aufgerufen, wenn spezielle Ereignisse gestartet werden sollen, z.B. können Benutzerinteraktionen mit den verschiedenen Elementen einer Webseite eine ganze Reihe von Ergeignissen starten.

Ändern wir die Anwendung so ab, dass der Zählerstand sich erhöht, wenn ein Benutzer auf einen [Button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) klickt.

Buttons unterstützen sogenannte [mouse events](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent) von denen der [click](https://developer.mozilla.org/en-US/docs/Web/Events/click) der gebräuchlichste ist. Abgesehen vom Namen mouse events kann der Klick auf einen Button auch durch einen Tastendruck oder Touchscreen ausgelöst werden.

In React sieht die [Erstellung eines Event Handlers](https://reactjs.org/docs/handling-events.html) für ein Klick-Event ungefähr so aus: 

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

Wir setzten den Wert vom Attribut onClick auf einen Verweis auf die Funktion handleClick, die im Code definiert wird.

Jetzt führt jeder Klick auf den Button plus dazu, dass die Funktion handleClick ausgeführt wird. Das heißt jeder Klick schreibt eine Nachricht in die Browserkonsole.

Die event handler-Funktion kann auch direkt im Attribut onClick definiert werden:

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

Wenn wir nun den Event Handler so abändern

```javascript
<button onClick={() => setCounter(counter + 1)}>
  plus
</button>
```

erhalten wir das gewünschte Verhalten, dass sich der Zählerstand um 1 erhöht und der Komponent erneut gerendert wird.

Es gibt auch einen Button, um den Zähler zurückzusetzen:

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

Jetzt ist unsere Anwendung bereit!

## Event handler is a function

Wir definieren die Event Handler für unsere Buttons dort, wo wir ihre onClick-Attribute vergeben:

```javascript
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

Was, wenn wir versuchen würden, die Event Handler in einer einfacheren Form zu definieren?

```javascript
<button onClick={setCounter(counter + 1)}> 
  plus
</button>
```

Das würde erstmal zu sehr vielen Fehlermeldungen führen:

!["fullstack content"](./images/part1c_image2.png?raw=true)

Was passiert hier? Ein Event Handler sollte entweder eine Funktion oder ein Verweis auf eine Funktion sein, aber wenn wir so etwas schreiben

```javascript
<button onClick={setCounter(counter + 1)}>
```

ist der Event Handler eigentlich ein Funktionsaufruf. In vielen Situationen ist das in Ordnung, aber nicht in dieser besonderen Situation. Am Anfang ist der Wert der Variable counter 0. Wenn React den Komponenten das erste Mal rendert, ruft es die Funktion setCounter(0+1), und ändert damit den Wert des States des Komponenten zu 1. Das erzeugt ein erneutes rendern des Komponenten, dadurch ruft React wieder die Funktion setCounter auf und der State ändert sich erneut...

Definieren wir die Event Handler wir vorhin:

```javascript
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

Jetzt wird das Buttonattribut onClick, das definiert was passiert, wenn der Button geklickt wird, auf () => setCounter(counter + 1) gesetzt.

Normalerweise ist das Definieren von Event Handlern innerhalb von JSX-Vorlagen keine gute Idee. Hier ist es in Ordnung, weil unsere Event Handler so einfach gestaltet sind.

Trennen wir trotzdem die Event Handler in verschiedene Funktionen ab:

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

Hier wurden die Event Handler korrekt definiert. Der Wert des onClick-Attributs ist eine Varaiable, die auf eine Funktion verweist:

```javascript
<button onClick={increaseByOne}> 
  plus
</button>
```

## Passing state to child components

Es wird empfohlen React-Komponenten zu schreiben, die klein und über ganze Anwendungen und sogar über Projekte wiederverwertbar sind. Verbessern wir unsere Anwendung so, dass sie aus drei kleineren Komponenten besteht, ein Komponent für das Darstellen des Zählers und zwei Komponenten für Buttons.

Als ersten implementieren wir den Komponenten Display, der für das Darstellen des Inhalts des Zählers verantwortlich ist.

Eine best practice in React ist es [den State durch die Komponentenstruktur zu reichen](https://reactjs.org/docs/lifting-state-up.html). Die Dokumentation besagt:

Oft stellen mehrere Komponenten die selben sich veränderten Daten dar. Wir empfehlen den gemeinsamen State in den obersten gemeinsamen Komponenten zu schreiben.

Setzt wir also den State der Anwendung in den Komponenten App und geben ihn als props an den Komponenten Display weiter:

```javascript
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

Den Komponenten zu verwenden ist einfach, wir müssen nur den State des Zählers übergeben:

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

Alles funktioniert weiterhin. Wenn die Buttons geklickt werden und die App erneut gerendert wird, werden auch alle Unterkomponenten neugerendert, was Display einschließt.

Erstellen wir jetzt einen Komponenten Button für die Buttons unserer Anwendung. Wir übergeben sowohl den Event Handler also auch den Titel des Buttons über die props:

```javascript
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

Und unser Komponent App sieht so aus:

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

Da wir jetzt Komponenten für die Buttons haben, die sich leicht wiederverwenden lassen, können wir in unsere Anwendung auch einen neue Funktionalität einbauen: einen Button, der es ermöglicht, den Zähler runterzusetzen.

Der Event Handler wird an den Komponenten Button über onClick weitergegeben. Der Name der props selbst ist nicht signifikat, aber unsere Benennung war nicht komplett zufällig. Reacts eigene Anleitung schlägt diese Konvention vor.

## Changes in state cause rerendering

Besprechen wir nochmal die Hauptprinzipien, wie unsere Anwendung läuft:

Wenn die Anwendung startet, wird der Code im Komponenten App ausgeführt. Dieser Code nutzt useState, um den State der Anwendung zu erstellen und einen Anfangswert für die Variable counter festzulegen. Diser Komponent App enthält den Komponenten Display, der den Wert des Zählers - 0 - ausgibt, und drei Button-Komponenten. Die Buttons haben alle Event Handler, die verwendet werden den State des Zähler zu ändern.

Wenn einer der Buttons geklickt wird, wird der Event Handler ausgeführt. Der Event Handler verändert den State des Komponenten App mit der Funktion setCounter.

Also, wenn ein Benutzer auf den Button plus klickt, ändert der Event Handler den Wert des Zählers auf 1 und der Komponent App wird erneut gerendert. Das führt dazu, das seine Unterkomponenten Display und Button auch erneut gerendert werden. Display erhält den neuen Wert des Zählers - 1 - als props. Die Komponenten Button erhalten Event Handler, die genutzt werden, um den State des Zählers zu ändern.

## Refactoring the components

Der Komponent, der den Wert des Zählers anzeigt, sieht so aus:

```javascript
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

Der Komponent verwendet nur das Zählerfeld seiner props. Das bedeutet, das wir den Komponenten mit Destrukturieren vereinfachen können, ungefähr so:

```javascript
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

Da die Funktion, die den Komponenten definiert, nur eine Ausgabe hat, können wir die Funktion als Pfeilfunktion darstellen:

```javascript
const Display = ({ counter }) => <div>{counter}</div>
```

Wir können auch den Komponenten Button vereinfachen.

```javascript
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

Wir nutzen Destrukturierung, um nur die benötigten Felder der props zu bekommen und verwenden hierfür auch Pfeilfunktionen:

```javascript
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
```

Jetzt können wir noch einen Schritt weitergehen und den Komponenten in nur einer Zeile schreiben:

```javascript
const Button = ({ onClick, text }) => <button onClick={onClick}>{text}</button>
```

Trotzdem solltet ihr vorsichtig sein und euere Komponenten nicht zu sehr zu vereinfachen. Denn es sorgt dafür, dass Erweiterungen deutlich schwieriger umzusetzen sind.

[zurück zu Abschnitt 1b](part_1b.md)

[weiter zu Abschnitt 1d](part_1d.md)
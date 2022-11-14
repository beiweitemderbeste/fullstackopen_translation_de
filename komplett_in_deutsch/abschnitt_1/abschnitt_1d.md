# Abschnitt 1d

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

Version 18 von React wurde im späten März 2022 veröffentlicht. Der Code in diesem Kurs sollte mit der neuen React-Version kompatibel sein, aber einige Bibliotheken könnten es noch nicht sein. Zur Zeit des Verfassens dieses Textes (04.04.) ist mindestens der Client für Apollo, der in Abschnitt 8 verwendet wird, noch nicht zur aktuellen Reactversion kompatibel.

Tritt der Fall auf, dass eure Anwendung wegen Kompabilitätsproblemen von Bibliotheken nicht funktioniert, führt ein Downgrade zu einer älteren Reactversion durch, in dem ihr die Datei package.json so ändert:

```javascript
{
  "dependencies": {
    "react": "^17.0.2",    
    "react-dom": "^17.0.2",    
    "react-scripts": "5.0.0",
    "web-vitals": "^2.1.4"
  },
  // ...
}
```

Nach der Änderung müssen die Abhängigkeiten erneut installiert werden

```javascript
npm install
```

Beachtet, dass auch die Datei index.js abgeändert werden muss. In React 17 sieht das so aus

```javascript
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

aber in React 18 wäre das korrekt

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

## Complex state

In unserem vorangegangen Beispiel bestand der State der Anwendung aus nur einem Integer. Was, wenn unsere Anwendung einen komplexeren State benöitgen würde?

In den meisten Fällen ist die beste Art das zu erreichen, indem man die Funktion useState mehrfach verwendet, um verschiedene "Stücke" des States zu erstellen.

Im folgenden Code erstellen wir 2 Teile des States der Anwendung mit den Namen left und right, die beide einen Anfangswert von 0 zugewiesen bekommen:

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

Der Komponent erhält Zugang zu den Funktionen setLeft und setRigt, die er benutzen kann, um die zwei Teile des States zu aktualisieren.

Der State des Komponenten oder ein Teil seines States kann von jedem Datentyp sein. Wir können dieselbe Funktionalität erreichen, wenn wir jeden Klick auf left oder right mitzählen und in einem einzigen Objekt speichern:

```javascript
{
  left: 0,
  right: 0
}
```

In diesem Fall würde die Anwendung so aussehen:

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

Jetzt hat der Komponent nur einen State un die Event Handler kümmern sich darum, den State der ganzen Anwendung zu ändern.

Der Event Handler sieht ein bisschen chaotisch aus. Wenn der Button left geklickt wird, wird die folgende Funktion aufgerufen:

```javascript
const handleLeftClick = () => {
  const newClicks = { 
    left: clicks.left + 1, 
    right: clicks.right 
  }
  setClicks(newClicks)
}
```

Das folgende Objekt wird als der neue State der Anwendung verwendet:

```javascript
{
  left: clicks.left + 1,
  right: clicks.right
}
```

Der neue Wert der Eigenschaft left ist jetzt der gleiche Wert von left + 1 aus dem vorangegangenen States und der Wert der Eigenschaft right ist jetzt derselbe Wert von right wie vom vorangegangenen State.

Ein geschickterer Weg, um ein neues State-Objekt zu definieren, ist die Object Spread-Syntax, die mit der Spezifikation der Sprache im Sommer 2018 kam:

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

Die Syntax erscheint zu erst ein bisschen fremd. In der Praxis erstellt { ...clicks } ein neues Objekt, dass Kopien jeder Eigenschaft des Objekts clicks hat. Wenn wir eine spezielle Eigenschaft bestimmen, z.B. right in { ...clicks, right: 1 }, ist der Wert der Eigenschaft right im neuen Ojbekt 1.

Im oberen Beispiel, erstellt die Zeile

```javascript
{ ...clicks, right: clicks.right + 1 }
```

eine Kopie des Objekts clicks, bei der der Wert der Eigenschaft right um 1 erhöht wird.

Das Zuweisen des Objekts zu einer Variablen in den Event Handlern ist nicht notwendig und wir können die Funktionen auf folgendes verkürzen:

```javascript
const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })
```

Einige Leser mögen sich wundern, warum wir den State nicht direkt ändern, ungefähr so:

```javascript
const handleLeftClick = () => {
  clicks.left++
  setClicks(clicks)
}
```

Die Anwendung scheint zu funktionieren, trotzdem ist es in React verboten, den State direkt zu verändern, da das ungewünschte Nebeneffekte haben kann. Das Ändern des States muss immer über den Weg, ein neues Objekt zu erstellen, erfolgen.

In dieser speziellen Anwendung ist das Speichern des States in einem einzigen State-Objekt eine schlechte Wahl: es gibt keinen offensichtlichen Profit dadurch und es verkompliziert die Anwendung nur unnötig. In diesem Fall wäre es besser, die Klickzähler in verschiedenen Teilen des States zu speichern.

Es gibt Gelegenheiten, wo es profitabler sein kann, einen Teil des States in einer komplexeren Datenstruktur zu speichern. Die offizielle React-Dokumentation enthält einige hilfreiche Tipps zu diesem Thema.

## Handling arrays

Erweitern wir den State unserer Anwendung um ein Array allClicks, das jeden Klick mit schreibt, der in der Anwendung auftaucht.

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

Jeder Klick wir in einem abgetrennten Teil des States, allClicks, gespeichert. Dieser wird alles leeres Array initialisiert:

```javascript
const [allClicks, setAll] = useState([])
```

Wenn der Button left geklickt wird, hängen wir den Buchstaben L an das Array allClicks:

```javascript
const handleLeftClick = () => {
  setAll(allClicks.concat('L'))
  setLeft(left + 1)
}
```

Der Teil des States, der in allClicks gespeichert ist, wird als Array festgelegt, das alle Elemente des vorherigen States enthält und den Buchstaben L. Das Hinzufügen des neuen Elements geschieht über die Methode concat, die nicht das bestehende Array verändert, sondern eine neue Kopie des Arrays mit dem neuen Element erstellt.

Wie bereits erwähnt ist es in Javascript auch möglich mit der Methode push neue Elemente an ein Array zu hängen. Wenn wir ein Element über push hinufügen und dann den State aktualisieren, würde die Anwendung immer noch funktionieren:

```javascript
const handleLeftClick = () => {
  allClicks.push('L')
  setAll(allClicks)
  setLeft(left + 1)
}
```

Trotzdem, macht das nicht! Wie bereits erwähnt wird der State von React-Komponenten wie z.B. allClicks nicht direkt verändert. Auch wenn das direkte Verändern des States in machen Fällen zu funktionieren scheint, kann es zu Problemen führen, die sich nur sehr schwer reparieren lassen.

Schauen wir uns genauer, wie das Klicken auf der Seite angezeigt wird:

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

Wir rufen die Methode join für das Array allClicks auf, dass alle Elemente des Arrays zu einem einzigen String zusammenfügt.

## Conditional rendering

Verändern wir die Anwendung so, dass das Anzeigen der Klickhistorie von einem Komponenten history gehändelt wird:

```javascript
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }
  
  return (    
    <div>      
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const App = () => {
  // ...

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <History allClicks={allClicks} />    
    </div>
  )
}
```

Das Verhalten des Komponenten hängt davon ab, ob einer der Button geklickt wurde oder nicht. Wenn nicht, ist das Array allClicks leer, der Komponent zeigt stattdessen ein div-Element mit einigen Anweisungen an:

```html
<div>the app is used by pressing the buttons</div>
```

In allen anderen Fällen zeigt der Komponent die Klickhistorie an:

```javascript
<div>
  button press history: {props.allClicks.join(' ')}
</div>
```

Der Komponent History zeigt komplett verschiedene React-Elemente an, abhängig vom State der Anwendung. Das wird conditional rendering gennant.

React bietet auch viele andere Wege für conditional rendering an. Wir schauen uns das noch genauer in Abschnitt 2 an.

Verändern wir unsere Anwendung ein letztes Mal, indem wir unseren Button-Komponenten von vorhin einsetzen

```javascript
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }

  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const Button = ({ handleClick, text }) => (  
  <button onClick={handleClick}>    
    {text}  
  </button>
)

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
      <Button handleClick={handleLeftClick} text='left' />
      <Button handleClick={handleRightClick} text='right' />      
      {right}
      <History allClicks={allClicks} />
    </div>
  )
}
```

## Old React

In diesem Kurs benutzen wir den State Hook, um State an React-Komponenten hinzufügen. Der State Hook ist Teil neuerer Reactversionen und ist ab Version 16.8.0 verfügbar. Komponenten, die vorher einen State benötigten wurden als Klassenkomponenten definiert.

In diesem Kurs haben wir die leicht radikale Entscheidung getroffen ab Tag 1 ausschließlich Hooks zu nutzen, um sicherzustellen, dass wir den aktuellen und zukünftigen Programmierstil von React lernen. 

## Debugging React applications

Ein großer Teil der typischen Entwicklerarbeit wird für Debugging und das Lesen von Code verwendet. Ab und zu schreigben wir selbst noch ein oder zwei Zeilen, aber ein großer Teil unserer Zeit wird dafür verwendet herauszufinden, warum etwas nicht funktioniert oder warum es funktioniert. Gute Werkzeuge für Debugging sind aus diesem Grund extrem wichtig.

Glücklicherweise ist React eine extrem Entwicklerfreundliche Bibliothek, wenn es um Debugging geht.

Bevor wir weitermachen, erinnern wir uns an eine der wichtigsten Regeln der Webentwicklung.

Die erste Regel der Webentwicklung

- Immer die Browserkonsole offen haben

Besonders der Console-Tab sollte immer geöffnet sein (es sei denn, es gibt einen besonderen Grund, einen anderen Tab anzuschauen).

Habt immer euren Editor und eure Webseite gleichzeitig offen.

Falls und wenn es zu Fehlern kommt, leuchtet der Browser wie ein Weihnachtsbaum auf:

!["fullstack content"](./bilder/abschnitt1d_bild1.png?raw=true)

In so einem Fall solltet ihr erst das Problem finden und lösen, bevor ihr weiterprogrammiert. Es gab noch nie eine Zeit, in der das masshaft weiters Programmieren Probleme auf wundersame Weise gelöst hat. Ich denke auch nicht, dass das jemals funktionieren wird.

Früher hat man mit Print-Ausdrücken gearbeitet, was heutzutage immer noch eine gute Idee ist. Wenn der Komponent

```javascript
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
```

nicht funktioniert wie gedacht, ist es hilfreich, seine Variable in der Konsole ausgeben zu lassen.  Um das machen zu können, müssen wir unsere Funktion wieder zurückbauen und die props nicht Destrukturieren:

```javascript
const Button = (props) => { 
  console.log(props)  
  
  const { onClick, text } = props
  return (
    <button onClick={onClick}>
      {text}
    </button>
  )
}
```

Das wird direkt zeigen, wenn z.B. eines der Attribute falsch geschrieben wurde.

Hinweis: Wenn ihr console.log zum Debuggen einsetzt, solltet ihr nicht mehrere Objekte mit dem +-Zeichen verbinden. Also statt 

```javascript
console.log('props value is ' + props)
```

zu schreiben, trennt die Objekte mit einem Komma:

```javascript
console.log('props value is', props)
```

Wenn ihr den Java-artigen Weg geht, um Strings mit einem Objekt zu verwenden, erhaltet ihr eine eher uninformative Nachricht:

```javascript
props value is [object Object]
```

Im Gegensatz dazu sind alle Objekte, die von einem Komma getrennt sind, in der Browserkonsole verfügbar.

Ausgaben über die Konsole ist natürlich nicht der einzige Weg, um Anwendungen zu debuggen. Ihr könnt die Ausführung eurer Anwendung im Debugger der Chrome developer tools jederzeit anhalten, in dem ihr den Befehl debugger irgendwo in euren Code schreibt.

Die Ausführung wird gestoppt, wenn sie an den Punkt kommt, wo der Befehl debugger ausgeführt wird:

!["fullstack content"](./bilder/abschnitt1d_bild2.png?raw=true)

Wenn wir den Console-Tab öffnen, ist es leicht den aktuellen State der Variablen zu inspizieren:

!["fullstack content"](./bilder/abschnitt1d_bild3.png?raw=true)

Wenn der Grund für den Bug gefunden wurde, kann der Befehl debugger entfernt und die Seite aktualisiert werden.

Der Debugger ermöglicht es uns unseren Code Zeile für Zeile auszuführen, dazu gibt es auf der rechten Seite eine Steuerung.

Ihr könnt den Debugger auch ohne den Befehl dazu aufrufen, indem ihr Breakpoints im Sources-Tab setzt. Die Werte der Komponentenvariablen kann man in der Scope-Sektion untersuchen:

!["fullstack content"](./bilder/abschnitt1d_bild4.png?raw=true)

Der State des Komponenten App wurde so definiert:

```javascript
const [left, setLeft] = useState(0)
const [right, setRight] = useState(0)
const [allClicks, setAll] = useState([])
```

Die Entwicklerwerkzeuge zeigen jetzt den State der Hooks geordnet nach ihrer Definition an:

!["fullstack content"](./bilder/abschnitt1d_bild6.png?raw=true)

Der erste State enthält den Wert des States left, der nächste enthält den Wert des States right und der letzte enthält den Wert des States allClicks.

## Rules of Hooks

Es gibt einige Begrenzungen und Regeln, die wir beachten müssen, um sicherzustellen, dass unsere Anwendung die hook-basierten State-Funktionen korrekt verwendet.

Die Funktion useState (und die Funktion useEffect, die wir uns später noch genauer anschauen) dürfen nicht innerhalb einer Schleife, einer Bedingung oder außerhalb einer Funktion aufgerufen werden. Das muss erfolgen, damit die Hooks immer in derselben Reihenfolge  aufgerufen werden und das passiert nicht, wenn sich die Anwendung ungleichmäßig verhält.

Um nochmal zu wiederholen: Hooks dürfen nur innerhalb des Funktionskörpers eines React-Komponenten aufgerufen werden:

```javascript
const App = () => {
  // these are ok
  const [age, setAge] = useState(0)
  const [name, setName] = useState('Juha Tauriainen')

  if ( age > 10 ) {
    // this does not work!
    const [foobar, setFoobar] = useState(null)
  }

  for ( let i = 0; i < age; i++ ) {
    // also this is not good
    const [rightWay, setRightWay] = useState(false)
  }

  const notGood = () => {
    // and this is also illegal
    const [x, setX] = useState(-1000)
  }

  return (
    //...
  )
}
```

## Event Handling Revisited

Event Handling hat sich in vorangegangenen Ausgaben dieses Kurses als schwieriges Thema erwiesen.

Aus diesem Grund schauen wir uns dieses Thema nochmal an.

Nehmen wir an, dass wir eine einfache Anwendung mit dem folgenden App-Komponenten entwickeln:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  return (
    <div>
      {value}
      <button>reset to zero</button>
    </div>
  )
}
```

Wir wollen, dass bei einem Klick auf den Button der State in der Variable value zurückgesetzt wird.

Event Handler müssen immer eine Funktion oder ein Verweis auf eine Funktion sein. Der Button wird nicht funktionieren, wenn der Event Handler auf eine Variable eines anderen Typs gesetzt wird.

Wenn wir den Event Handler als String definieren würden:

```javascript
<button onClick="crap...">button</button>
```

Würde uns React eine Warnung in der Konsole anzeigen:

```javascript
index.js:2178 Warning: Expected `onClick` listener to be a function, instead got a value of `string` type.
    in button (at index.js:20)
    in div (at index.js:18)
    in App (at index.js:27)
```

Der folgende Versuch würde auch nicht funktionieren:

```javascript
<button onClick={value + 1}>button</button>
```

Hier haben wir versucht den Event Handler auf value + 1 zu setzen, was nur das Ergebnis dieser Operation ausgibt. React wird auch dafür eine Warnung anzeigen:

```javascript
index.js:2178 Warning: Expected `onClick` listener to be a function, instead got a value of `number` type.
```

Dieser Versuch würde auch nicht funktionieren:

```javascript
<button onClick={value = 0}>button</button>
```

Der Event Handler ist keine Funktion, sondern die Zuweisung einer Variablen und React wird auch dafür eine Warnung in der Konsole anzeigen. Dieser Versuch hat auch das Problem, dass der State direkt verändert wird, was wir in React nie tun sollten.

```javascript
<button onClick={console.log('clicked the button')}>
  button
</button>
```

Die Nachricht wird in der Konsole angezeigt, wenn der Komponent gerendert wird, aber ansonsten passiert nichts bei einem Klick auf den Button. Warum funktioniert das nicht, selbst wenn unser Event Handler die Funktion console.log enthält?

Hier ist das Problem, dass unser Event Handler als Funktionsaufruf definiert ist, was bedeutet, dass dem Event Handler tatsächlich der Rückgabewert der Funktion zugewiesen wird, was hier undefined ist.

Der Funktionsaufruf console.log wird ausgeführt, wenn der Komponent gerendert wird und aus diesem Grund wird er nur einmal an der Konsole ausgegeben.

Der folgende Versuch hat auch seine Fehler:

```javascript
<button onClick={setValue(0)}>button</button>
```

Wir haben wieder versucht für den Event Handler einen Funktionsaufruf zu setzen. Das funktioniert nicht. Der besondere Versuch verursacht auch ein anderes Problem. Wenn der Komponent gerendert wird, wird die Funktion setValue(0) ausgeführt, was dazu führt, dass der Komponent erneut gerendert wird. Das erneute Rendern ruft wiederum setValue(0), was zu einer unendlichen Schleife führt.

Um einen bestimmten Funktionsaufruf auszuführen, wenn der Button geklickt wird, erreichen wir so:

```javascript
<button onClick={() => console.log('clicked the button')}>
  button
</button>
```

Jetzt ist der Event Handler eine Funktion, die als Pfeilfunktion () => console.log('clicked the button') definiert ist. Wenn der Komponent gerendert wird, wird keine Funktion aufgerufen und nur der Verweis auf die Pfeilfunktion als Event Handler gesetzt. Die Funktion wird nur aufgerufen, wenn der Button geklickt wird.

Wir können mit der gleichen Technik den State in unserer Anwendung zurücksetzen:

```javascript
<button onClick={() => setValue(0)}>button</button>
```

Der Event Handler ist jetzt die Funktion () => setValue(0).

Ihr werdet oft sehen, dass Event Händler an einem anderen Ort definiert werden. In der folgenden Version unserer Anwendung definieren wir eine Funktion, die dann im Körper der Komponentenfunktion der Variablen handleClick zugewiesen wird:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  const handleClick = () =>
    console.log('clicked the button')

  return (
    <div>
      {value}
      <button onClick={handleClick}>button</button>
    </div>
  )
}
```

Der Variablen handleClick wird jetzt ein Verweis auf die Funktion zugewiesen. Der Verweis wird dem Button als onClick-Attribut zugewiesen:

```javascript
<button onClick={handleClick}>button</button>
```

Natürlich kann unsere Event Handler-Funktion auch aus verschiedenen Befehlen bestehen. Für diese Fälle verwenden wir die längere Syntax mit geschwungenen Klammern:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  const handleClick = () => {    
    console.log('clicked the button')    
    setValue(0)  
  }

  return (
    <div>
      {value}
      <button onClick={handleClick}>button</button>
    </div>
  )
}
```

## Function that returns a function

Ein anderer Weg, um einen Event Handler zu definieren, ist eine Funktion, die wiederum eine Funktion zurückgibt.

Verändern wir unseren Code wir folgt:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  const hello = () => {    
    const handler = () => console.log('hello world')    
    return handler  
  }

  return (
    <div>
      {value}
      <button onClick={hello()}>button</button>
    </div>
  )
}
```

Der Code funktioniert korrekt, auch wenn es kompliziert aussieht.

Auf den Eventhandler wird jetzt ein Funktionsaufruf gesetzt:

```javascript
<button onClick={hello()}>button</button>
```

Vorhin haben wir gesagt, dass ein Event Handler kein Funktionsaufruf sein darf, sondern es eine Funktion oder ein Verweis auf eine Funktion sein muss. Warum funktioniert hier ein Funktionsaufruf trotzdem?

Wenn der Komponent gerendert wird, wird folgende Funktion ausgeführt:

```javascript
const hello = () => {
  const handler = () => console.log('hello world')

  return handler
}
```

Der Ausgabewert der Funktion ist eine andere Funktion, die der Event Handler-Variablen zugewiesen wird.

Wenn React die Zeile

```javascript
<button onClick={hello()}>button</button>
```

anzeigt, weist sie den Ausgabewert von hello() dem onClick-Attribut zu. Im Wesentlichen wird die Zeile auf folgendes umgewandelt:

```javascript
<button onClick={() => console.log('hello world')}>
  button
</button>
```

Da die Funktion hello eine Funktion ausgibt, ist jetzt auch der Event Handler eine Funktion.

Was ist der Sinn dieses Konzepts?

Ändern wir den Code ein bisschen ab:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  const hello = (who) => {    
    const handler = () => {      
      console.log('hello', who)    
    }    
    return handler  
  }

  return (
    <div>
      {value}
      <button onClick={hello('world')}>button</button>      
      <button onClick={hello('react')}>button</button>      
      <button onClick={hello('function')}>button</button>    
    </div>
  )
}
```

Jetzt hat die Anwendung 3 Buttons mit Event Handlern, über die die Hello-Funktion inklusive Parameter ausgeführt wird.

Der erste Button wird so definiert:

```javascript
<button onClick={hello('world')}>button</button>
```

Der Event Handler wird erstellt, wenn die Funktion hello('world') ausgeführt wird. Der Funktionsaufruf gibt eine Funktion aus:

```javascript
() => {
  console.log('hello', 'world')
}
```

Der zweite Button wird so definiert:

```javascript
<button onClick={hello('react')}>button</button>
```

Der Funktionsaufruf hello('react'), der den Event Handler erstellt, gibt folgendes zurück:

```javascript
() => {
  console.log('hello', 'react')
}
```

Beide Buttons bekommen ihre eigenen maßgeschneiderten Event Handlers.

Funktionen, die Funktionen ausgeben, können dafür genutzt werden, generische Funktionalitäten einzubauen, die mit Parametern angepasst werden können. Man kann von der Funktion hello, die die Event Handler erstellt, als eine Art Fabrik denken, die angepasste Event Handler produziert, um Benutzer zu begrüßen.

Unsere aktuelle Definition ist ein bisschen ausführlich:

```javascript
const hello = (who) => {
  const handler = () => {
    console.log('hello', who)
  }

  return handler
}
```

Eliminieren wir die Helfervariable und geben direkt die erstellte Funktion zurück:

```javascript
const hello = (who) => {
  return () => {
    console.log('hello', who)
  }
}
```

Da unsere Funktion hello aus einem einzigen return-Befehl besteht, können wir die geschwungenen Klammern weglassen und die Pfeilsyntax verwenden:

```javascript
const hello = (who) =>
  () => {
    console.log('hello', who)
  }
```

Schreiben wir alle Pfeile in dieselbe Zeile: 

```javascript
const hello = (who) => () => {
  console.log('hello', who)
}
```

Wir können den gleichen Trick, der die Event Handler definiert, verwenden, um den State des Komponenten auf einen bestimmten Wert setzten. Ändern wir unseren Code wie folgt ab:

```javascript
const App = () => {
  const [value, setValue] = useState(10)
  
  const setToValue = (newValue) => () => {    
    console.log('value now', newValue)  //print the new value to console   
    setValue(newValue)  
  }

  return (
    <div>
      {value}
      <button onClick={setToValue(1000)}>thousand</button>      
      <button onClick={setToValue(0)}>reset</button>      
      <button onClick={setToValue(value + 1)}>increment</button>    
    </div>
  )
}
```

Wenn der Komponent gerendert wird, wird der Button thousand erstellt:

```javascript
<button onClick={setToValue(1000)}>thousand</button>
```

Der Event Handler wird auf den Rückgabewert von setToValue(1000) gesetzt, was die folgende Funktion ist:

```javascript
() => {
  console.log('value now', 1000)
  setValue(1000)
}
```

Der Button increment sieht so aus:

```javascript
<button onClick={setToValue(value + 1)}>increment</button>
```

Der Event Handler wird durch den Funktionsaufruf setToValue(value + 1) erstellt, der als Parameter den aktuellen Wert der Statevariable, um 1 erhöht, erhält. Wenn der Wert 10 wäre, wäre der erstellte Event Handler die Funktion:

```javascript
() => {
  console.log('value now', 11)
  setValue(11)
}
```

Aber man muss nicht Funktionen, die Funktionen ausgeben, verwenden, um diese Funktionalität zu erreichen. Ändern wir die Funktion setToValue, die für das Aktualisieren des States zuständig ist, in eine normale Funktion zurück:

```javascript
const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = (newValue) => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <button onClick={() => setToValue(1000)}>
        thousand
      </button>
      <button onClick={() => setToValue(0)}>
        reset
      </button>
      <button onClick={() => setToValue(value + 1)}>
        increment
      </button>
    </div>
  )
}
```

Jetzt können wir den Event Handler als Funktion, die die Funktion setToValue mit einem passenden Parameter aufruft, definieren. Der Event Handler, der für das Zurücksetzen der Anwendung zuständig ist, würde so aussehen:

```javascript
<button onClick={() => setToValue(0)}>reset</button>
```

Welche der beiden präsentierten Arten, wie man Event Handler definiert, wählt ist reine Geschmackssache.

## Passing Event Handlers to Child Components

Verwenden wir für den Button für einen eigenen Komponenten:

```javascript
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)
```

Der Komponent bekommt seine Event Handler-Funktion from prop handleClick und den Text des Buttons vom prop text.

Das Verwenden des Komponents Button ist einfach, trotzdem müssen wir sicherstellen, das wir die korrekten Namen für die Attribute verwenden, wenn wir props zu dem Komponenten weiterreichen.

!["fullstack content"](./bilder/abschnitt1d_bild7.png?raw=true)

## Do Not Define Components Within Components

Fangen wir an, den Wert der Anwendung in seinem eigenen Display-Komponenten anzuzeigen.

Wir ändern die Anwendung, indem wir einen neuen Komponenten innerhalb des App-Komponenten definieren.

```javascript
// This is the right place to define a component
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)

const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = newValue => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  // Do not define components inside another component
  const Display = props => <div>{props.value}</div>

  return (
    <div>
      <Display value={value} />
      <Button handleClick={() => setToValue(1000)} text="thousand" />
      <Button handleClick={() => setToValue(0)} text="reset" />
      <Button handleClick={() => setToValue(value + 1)} text="increment" />
    </div>
  )
}
```

Die Anwendung scheint weiterhin zu funktionieren, aber implementiert niemals Komponenten auf diese Art! Diese Methode hat keine Boni und für zu vielen unagenehmen Problemen. Die größten Probleme sind dem Fakt geschuldet, dass React Komponenten, die innerhalb anderer Komponenten definiert wurden, als neuen Komponenten behandelt. Das macht es für React unmöglich, den Komponenten zu optimieren.

Schieben wir stattdessen den Komponenten Display seine korrekte Stelle, was außerhalb des Komponenten App ist:

```javascript
const Display = props => <div>{props.value}</div>

const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)

const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = newValue => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  return (
    <div>
      <Display value={value} />
      <Button handleClick={() => setToValue(1000)} text="thousand" />
      <Button handleClick={() => setToValue(0)} text="reset" />
      <Button handleClick={() => setToValue(value + 1)} text="increment" />
    </div>
  )
}
```

## Useful Reading

Im Internet gibt es viel Material zu React. Trotzdem nutzen wir den aktuellen Programmierstil von React, für den die Mehrheit des Onlinematerials veraltet ist.

Ihr findet die folgenden Links sicher nützlich:

- Die offizielle Reactdokumentation ist es Wert gelesen zu werden, auch wenn das meiste erst zu einem späteren Zeitpunkt im Kurs relevant wird. Zudem ist alles, was sich auf klassenbasierte Komponenten bezieht, für uns irrelavant.

Einige Kurse auf egghead.io, wie z.B. Start learning React, sind von hoher Qualität und der kürzlich aktualisierte The Beginner's Guide to React ist auch relativ gut. Beide Kurse führen Themen ein, die auch noch später in diesem Kurs bearbeitet werden. Hinweis: Ersterer verwendent Klassenkomponenten, aber der Zweite verwenden funktionale.

## Exercises

Reicht die Lösungen für die Aufgaben ein, indem ihr euren Code auf GitHub veröffentlicht und markiert die abgeschlossen Aufgaben im Submissionsystem.

Merkt euch alle Aufgaben eines Abschnitts gesammtelt einzureichen. Wenn ihr eure Lösungen für einen Abschnitt eingereicht habt, könnt ihr keinen weiteren Aufgaben für diesen Abschnitt einreichen.

Einige der Aufgaben beziehen sich auf die gleiche Applikation. In diesen Fällen reicht es aus die Endversion der Anwendung einzureichen. Wenn ihr möchtet, können ihr einen Commit für jede beendete Aufgabe machen, aber das wird nicht verlangt.

WARNUNG: create-react-app erstellt aus eurem Projekt ein git-Repository, es sei denn, ihr erstellt eure Anwendung in einem bereits bestehenden git-Repository. Sehr wahrscheinlich möchtet ihr nicht für jedes Projekt ein eigenes Repository, deswegen könnt ihr einfach den Befehlt rm -rf .git im Wurzelverzeichnis eurer Anwendung ausführen.

In manchen Fällen müsst ihr auch den folgenden Befehl im Wurzelverzeinis des Projekts ausführen:

```bash
rm -rf node_modules/ && npm i
```

### 1.6 unicafe step1

Wie die meisten Firmen sammelt Unicafe Feedback von seinen Kunden. Eure Aufgabe ist es, eine Webanwendung zu implementieren, um das Feedback zu sammeln. Es gibt nur drei Optionen für Feedback: good, neutral, und bad.

Die Anwendung soll die absolute Zahl des abgegebenen Feedbacks pro Kategorie anzeigen. Die Anwendung könnte so aussehen:

!["fullstack content"](./bilder/abschnitt1d_bild8.png?raw=true)

Hinweis: Die Anwendung muss nur in einer einzigen Browsersitzung funktionieren. Wenn ihr die Seite aktualisiert, darf das gesammlte Feedback verschwinden.

Es wäre ratsam, die gleiche Struktur, wie in diesem Material und der vorangegangenen Aufgabe, zu verwenden. Die Datei index.js sieht so aus:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

Ihr könnt den folgenden Code als Ausgungspunkt für die Datei App.js verwenden:

```javascript
import { useState } from 'react'

const App = () => {
  // save clicks of each button to its own state
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  return (
    <div>
      code here
    </div>
  )
}

export default App
```

### 1.7 unicafe step2

Erweitert eure Anwendung, so dass sie mehr Statistiken über das gesammelte Feedback anzeigt: die absolute Zahl des gesammelten Feedback, die durchschnittliche Bewertung (good: 1, neutral: 0, bad: -1) und der Anteil des positiven Feedbacks.

!["fullstack content"](./bilder/abschnitt1d_bild9.png?raw=true)

### 1.8 unicafe step3

Verbessert eure Anwendung, so dass sie die Statistiken in einem eigenen Komponenten anzeigt. Der State der Anwendung sollte im Komponenten App verbleiben.

Beachtet, dass Komponenten nicht innerhalb anderer Komponenten definiert werden sollten:

```javascript
// a proper place to define a component
const Statistics = (props) => {
  // ...
}

const App = () => {
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  // do not define a component within another component
  const Statistics = (props) => {
    // ...
  }

  return (
    // ...
  )
}
```

### 1.9 unicafe step4

Ändert eure Anwendung so, dass sie die Statistiken nur ausgibt, wenn Feedback gesammelt wurde.

!["fullstack content"](./bilder/abschnitt1d_bild10.png?raw=true)

### 1.10 unicafe step5

Verbessern wir die Anwendung noch mehr. Extrahiert die folgenden zwei Komponenten:

- Button: für das Definieren der Buttons, um Feedback abzugeben
- StatisticLine: um eine einzelne Statistik, z.B. den durchschnittlichen Stand, anzuzeigen

Um klar zu sein: der Komponent StatisticLine zeigt immer nur eine einzelne Statistik an, was bedeutet, dass die Anwendung mehrere Komponenten verwendet, um alle Statistiken anzuzeigen.

```javascript
const Statistics = (props) => {
  /// ...
  return(
    <div>
      <StatisticLine text="good" value ={...} />
      <StatisticLine text="neutral" value ={...} />
      <StatisticLine text="bad" value ={...} />
      // ...
    </div>
  )
}
```

Der State der Anwendung sollte weiterhin im Hauptkomponenten App verbleiben.

### 1.11* unicafe step6

Stellt die Statistiken in einer HTML-Tablle dar, so dass eure Anwendung ungefähr so aussieht:

!["fullstack content"](./bilder/abschnitt1d_bild11.png?raw=true)

Erinnert euch daran, immer eure Browserkonsole geöffnet zu haben. Wenn ihr diese Warnung in euerer Konsole seht:

!["fullstack content"](./bilder/abschnitt1d_bild12.png?raw=true)

Dann führt alles Nötige aus, um die Warnung verschwinden zu lassen. Versucht die Fehlermeldung in einer Suchmaschine einzugeben, wenn ihr nicht weiterkommt.

Stellt sicher, dass ihr ab jetzt keine Warnung mehr in eurer Konsole angezeigt bekommt.

### 1.12* anecdotes step1

Die Welt der Softwareentwicklung ist voll von Anekdoten, die zeitlose Wahrheiten über unsere Branche in kurzen Einzeilern erzählen.

```javascript
import { useState } from 'react'

const App = () => {
  const anecdotes = [
    'If it hurts, do it more often.',
    'Adding manpower to a late software project makes it later!',
    'The first 90 percent of the code accounts for the first 10 percent of the development time...The remaining 10 percent of the code accounts for the other 90 percent of the development time.',
    'Any fool can write code that a computer can understand. Good programmers write code that humans can understand.',
    'Premature optimization is the root of all evil.',
    'Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.',
    'Programming without an extremely heavy use of console.log is same as if a doctor would refuse to use x-rays or blood tests when diagnosing patients.'
  ]
   
  const [selected, setSelected] = useState(0)

  return (
    <div>
      {anecdotes[selected]}
    </div>
  )
}

export default App
```

Der Inhalt der Datei index.js ist der gleiche, wie in der vorherigen Aufgabe.

Findet heraus, wie man zufällige Zahlen in Javascript, z.B. per Suchmaschine oder Mozilla Developer Network, generiert. Denkt daran, dass ihr das Generieren zufälliger Zahlen direkt in der Browserkonsole testen könnt.

Eure fertige Anwendung sollte ungefähr so aussehen:

!["fullstack content"](./bilder/abschnitt1d_bild13.png?raw=true)

WARNUNG: create-react-app erstellt aus eurem Projekt ein git-Repository, es sei denn, ihr erstellt eure Anwendung in einem bereits bestehenden git-Repository. Sehr wahrscheinlich möchtet ihr nicht für jedes Projekt ein eigenes Repository, deswegen könnt ihr einfach den Befehlt rm -rf .git im Wurzelverzeichnis eurer Anwendung ausführen.

### 1.13* anecdotes step2

Erweitert eure Anwendung, so dass man für die angezeigte Anekdote abstimmen kann.

!["fullstack content"](./bilder/abschnitt1d_bild14.png?raw=true)

Hinweis: Speichert die Stimmen für jede Anektdote in einem Array oder Objekt im State des Komponenten. Erinnert euch daran, dass man den State, der in komplexen Datenstrukturen wie Objekte oder Arrays gespeichert ist, aktualisiert, indem eine Kopie des States erstellt.

```javascript
const points = { 0: 1, 1: 3, 2: 4, 3: 2 }

const copy = { ...points }
// increment the property 2 value by one
copy[2] += 1 
```

> OR a copy of an array like this:

```javascript
const points = [1, 4, 6, 3]

const copy = [...points]
// increment the value in position 2 by one
copy[2] += 1 
```

Ein Array zu benutzen, wäre die einfachere Wahl in diesem Fall. Wenn ihr das Internet durchsucht, werdet ihr viele Hinweise darauf finden, wie man ein mit Nullen gefülltes Array einer bestimmten Länge erstellt.

### 1.14*: anecdotes step3

Implementiert jetzt die Endversion der Anwendung, die die Anekdote mit der höchsten Anzahl an Stimmen anzeigt.

!["fullstack content"](./bilder/abschnitt1d_bild15.png?raw=true)

Wenn mehrere Anekdoten auf dem ersten Platz landen, reicht es aus, eine davon anzuzeigen.
Das war die letzte Aufgabe dieses Abschnitts und es ist Zeit, euren Code auf GitHub zu veröffentlichen und markiert eure fertigen Aufgaben im Submissionsystem.

[zurück zu Abschnitt 1c](part_1c.md)

[zurück zu Abschnitt 1](part_1.md)

[weiter zu Abschnitt 2](../part_2/part_2.md)
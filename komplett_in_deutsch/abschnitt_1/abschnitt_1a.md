# Abschnitt_1a

## Inhaltsverzeichnis

- [Component](#Component)
- [JSX](#JSX)
- [Multiple Components](#Multiple-Components)
- [props: passing data to components](#props-passing-data-to-components)
- [Some notes](#Some-notes)
- [Do not render objects](#do-not-render-objects)
- [Exercises](#Exercises)

Wir befassen uns jetzt mit dem wahrscheinlich wichtigstem Thema dieses Kurses, der [React-Bibliothek](https://reactjs.org/). Wir fangen an, indem wir eine einfache React-Anwendung erstellen und uns mit den Grundkonzepten von React auseinandersetzen.

Der bei weitem einfachste Weg zu starten ist [create-react-app](https://github.com/facebook/create-react-app) zu benutzen. Es ist möglich (aber nicht notwendig) create-react-app auf eurem Rechner zu installieren, wenn npm mit Nodejs (mind. Version 5.3) installiert wurde.

Wir beginnen, indem wir die Anwendung "part1" erstellen und in das Verzeichnis "part1" wechseln:

```
npx create-react-app part1
cd part1
```

Die Anwendung wird folgendermaßen gestartet:

```
npm start
```

Standardmäßig läuft die Anwendung unter [http://localhost:3000](http://localhost:3000)

Euer Standardbrowser sollte sie automatisch öffnen. Öffnet direkt die Browserkonsole:

!["fullstack content"](./bilder/abschnitt1a_bild1.png?raw=true)

Der Quellcode der Anwendung liegt im src-Verzeichnis. Wir vereinfachen den Inhalt der Datei index.js, so dass er so aussieht:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

und die Datei App.js so aussieht:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)

export default App
```

Die Dateien App.css, App.test.js, index.css, logo.svg, setupTests.js und reportWebVitals.js können gelöscht werden, weil sie momentan nicht benötigt werden. 

Wenn ihr folgenden Fehler angezeigt bekommt

!["fullstack content"](./bilder/abschnitt1a_bild2.png?raw=true)

liegt das daran, dass ihr aus irgeneinem Grund eine ältere React-Version als die aktuelle (18) benutzt.

Die Lösung dafür ist, index.js so abzuändern

```javascript
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

Ihr müsst das dann wahrscheinlich auch in euren anderen Projekten machen.

Schaut [hier](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps/#a-note-on-react-version) nach, wenn ihr mehr über die Versionsunterschiede wissen wollt.

## Component

Die Datei App.js definiert einen [React-Komponenten](https://reactjs.org/docs/components-and-props.html) "App". Der Befehl in der letzten Zeile von index.js

```javascript
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

stellt die Inhalte des Komponenten in einem div-Element mit der id "root" dar. Das div-Element wird in der Datei public/index.html eingefügt.

Standardmäßig enthält public/index.html kein HTML, das für uns im Browser sichtbar ist. Ihr könnt versuchen HTML in diese Datei zu schreiben, aber normalerweise wird in React jeder Inhalt über Komponenten dargestellt.

Schauen wir uns den Code, der den Komponenten definiert, ein bisschen genauer an:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)
```

Wie ihr wahrscheinlich schon geraten habt, wird der Komponent als div-Tag angezeigt, der einen p-Tag mit dem Text "Hello world" umschließt.

Technisch gesehen wird der Komponent als Javascriptfunktion definiert. Das Folgende ist eine Funktion (an die keine Parameter übergeben werden):

```javascript
() => (
  <div>
    <p>Hello world</p>
  </div>
)
```

Die Funktion wird dann der Konstanten App zugewiesen:

```javascript
const App = ...
```

Es gibt verschiedene Wege, um Funktionen in Javascript zu definieren. Wir verwenden hier [Pfeilfunktionen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), die in einer neueren Version von Javascript (bekannt als [ECMAScript 6](http://es6-features.org/#Constants)) beschrieben werden.

Da die Funktion nur aus einem Ausdruck besteht, haben wir eine Kurzform verwendet, die so lautet:

```javascript
const App = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}
```

In anderen Worten, die Funktion gibt den Wert des Ausdrucks zurück.

Die Funktion, die den Komponenten definiert, kann jede Art von Javascriptcode enthalten. Ändert euren Komponenten folgendermaßen ab und beobachtet, was passiert:

```javascript
const App = () => {
  console.log('Hello from component')
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}
```

Es ist auch möglich dynamischen Inhalt in einem Komponenten anzuzeigen.

Ändert den Komponenten dafür wie folgt ab:

```javascript
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20

  return (
    <div>
      <p>Hello world, it is {now.toString()}</p>
      <p>
        {a} plus {b} is {a + b}
      </p>
    </div>
  )
}
```

Jeglicher Javascriptcode innerhalb der geschwungenen Klammern wird ausgewertet und das Ergebnis davon wird in seinem jeweiligem Platz im HTML-Code eingefügt.

## JSX

Es sieht so aus als würden React-Komponenten HTML ausgeben, was aber nicht der Fall ist. React-Kompenenten werden meistens in [JSX](https://reactjs.org/docs/introducing-jsx.html) geschrieben. Auch wenn JSX wie HTML aussieht, handelt es sich tatsächlich um eine andere Art Javascript zu schreiben.

Nachdem Kompilieren sieht unsere Anwendung so aus:

```javascript
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20
  return React.createElement(
    'div',
    null,
    React.createElement(
      'p', null, 'Hello world, it is ', now.toString()
    ),
    React.createElement(
      'p', null, a, ' plus ', b, ' is ', a + b
    )
  )
}
```

Die Kompilierung wird von [Babel](https://babeljs.io/repl/) übernommen. Projekte, die mit create-react-app erstellt wurden, sind so konfiguriert, das die Kompilierung automatisch geschieht. Wir werden uns in Abschnitt 7 dieses Kurs näher mit diesem Thema beschäftigen.

Es ist möglich, React in "purem" Javascript zu schreiben ohne JSX zu verwenden. Allerdings würde das niemand, der bei klarem Verstand ist, tun.

In der Praxis sieht JSX ähnlich wie HTML aus, mit dem Unterschied, dass man dynamischen Inhalt leicht einfügen kann, indem man Javascript in geschwungenen Klammern einfügt. Die Grundgedanke von JSX ist sehr ähnlich zu anderen Template-Sprachen, wie z.B. Thymeleaf für Java Spring.

JSX ist ähnlich wie [XML](https://developer.mozilla.org/en-US/docs/Web/XML/XML_introduction), was bedeudet, dass jeder Tag geschlossen werden muss. Zum Beispiel ist ein Zeilenumbruch ein leeres Element, dass in HTML so geschrieben werden kann:

```
<br>
```

aber wenn man JSX schreibt, muss man den Tag schließen:

```
<br />
```

## Multiple Components

Verändern wir die Datei App.js wie folgt:

```javascript
const Hello = () => {  
  return (
    <div>
      <p>Hello world</p>
    </div>  
  )
}

const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />
    </div>
  )
}
```

Hinweis: der Export am unteren Rand wird in diesen und anderen Beispielen weggelassen. Ihr müsst ihn trotzdem einfügen, damit die Anwendung funktioniert.

Wir haben einen neuen Komponenten Hello definiert und innerhalb des Komponenten App benutzt. Ein Komponent kann selbstredend mehrere Male verwendet werden:

```javascript
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />
      <Hello />
      <Hello />
    </div>
  )
}
```

Mit React ist sehr einfach Komponenten zu erstellen und wenn man mehrere Komponenten miteinander kombiniert, lassen sich auch komplexere Anwendungen leicht verwalten. Tatsächlich ist eine Kernphilosophie von React das Kombinieren vieler spezialisierter und wiederverwendbarer Komponenten.

Eine andere Konvention ist die Idee einen Hauptkomponenten "App" an die Spitze der Appstruktur zu setzen. Trotzdem werden wir in Abschnitt 6 noch lernen, dass es Situationen gibt, wo der Komponent "App" nicht nicht ganz oben steht.

## props: passing data to components

Es ist möglich an die Komponenten Daten zu übergeben, diese werden [props](https://reactjs.org/docs/components-and-props.html) genannt.

Ändern wir den Komponenten Hello wie folgt ab:

```javascript
const Hello = (props) => {
  return (
    <div>
      <p>Hello {props.name}</p>
    </div>
  )
}
```

Jetzt übergibt die Funktion, die den Komponenten definiert, einen Parameter props. Als Argument erhält der Parameter ein Objekt, das zugehörige Felder für alle "props", die der Komponent definiert, hat.

Die props werden so definiert:

```javascript
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='George' />
      <Hello name='Daisy' />
    </div>
  )
}
```

Es kann unzählige props geben und ihre Werte können hartkodierte Strings sein oder das Ergebnis von Javascript-Ausdrücken. Wenn die Werte der props mit Javascript ermittelt werden, müssen sie in geschwungen Klammern einfügt werden.

Ändern wir den Code so ab, dass der Komponent Hello zwei props benutzt:

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
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

Die props, die vom Komponenten App überreicht werden, sind die Werte der Variablen: das Ergebnis der Evaluierung der Summe und eines normalen Strings.

## Some notes

React wurde so konfiguriert, dass es sehr klare Fehlermeldungen ausgibt. Abgesehen davon solltet ihr gerade am Anfang in sehr kleinen Schritten vorgehen und sicherstellen, das jede Änderung funktioniert.

Die Konsole sollte immer geöffnet sein. Wenn der Browser Fehlermeldungen anzeigt, ist es nicht ratsam weiter zu programmieren und auf Wunder zu hoffen. Ihr solltet stattdessen versuchen zu verstehen, was den Fehler verursacht hat und zurück zum letzten Stand gehen, der funktioniert hat:

!["fullstack content"](./bilder/abschnitt1a_bild3.png?raw=true)

Es ist immer gut, sich daran zu erinnern, dass es in React möglich ist console.log()-Befehle in den Code zu setzen.

Beachtet auch, dass React-Komponenten am Anfang groß geschrieben werden müssen. Ihr könnt versuchen einen Komponenten so zu definieren:

```javascript
const footer = () => {
  return (
    <div>
      greeting app created by <a href='https://github.com/mluukkai'>mluukkai</a>
    </div>
  )
}
```

und so zu benutzen:

```javascript
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <footer />    
    </div>
  )
}
```

Die Seite wird den Inhalt des Footer-Komponenten nicht anzeigen und stattdessen erstellt React nur ein leeres [Footer](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer)-Element, mit anderen Worten das eingebaute HTML-Element statt des React-Elements mit demselben Namen. Wenn ihr den Namen des Komponent am Anfang großschreibt, erstellt React ein div-Element für den Footer und zeigt darin seinen Inhalt an.

Beachtet, dass der Inhalt eines React-Komponenten (normalerweise) ein Hauptelement enthalten muss. Wenn wir einen Komponenten ohne ein Hauptelement definieren: 

```javascript
const App = () => {
  return (
    <h1>Greetings</h1>
    <Hello name='Maya' age={26 + 10} />
    <Footer />
  )
}
```

ist das Ergebnis eine Fehlermeldung

!["fullstack content"](./bilder/abschnitt1a_bild4.png?raw=true)

Ein Hauptelement zu verwenden, ist nicht die einzige Möglichkeit, die hier funktioniert. Ein Array wäre auch möglich:

```javascript
const App = () => {
  return [
    <h1>Greetings</h1>,
    <Hello name='Maya' age={26 + 10} />,
    <Footer />
  ]
}
```

Jedoch ist dies beim Definieren des Hauptkomponenten nicht besonders ratsam und es macht den Code hässlich.

Weil ein Hauptelement vorgeschrieben ist, haben wir ein Extra-div-Element in der DOM. Das kann man vermeiden, indem man [Fragemente](https://reactjs.org/docs/fragments.html#short-syntax) verwendet, mit anderen Worten, man umschließt den Inhalt des Komponenten mit einem leeren Element:

```javascript
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}
```

Jetzt ist die Kompilierung erfolgreich und die von React generierte DOM enthält nicht länger ein unnötiges div-Element.

## Do not render objects

Überlegt euch eine Anwendung, die die Namen und das Alter unserer Freunde am Monitor ausgibt:

```Javascript
const App = () => {
  const friends = [
    { name: 'Peter', age: 4 },
    { name: 'Maya', age: 10 },
  ]

  return (
    <div>
      <p>{friends[0]}</p>
      <p>{friends[1]}</p>
    </div>
  )
}

export default App
```

Allerdings erscheint nichts am Monitor. Ich habe 15 Minuten lang versucht das Problem zu finden, aber ich habe es trotzdem nicht gefunden.

Schließlich habe ich mich an das Versprechen erinnert, das wir gemacht haben:

  Ich verspreche die Browserkonsole die ganze Zeit geöffnet zu haben, während des Kurses und für mein restliches Leben, wenn ich Webentwicklung betreibe.

Die Konsole schreit in rot:

!["fullstack content"](./images/part1a_image5.png?raw=true)

Die Ursache des Problems ist, dass Objekte keine validen React-Kinder sind, was bedeutet, dass die Anwendung versucht Objekte anzuzeigen und immer wieder scheitert.

Der Code versucht die Informationen eines Freundes wie folgt anzuzeigen:

```javascript
<p>{friends[0]}</p>
```

und das verursacht das Problem, weil das Element in den geschweiften Klammern, das angezeigt werden soll, ein Objekt ist.

```javascript
{ name: 'Peter', age: 4 }
```

In React müssen die individuellen Elemente in geschweiften Klammern, die angezeigt werden sollen, primitive Werte sein, also z.B. Strings oder Nummern.

Die Lösung sieht so aus:

```javascript
const App = () => {
  const friends = [
    { name: 'Peter', age: 4 },
    { name: 'Maya', age: 10 },
  ]

  return (
    <div>
      <p>{friends[0].name} {friends[0].age}</p>
      <p>{friends[1].name} {friends[1].age}</p>
    </div>
  )
}

export default App
```

Jetzt wird der Name des Freundes gesondert innerhalb der geschweiften Klammern angezeigt

```javascript
{friends[0].name}
```

und auch sein Alter

```javascript
{friends[0].age}
```

Nach der Korrektur des Fehlers solltet ihr die Fehlermeldungen in eurer Browserkonsole löschen, in dem ihr 🚫 betätigt, danach die Seite aktualisieren und sicherstellen, dass keine weiteren Fehlermeldungen angezeigt werden.

Ein kleiner Hinweis zum vorherigen. React erlaubt auch das Anzeigen von Arrays, wenn das Array Werte enthält, die angezeigt werden könnten (also z.B. Strings oder Nummern).

```javascript
const App = () => {
  const friends = [ 'Peter', 'Maya']

  return (
    <div>
      <p>{friends}</p>
    </div>
  )
}
```

In diesem Abschnitt ist der Versuch vom direkten Anzeigen von Tabellen wertlos, wir werden später darauf zürückkommen. 

## Exercises

Die Aufgaben werden über GitHub eingereicht und müssen im Submissionsystem als erledigt markiert werden.

Ihr könnt alle Aufgaben über ein Repository einreichen oder verschiedene Repositories benutzen. Bitte benennt eure Verzeichnisse korrekt, wenn ihr Aufgaben verschiedener Abschnitte im selben Repository einreicht. 

Ein Beispiel für eine gelungene Benennung eurer Verzeichnisse könnte so aussehen:

```
part0
part1
  courseinfo
  unicafe
  anecdotes
part2
  phonebook
  countries
```

Für jeden Abschnitt des Kurses gibt es ein Verzeichnis, das sich weiter in verschiedene Verzeichnisse aufteilt, die die Aufgaben enthalten, wie z.B. "unicafe" in Abschnitt 1.

Für jede Webapplikation gibt es mehrere Aufgaben. Es wird empfohlen immer alle Dateien, die zu einer Anwendung gehören, einzureichen, abgesehen vom Verzeichnis node_modules.

Die Aufgaben werden Abschnitt für Abschnitt eingereicht. Wenn ihr Aufgaben für einen Abschnitt eingereicht habt, könnt ihr nicht länger noch fehlende Aufgaben für diesen Abschnitt abgeben.

Beachtet, dass es in diesem Abschnitt noch mehr Aufgaben gibt als die folgenden:

### 1.1: course information, step1

Die Anwendung, mit der wir in dieser Übung anfangen, werden wir in den nächsten Übungen fortführen. Für dieses und andere Kapitel reicht es aus, nur den Endstand der Anwendung einzureichen. Wenn ihr wollt, könnte ihr für jede einzelne Aufgabe einen Commit erstellen, aber das ist komplett optional.

Benutzt create-react-app, um die Anwendung zu initialisieren. Ändert index.js wie folgt ab:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

und App.js so:

```javascript
const App = () => {
  const course = 'Half Stack application development'
  const part1 = 'Fundamentals of React'
  const exercises1 = 10
  const part2 = 'Using props to pass data'
  const exercises2 = 7
  const part3 = 'State of a component'
  const exercises3 = 14

  return (
    <div>
      <h1>{course}</h1>
      <p>
        {part1} {exercises1}
      </p>
      <p>
        {part2} {exercises2}
      </p>
      <p>
        {part3} {exercises3}
      </p>
      <p>Number of exercises {exercises1 + exercises2 + exercises3}</p>
    </div>
  )
}

export default App
```

und löscht alle unnötigen Dateien (App.css, App.test.js, index.css, logo.svg, setupTests.js, reportWebVitals.js).

Leider ist die ganze Anwendung im selben Komponenten. Verändert den Code so, dass sie aus drei neuen Komponenten besteht: Header, Content und Total. Alle Daten verbleiben im App-Komponenten, der die benötigten Daten über props an jeden Komponenten weiterreicht.

Definiert die Komponenten in App.js.

Der App-Komponent sollte ungefähr so aussehen:

```javascript
const App = () => {
  // const-definitions

  return (
    <div>
      <Header course={course} />
      <Content ... />
      <Total ... />
    </div>
  )
}
```

ACHTUNG: create-react-app wandelt das Projekt in ein git-Repository um, sofern es nicht innerhalb eines bereits bestehendenen Repositorys erstellt wurde. Ihr wollt eurer Projekt sicherlich nicht in ein Repository umwandeln, also gebt den Befehl "rm -rf .git" im Hauptverzeichnis eures Projektes ein.

### 1.2: course information, step2

Ändert den Content-Komponenten so ab, dass dieser keine Namen von Abschnitten oder Zahlen der Aufgaben selbst anzeigt, sondern drei Parts-Komponenten, die jeweils den Abschnittsnamen oder die Zahl der Aufgaben des Abschnitts anzeigen.

```javascript
const Content = ... {
  return (
    <div>
      <Part .../>
      <Part .../>
      <Part .../>
    </div>
  )
}
```

Unsere Anwendung stellt Informationen auf recht primitive Weise dar, da die Informationen auf individuellen Variablen basieren. Diese Situation werden wir in Kürze verbessern.

[zurück zu Abschnitt 1](abschnitt_1.md)

[weiter zu Abschnitt 1b](abschnitt_1b.md)
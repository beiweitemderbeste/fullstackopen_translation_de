# Abschnitt_1a

## Inhaltsverzeichnis

- [Component](#Component)
- [JSX](#JSX)
- [Multiple Components](#Multiple-Components)
- [props: passing data to components](#props-passing-data-to-components)
- [Some notes](#Some-notes)
- [Exercises](#Exercises)

Wir befassen uns jetzt mit dem wahrscheinlich wichtigstem Thema dieses Kurses, der React-Bibliothek. Wir fangen an, indem wir eine einfache React-Anwendung erstellen und uns mit den Grundkonzepten von React auseinandersetzen.

Der bei weitem einfachste Weg zu starten ist "create-react-app" zu benutzen. Es ist möglich (aber nicht notwendig) create-react-app auf eurem Rechner zu installieren, wenn npm mit Nodejs (mind. Version 5.3) installiert wurde.

Wir beginnen, indem wir die Anwendung "part1" erstellen und in das Verzeichnis wechseln:

```
npx create-react-app part1
cd part1
```

Die Anwendung wird folgendermaßen gestartet:

```
npm start
```

Standardmäßig läuft die Anwendung unter http://localhost:3000

Euer Standardbrowser sollte sie automatisch öffnen. Öffnet direkt die Browserkonsole:

!["fullstack content"](./bilder/abschnitt1a_bild1.png?raw=true)

Der Quellcode der Anwendung liegt im src-Verzeichnis. Wir vereinfachen den Inhalt der Datei index.js, so dass er so aussieht:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

und die Datei App.js sieht so aus:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)

export default App
```

Die Dateien App.css, App.test.js, index.css, logo.svg, setupTests.js und reportWebVitals.js können gelöscht werden, weil wir sie momentan nicht benötigt werden. 

Wenn ihr folgenden Fehler angezeigt bekommt:

!["fullstack content"](./bilder/abschnitt1a_bild2.png?raw=true)

liegt das daran, dass ihr aus irgeneinem Grund eine ältere React-Version als die aktuelle (18) benutzt.

Die Lösung dafür ist index.js so abzuändern:

```javascript
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

Ihr müsst das dann wahrscheinlich auch in euren anderen Projekten machen.

Schaut [hier](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps/#a-note-on-react-version) nach, wenn ihr mehr über die Versionsunterschieden lesen wollt.

## Component

Die Datei App.js definiert einen React-Komponenten "App". Der Befehl in der letzten Zeile von index.js

```javascript
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

stellt die Inhalte von diesem App-Komponenten einem div-Element mit der id "root" dar. Das div-Element wird in der Datei public/index.html eingefügt.

Standardäßig enthält public/index.html kein HTML, das für uns im Browser sichtbar ist. Ihr könnt versuchen HTML in diese Datei zu schreiben, aber normalerweise wird in React jeder Inhalt über Komponenten dargestellt.

Schauen wir uns den Code, der den Komponenten definiert, ein bisschen genauer an:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)
```

Wir ihr wahrscheinlich schon geraten habt, wird der Komponent als div-Tag angezeigt, der einen p-Tag mit dem Text "Hello world" umschließt.

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

Es gibt verschiedene Wege, um Funktionen in Javascript zu definieren. Wir verwenden hier Pfeilfunktionen, die in einer neueren Version von Javascript (bekannt als ECMAScript 6) beschrieben werden.

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

Es sieht so aus als würden React-Komponenten HTML ausgeben, was aber nicht der Fall ist. React-Kompenenten werden meistens in JSX geschrieben. Auch wenn JSX wie HTML aussieht, handelt es sich tatsächlich um eine andere Art Javascript zu schreiben:

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

Die Kompilierung wird von "babel" übernommen. Projekte, die mit create-react-app erstellt wurden, sind so kofiguriert, das die Kompilierung automatisch geschieht. Wir werden uns in Abschnitt 7 dieses Kurs näher mit diesem Thema beschäftigen.

Es ist möglich, React in "purem" Javascript zu schreiben ohne JSX zu verwenden. Allerdings würde das niemand, der bei klarem Verstand ist, tun.

In der Praxis sieht JSX ähnlich wie HTML aus, mit dem Unterschied, dass man dynamischen Inhalt leicht einfügen kann, indem man Javascript in geschwungenen Klammern einfügt. Die Grundgedanke von JSX ist sehr ähnlich zu anderen Template-Sprachen, wie z.B. Thymeleaf für Java Spring.

JSX ist ähnlich wie XML, was bedeudet, dass jeder Tag geschlossen werden muss. Zum Beispiel ist ein Zeilenumbruch ein leeres Element, dass in HTML so geschrieben werden kann:

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
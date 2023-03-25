# Abschnitt_1b

## Inhaltsverzeichnis

- [Variables](#Variables)
- [Arrays](#Arrays)
- [Objects](#Objects)
- [Functions](#Functions)
- [Exercises](#Exercises)
- [Objects Methods and this](#Objects-Methods-and-this)
- [Classes](#Classes)
- [Javascript Materials](#Javascript-Materials)

Während des Kurses werden wir zusätzlich zur Webentwicklung viel Javascript lernen.

Bei Javascript hat sich in den letzten Jahren viel getan und in diesem Kurz greifen wir auf einige Features der neuen Versionen zu. Der offizielle Name des Javascript-Standards ist [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript). Im Moment ist die aktuelleste Version von Juni 2022 [ECMAScript®2022](https://www.ecma-international.org/ecma-262/), auch bekannt als ES13.

Nicht alle Browser unterstützen schon alle neue Javascript-Features. Aus diesem Grund wird ein großer Teil des Codes, der im Browser läuft, von einer neueren in eine ältere, kompatiblere Version von Javascript übersetzt.

Heutzutage geschieht dies am häufigsten mit [Babel](https://babeljs.io/). Die Übersetzung wird in React-Anwendungen, die mit create-react-app erstellt wurden, automatisch eingestellt. Wir schauen uns die Konfiguration dieser Übersetzung in Abschnitt 7 noch genauer an.

[Node.js](https://nodejs.org/en/) ist eine Javascript-Laufzeitumgebung, die auf Googles [Chrome V8](https://developers.google.com/v8/) Javascript Engine basiert, und praktisch auf allen Geräten läuft, von Servern bishin zu mobilen Telefonen. Üben wir ein bisschen das Schreiben von Javascript, indem wir Node benutzen. Die neueste Version von Node.js versteht auch die neuesten Versionen von Javascript, so dass dieser Code nicht übersetzt werden muss.

Unser Code wird in Dateien geschrieben, deren Dateinamen mit ".js" enden und mit dem Befehl "node dateiname.js" gestartet werden.

Man kann auch Javascript-Code in die Node.js-Konsole tippen (diese öffnet man durch den Befehl "node" im Terminal). Ebenso könnt ihr auch Javascript in der Entwicklerkonsole eures Browsers tippen. Die aktuellen Versionen von Firefox können mit den neuen Features von Javascript ganz gut umgehen, ohne den Code vorher übersetzen zu müssen. Alternativ könnt ihr auch Werkzeuge wie [JS Bin](https://jsbin.com/?js,console) verwenden.

Javascript erinnert vom Namen und Syntax an Java. Aber wenn es um die Kerneigenschaften der Sprachen geht, könnten sie nicht unterschiedlicher sein. Wenn man einen Java-Background hat, mag einem Javascript ein bisschen fremd vorkommen, besonders wenn man keine Anstrengung unternimmt seine Features nachzuschauen.

In bestimmten Kreisen ist es beliebt Eigenschaften von Java zu simulieren und in Javascript zu schreiben. Das wird von unserer Seite nicht empfohlen, da die Sprachen und ihre zugehörigen Ökosysteme sehr verschieden sind.

## Variables

In JavaScript gibt es mehrere Wege Variablen zu definieren:

```javascript
const x = 1
let y = 5

console.log(x, y)   // 1, 5 are printed
y += 10
console.log(x, y)   // 1, 15 are printed
y = 'sometext'
console.log(x, y)   // 1, sometext are printed
x = 4               // causes an error
```

[const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) definiert keine Variable sondern eine Konstante, deren Wert nicht geändert werden kann. Auf der anderen Seite definiert [let eine normale Variable.

Im obigen Beispiel sehen wir auch, dass sich der Datentyp von Variablen während der Ausführung ändert. Am Anfang speichert y einen Integer, am Ende speichert es einen String.

In Javascript ist es möglich mit dem Schlüsselwort var Variablen zu definieren. var war für eine lange Zeit der einzige Weg, um Variablen zu definieren. const und let wurden erst kürzlich in Version ES6 hinzugefügt. In bestimmten Situationen arbeitet var anders als Variabeldefinitionen in den meisten Sprachen - lest dazu mehr unter [JavaScript Variables - Should You Use let, var or const? on Medium](https://medium.com/craft-academy/javascript-variables-should-you-use-let-var-or-const-394f7645c88f) oder [Keyword: var vs. let on JS Tips](http://www.jstips.co/en/javascript/keyword-var-vs-let/). In diesem Kurs var zu benutzen ist unvernünftig und ihr solltet bei const und let bleiben! Ihr könnt zu diesem Thema mehr auf YouTube finden - z.B. [var, let and const - ES6 JavaScript Features](https://youtu.be/sjyJBL5fkp8)

## Arrays

Ein Array und ein paar Beispiele, wie es verwendet werden kann:

```javascript
const t = [1, -1, 3]

t.push(5)

console.log(t.length) // 4 is printed
console.log(t[1])     // -1 is printed

t.forEach(value => {
  console.log(value)  // numbers 1, -1, 3, 5 are printed, each to own line
})  
```

Beachtet bitte, dass in diesem Beispiel die Inhalte von dem Array geändert werden können, obwohl es als Konstante definiert worden ist. Weil das Array ein Objekt ist, zeigt die Variable immer auf das gleiche Objekt. Allerdings ändert sich der Inhalt des Arrays, wenn neue Werte hinzugefügt werden.

Ein Möglichkeit, um eine Schleife durch das Array laufen zu lassen, ist "forEach". "forEach" erhält als Parameter eine Pfeilfunktion.

```javascript
value => {
  console.log(value)
}
```

forEach ruft die Funktion für jeden Wert des Arrays auf und übergibt immer den jeweiligen Wert als Argument an die Funktion. Der Funktion können als Argument der forEach-Schleife auch [andere Argumente](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) übergeben werden.

Im vorangegangenen Beispiel wurde neue Werte mit der Methode [push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) zum Array hinzugefügt. Wenn React verwendet wird, nutzt man oft Techniken aus der funktionalen Programmierung. Eine Charakteristik des funktionalen Programmierungsparadigmas ist das Benutzen von [unveränderbaren](https://en.wikipedia.org/wiki/Immutable_object) Datenstrukturen. In React wird oft die Methode [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) verwendet. Diese hängt keinen neuen Wert an das Array, sondern erstellt ein neues Array, dass den Inhalt des alten Arrays und den neuen Wert enthält.

```javascript
const t = [1, -1, 3]

const t2 = t.concat(5)

console.log(t)  // [1, -1, 3] is printed
console.log(t2) // [1, -1, 3, 5] is printed
```

Der Aufruf der Methode t.concat(5) hängt keinen neuen Wert an das Array t, sondern gibt ein neues Array aus, das die Werte von t und den Wert 5 enthält.

Es gibt viele nützliche Methoden, die für Arrays definiert worden sind. Schauen wir uns ein kurzes Beispiel der Methode [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) an:

```javascript
const t = [1, 2, 3]

const m1 = t.map(value => value * 2)
console.log(m1)   // [2, 4, 6] is printed
```

Basierend auf dem Array t, erstellt map ein neues Array. Dieses bekommt als Parameter eine Funktion übergeben, die dafür genutzt wird, Werte des Arrays zu erstellen.

map kann also ein Array in etwas ganz anderes verwandeln:

```javascript
const m2 = t.map(value => '<li>' + value + '</li>')
console.log(m2)  
// [ '<li>1</li>', '<li>2</li>', '<li>3</li>' ] is printed
```

Hier wird ein Array, das nur Integers enthält, in ein Array umgewandelt, dass nur HTML-Strings enthält. Im zweiten Abschnitt des Kurses werden wir sehen, dass die map-Methode sehr häufig in React zum Einsatz kommt.

Einzelne Werte eines Arrays können leicht über [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) Variablen zugewiesen werden.

```javascript
const t = [1, 2, 3, 4, 5]

const [first, second, ...rest] = t

console.log(first, second)  // 1, 2 is printed
console.log(rest)          // [3, 4, 5] is printed
```

Hier werden den Variablen first und second die ersten beiden Integer des Arrays als Werte zugewiesen. Die verbliebenen Integer werden gesammelt einem eigenen Array zugewiesen, das selbst der Variable "rest" zugewiesen wird.

## Objects

Es gibt verschiedene Möglichkeiten, um Objekte in Javascript zu definieren. Eine häufig verwendete ist die Benutzung von [object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals), das die Eigenschaften eines Objekts innerhalb geschwungener Klammern darstellt:

```javascript
const object1 = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
}

const object2 = {
  name: 'Full Stack web application development',
  level: 'intermediate studies',
  size: 5,
}

const object3 = {
  name: {
    first: 'Dan',
    last: 'Abramov',
  },
  grades: [2, 3, 5, 3],
  department: 'Stanford University',
}
```

Die Werte der Eigenschaften können von jedem Datentyp sein, wie z.B. Integer, Strings, Arrays, andere Objekte...

Die Eigenschaften eines Objekts können über die Punktnotierung oder über eckige Klammern zugeordnet werden:

```javascript
console.log(object1.name)         // Arto Hellas is printed
const fieldName = 'age' 
console.log(object1[fieldName])    // 35 is printed
```

Man kann über die Punktnotierung bzw. eckige Klammern dem Objekt auch neue Eigenschaften hinzufügen:

```javascript
object1.address = 'Helsinki'
object1['secret number'] = 12341
```

Die untere Ergänzung muss über eckige Klammern erfolgen, da bei der Punktnotierung kein Leerzeichen im Namen sein darf.

Selbstverständlich können Objekte in Javascript auch Methoden haben. Allerdings werden wir in diesem Kurs keine Objekte mit eigenen Methoden definieren. Deswegen sprechen wir sie auch nur kurz an.

Objekte können auch über sogenannte Constructor-Funktionen definiert werden, was an Techniken anderer Programmiersprachen erinnert, z.B. Javas Klassen. Abgesehen von dieser Ähnlichkeit, hat Javascript nichts ähnliches wie Klassen aus objektorientierten Programmiersprachen. Es gab allerdings eine Ergänzung zur Klassensyntax in Version ES6, die es in einigen Fällen ermöglicht, objektorientierte Klassen zu erstellen.

## Functions

Wir wissen bereits, wie man Pfeilfunktionen definiert. Der komplette Prozess dafür lautet:

```javascript
const sum = (p1, p2) => {
  console.log(p1)
  console.log(p2)
  return p1 + p2
}
```

und die Funktion wird erwartet aufgerufen:

```javascript
const result = sum(1, 5)
console.log(result)
```

Wenn nur ein Parameter übergeben wird, kann man die runden Klammern weglassen:

```javascript
const square = p => {
  console.log(p)
  return p * p
}
```

Wenn die Funktion nur einen einzigen Ausdruck enthält, können auch die geschwungenen Klammern weggelassen werden. In diesem Fall gibt die Funktion nur das Ergebnis ihres einzigen Ausdrucks aus. Wenn wir jetzt noch den Befehl "console.log" weglassen, können wir die Definition der Funktion wir folgt kürzen:

```javascript
const square = p => p * p
```

Diese Form ist besonders nützlich, wenn man Arrays verändert, z.B. mit der map-Methode:

```javascript
const t = [1, 2, 3]
const tSquared = t.map(p => p * p)
// tSquared is now [1, 4, 9]
```

Pfeilfunktionen gibt es erst seit Version [ES6](http://es6-features.org/). Vorher gab es nur die Möglichkeit mit dem Schlagwort function Funktionen zu definieren.

Es gibt zwei Arten, um auf Funktionen zu verweisen. Eine davon ist, einen Namen in der [Funktionsdeklaration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function) zu verwenden:

```javascript
function product(a, b) {
  return a * b
}

const result = product(2, 6)
// result is now 12
```

Die andere Art ist die Funktion durch einen [Funktionsaudruck](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function) zu definieren. In diesem Fall wird für die Funktion kein Name vergeben und die Definition der Funktion kann im übrigen Code bleiben:

```javascript
const average = function(a, b) {
  return (a + b) / 2
}

const result = average(2, 5)
// result is now 3.5
```

In diesem Kurs definieren wir Funktionen nur als Pfeilfunktionen.

## Exercises

Wir arbeiten weiter an der Anwendung, mit wir schon in den vorangegangen Aufgaben gearbeitet haben. Ihr könnt mit dem gleichen Code weiterarbeiten, da nur der Endstand der eingereichten Anwendung erforderlich ist.

Hinweis: Ihr könnt Probleme bekommen, wenn es um die Struktur geht, mit der props an Komponenten weitergegeben werden. Um zu wissen, um welche props es geht, könnt ihr sie einfach auf der Konsole ausgeben, z.B. so:

```javascript
const Header = (props) => {
  console.log(props)
  return <h1>{props.course}</h1>
}
```

### 1.3 course information step3

Machen wir weiter, indem wir Objekte in unserer Anwendung verwenden. Ändert die Definitionen der Variablen im App-Komponenten wie folgt ab und dann sorgt dafür, dass die Anwendung weiterhin funktioniert:

```javascript
const App = () => {
  const course = 'Half Stack application development'
  const part1 = {
    name: 'Fundamentals of React',
    exercises: 10
  }
  const part2 = {
    name: 'Using props to pass data',
    exercises: 7
  }
  const part3 = {
    name: 'State of a component',
    exercises: 14
  }

  return (
    <div>
      ...
    </div>
  )
}
```

### 1.4 course information step4

Fügt nun alle Objekte in ein Array. Ändert wieder die Definitionen der Variablen wie folgt ab und passt wieder den Rest der Anwendung an, damit sie weiterhin funktioniert:

```javascript
const App = () => {
  const course = 'Half Stack application development'
  const parts = [
    {
      name: 'Fundamentals of React',
      exercises: 10
    },
    {
      name: 'Using props to pass data',
      exercises: 7
    },
    {
      name: 'State of a component',
      exercises: 14
    }
  ]

  return (
    <div>
      ...
    </div>
  )
}
```

Hinweis: Ab jetzt könnt ihr immer davon ausgehen, dass es sich um 3 Teile handelt, also müsst ihr keine Schleifen für das Array schreiben. Wir kommen auf das Thema - Komponenten im Bezug auf die Werte in einem Array zu erstellen - im nächsten Abschnitt zurück.

Übergebt allerdings nicht die verschiedenen Objekte als separate props vom App-Komponenten an die Komponenten Content und Total. Übergebt sie stattdessen direkt als Array:

```javascript
const App = () => {
  // const definitions

  return (
    <div>
      <Header course={course} />
      <Content parts={parts} />
      <Total parts={parts} />
    </div>
  )
}
```

### 1.5 course information step5

Gehen wir noch einen Schritt weiter. Macht den Kurs und seine Teile zu einem Javascript-Objekt. Behebt alle resultierenden Probleme.

```javascript
const App = () => {
  const course = {
    name: 'Half Stack application development',
    parts: [
      {
        name: 'Fundamentals of React',
        exercises: 10
      },
      {
        name: 'Using props to pass data',
        exercises: 7
      },
      {
        name: 'State of a component',
        exercises: 14
      }
    ]
  }

  return (
    <div>
      ...
    </div>
  )
}
```

## Object methods and this

Da wir in diesem Kurs eine Reactversion verwenden, die React Hooks kennt, müssen wir nicht wissen, wie man Objekte mit Methoden erstellt. Das Folgende ist nicht relevant für den weiteren Kurs, aber trotzdem gut zu wissen. Besonders, wenn man ältere Reactversionen pflegt.

Pfeilfunktionen und Funktionen, die mit dem Schlagwort function erstellt wurden, unterscheiden sich erheblich, wenn es darum geht, wie mit [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) umgegangen wird.

Wir können einem Objekt Methoden zuweisen, indem wir Objekteigenschaften als Funktionen definieren:

```javascript
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

arto.greet()  // "hello, my name is Arto Hellas" gets printed
```

Methoden können einem Objekt auch nach dessen Erstellung zugewiesen werden:

```javascript
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

arto.growOlder = function() {  
  this.age += 1
}

console.log(arto.age)   // 35 is printed
arto.growOlder()
console.log(arto.age)   // 36 is printed
```

Ändern wir das Objekt leicht ab:

```javascript
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
  doAddition: function(a, b) {    
    console.log(a + b)  
  },
}

arto.doAddition(1, 4)        // 5 is printed

const referenceToAddition = arto.doAddition
referenceToAddition(10, 15)   // 25 is printed
```

Jetzt hat das Objekt die Methode "doAddition", die die Summe zweier Zahlen errechnet, die als Parameter übergeben werden. Die Methode kann entweder über das Objekt aufgerufen werden

```javascript
arto.doAddition(1, 4)
```

oder indem der Verweis auf die Methode in einer Variable gespeichert wird und die Variable aufgerufen wird

```javascript
referenceToAddition(10, 15)
```

Wenn wir das gleiche mit der greet-Methode machen, laufen wir in ein Problem:

```javascript
arto.greet()       // "hello, my name is Arto Hellas" gets printed

const referenceToGreet = arto.greet
referenceToGreet() // prints "hello, my name is undefined"
```

Wenn wir die Methode über einen Verweis aufrufen, verliert die Methode das Wissen, auf wen "this" verweist. Im Gegensatz zu anderen Sprachen beruht in Javascript die Definition von [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) darauf, wie die Methode aufgerufen wurde. Geschieht der Aufruf über einen Verweis, zeigt der Wert von "this" auf das sogenannte [globale Objekt](https://developer.mozilla.org/en-US/docs/Glossary/Global_object) und das Endergebnis ist oft nicht das, was der Softwareentwickler im Sinn hatte.

Wenn man den Überblick darüber verliert, auf wen "this" verweist, bekommt man Probleme. Es gibt öfter Situationen, in denen React oder Node (genauer gesagt, die Javascript-Engine des Browsers) eine Methode eines Objektes aufruft, die der Entwickler definiert hat. In diesem Kurs umgehen wir diese Probleme, indem wir wenig "this" verwenden.

Eine Möglichkeit, um "this" verschwinden zu lassen, ist einen Timeout zu setzen, der die Funktion greet aufruft. Wir verwenden dafür die Funktion [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout):

```javascript
const arto = {
  name: 'Arto Hellas',
  greet: function() {
    console.log(this);
    console.log('hello, my name is ' + this.name)
  },
}

setTimeout(arto.greet, 1000)
```

Wie bereits erwähnt wird der Wert von "this" in Javascript darüber bestimmt, wie die Methode aufgrufen wird, zum jetzigen Punkt verweist "this" auf das globale Objekt.

Es gibt verschiedene Techniken, wie man das originiale "this" erhalten kann. Eine davon ist die Methode bind zu nutzen: 

```javascript
setTimeout(arto.greet.bind(arto), 1000)
```

Der Funktionsaufruf arto.greet.bind(arto) erstellt eine neue Funktion, bei der "this" an Arto gebunden ist, unabhängig davon, wie die Methode greet aufgerufen wird.

Indem man [Pfeilfunktionen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) nutzt, ist es möglich einige Probleme zu umgehen, die mit "this" verbunden sind. Allerdings sollten sie nicht für Objektmethoden verwendet werden, weil dann "this" überhaupt nicht funktioniert. Wir kommen später nochmal auf die Beziehung von Pfeilfunktionenn und "this" zurück.

Wenn ihr ein besseres Verständnis für "this" in Javascript bekommen wollt, dann empfehlen wir unbedingt die Screencastserie [Understand JavaScript's this Keyword in Depth](https://egghead.io/courses/understand-javascript-s-this-keyword-in-depthf) von [egghead.io](egghead.io).

## Classes

Wie bereits erwähnt gibt es in Javascript keine Klassen, die mit denen von objektorientierten Sprachen vergleichbar sind. Allerdings gibt es Features mit denen sich objektorientierte [Klassen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) simulieren lassen.

Schauen wir uns kurz die Klassensyntax an, die mit ES6 eingeführt wurde. Diese Syntax vereinfachte die Definition von Klassen in Javascript ungemein.

Im folgenden Beispiel definieren wir eine Klasse "Person" und zwei Person-Objekte:

```javascript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  greet() {
    console.log('hello, my name is ' + this.name)
  }
}

const adam = new Person('Adam Ondra', 29)
adam.greet()

const janja = new Person('Janja Garnbret', 23)
janja.greet()
```

Diese Syntax erinnert stark daran, wie Klassen und Objekte in Java erstellt werden. Ihr Verhalten ist dem von Java-Objekten auch sehr ähnlich. Im Kern sind sie aber immer noch Objekte, die auf Javascripts [Prototypenvererbung](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance) basieren. Der Datentyp von beiden Objekten ist tatsächlich Object, weil Javascript im Wesentlichen nur 7 Datentypen kennt: [Boolean, Null, Undefined, Number, String, Symbol, BigInt und Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures).

Die Einführung von Klassen war eine kontroverse Ergänzung. Schaut euch [Not Awesome: ES6 Classes](https://github.com/petsel/not-awesome-es6-classes) oder [Is “Class” In ES6 The New “Bad” Part?](https://medium.com/@rajaraodv/is-class-in-es6-the-new-bad-part-6c4e6fe1ee65) für mehr Details an.

Diese Klassensyntax aus ES6 wird viel in älteren React- und Node.js-Projekten verwendet, daher ist ihr Verstehen vorteilhaft. Allerdings nutzen wir in diesem Kurs die neuen [Hooks](https://reactjs.org/docs/hooks-intro.html) von React, daher gibt gibt es für die Klassen keine direkte Anwendung.

## Javascript Materials

Es gibt gute und schlechte Anleitungen für Javascript im Internet. Die meisten Links zu Javascript auf dieser Seite verweisen auf [Mozilla's JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

Wir empfehlen dringend [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) auf der Webseite von Mozilla.

Wenn ihr euch richtig gut mit Javascript auskennen wollt, es gibt da eine großartige, kostenfreie Buchserie: [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)

Eine andere großartige Quelle, um Javascript zu lernen, ist [javascript.info](https://javascript.info/)

Das kostenfreie und sehr fesselnde Buch [Eloquent Javascript](https://eloquentjavascript.net). Bringt euch sehr schnell von den Grundlagen zum interessanten Zeug, eine Mischung aus theoretischen Projekten und Aufgaben, es geht sowohl um generelle Programmierkenntnisse, sowie um Javascript an sich.

[egghead.io](egghead.io) hat viele qualitativ vervollte Screencasts zu Javascript, React und anderen interessanten Themen. Leider ist ein Teil des Materials hinter einer Zahlwand.

[zurück zu Abschnitt 1a](abschnitt_1a.md)

[weiter zu Abschnitt 1c](abschnitt_1c.md)
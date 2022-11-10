# part 1b

> table of contents

## Inhaltsverzeichnis

- [Variables](#Variables)
- [Arrays](#Arrays)
- [Objects](#Objects)
- [Functions](#Functions)
- [Exercises](#Exercises)
- [Objects Methods and this](#Objects-Methods-and-this)
- [Classes](#Classes)
- [Javascript Materials](#Javascript-Materials)

> During the course, we have a goal and a need to learn a sufficient amount of JavaScript in addition to web development.

Während des Kurses werden wir viel Javascript zusätzlich zur Webentwicklung lernen.

> JavaScript has advanced rapidly in the last few years and in this course we use features from the newer versions. The official name of the JavaScript standard is ECMAScript. At this moment, the latest version is the one released in June of 2021 with the name ECMAScript®2021, otherwise known as ES12.

Bei Javascript hat sich in den letzten Jahren viel getan und in diesem Kurz greifen wir auf einige Features der neuen Versionen zu. Der offizielle Name des Javascript-Standards ist ECMAScript. Im Moment ist die aktuelleste Version von Juni 2021 "ECMAScript®2021", auch bekannt als ES12.

> Browsers do not yet support all of JavaScript's newest features. Due to this fact, a lot of code run in browsers has been transpiled from a newer version of JavaScript to an older, more compatible version.

Nicht alle Browser unterstützen schon alle neue Javascript-Features. Aus diesem Grund wird ein großer Teil des Codes, der im Browser läuft, von einer neueren in eine ältere, kompatiblere Version von Javascript übersetzt.

> Today, the most popular way to do the transpiling is by using Babel. Transpilation is automatically configured in React applications created with create-react-app. We will take a closer look at the configuration of the transpilation in part 7 of this course.

Heutzutage geschieht dies am meisten mit "Babel". Die Übersetzung wird in React-Anwendungen, die mit create-react-app erstellt wurden, automatisch eingestellt. Wir schauen uns die Konfiguration dieser Übersetzung in Abschnitt 7 noch genauer an.

> Node.js is a JavaScript runtime environment based on Google's Chrome V8 JavaScript engine and works practically anywhere - from servers to mobile phones. Let's practice writing some JavaScript using Node. It is expected that the version of Node.js installed on your machine is at least version 16.13.2. The latest versions of Node already understand the latest versions of JavaScript, so the code does not need to be transpiled.

Node.js ist eine Javascript-Laufzeitumgebung, die auf Googles Chrome V8 Javascript Engine basiert, und praktisch überall läuft, von Servern bis zu mobilen Telefonen. Üben wir ein bisschen das Schreiben von Javascript, indem wir Node benutzen. Es wird erwartet, dass mindestens Version 16.12.2 von Node.js auf euren Rechnern installiert ist. Die neueste Version von Node.js versteht auch die neuesten Versionen von Javascript, so dass dieser Code nicht übersetzt werden muss.

> The code is written into files ending with .js that are run by issuing the command node name_of_file.js

Unser Code wird in Dateien geschrieben, deren Dateinamen mit ".js" enden und mit dem Befehl "node dateiname.js" gestartet werden.

> It is also possible to write JavaScript code into the Node.js console, which is opened by typing node in the command-line, as well as into the browser's developer tool console. The newest revisions of Chrome handle the newer features of JavaScript pretty well without transpiling the code. Alternatively you can use a tool like JS Bin.

Man kann auch Javascript-Code in die Node.js-Konsole tippen (diese öffnet man durch den Befehl "node" im Terminal). Ebenso könnt ihr auch Javascript in der Entwicklerkonsole eures Browsers tippen. Die aktuellen Versionen von Firefox können mit den neuen Features von Javascript ganz gut umgehen, ohne den Code vorher übersetzen zu müssen. Alternativ könnt ihr auch Werkzeuge wie [JS Bin](link-einfügen) verwenden.

> JavaScript is sort of reminiscent, both in name and syntax, to Java. But when it comes to the core mechanism of the language they could not be more different. Coming from a Java background, the behavior of JavaScript can seem a bit alien, especially if one does not make the effort to look up its features.

Javascript erinnert vom Namen und Syntax an Java. Aber wenn es um die Kerneigenschaften der Sprachen geht, könnten sie nicht unterschiedlicher sein. Wenn man einen Java-Background hat, mag einem Javascript ein bisschen fremd vorkommen, besonders wenn man keine Anstrengung unternimmt, um seine Eigenschaften nachzuschauen.

> In certain circles it has also been popular to attempt "simulating" Java features and design patterns in JavaScript. We do not recommend doing this as the languages and respective ecosystems are ultimately very different.

In bestimmten Kreisen ist es beliebt Eigenschaften von Java zu simulieren und in Javascript zu schreiben. Das wird von unserer Seite nicht empfohlen, da die Sprachen und ihre zugehörigen Ökosystem sehr verschieden sind.

## Arrays

> An array and a couple of examples of its use:

Ein Array und ein paar Beispiele, wie es verwendet werden kann

```javascript
const t = [1, -1, 3]

t.push(5)

console.log(t.length) // 4 is printed
console.log(t[1])     // -1 is printed

t.forEach(value => {
  console.log(value)  // numbers 1, -1, 3, 5 are printed, each to own line
})  
```

> Notable in this example is the fact that the contents of the array can be modified even though it is defined as a const. Because the array is an object, the variable always points to the same object. However, the content of the array changes as new items are added to it.

Beachtet bitte, dass in diesem Beispiel die Inhalte von dem Array geändert werden können, obwohl es als Konstante definiert worden ist. Weil das Array ein Objekt ist, zeigt die Variable immer auf das gleiche Objekt. Allerdings ändert sich der Inhalt des Arrays, wenn neue Werte hinzugefügt werden.

> One way of iterating through the items of the array is using forEach as seen in the example. forEach receives a function defined using the arrow syntax as a parameter.

Ein Möglichkeit, um eine Schleife durch das Array laufen zu lassen, ist "forEach". "forEach" erhält als Parameter eine Pfeilfunktion.

```javascript
value => {
  console.log(value)
}
```

> forEach calls the function for each of the items in the array, always passing the individual item as an argument. The function as the argument of forEach may also receive other arguments.

forEach ruft die Funktion für jeden Wert des Arrays auf und übergibt immer den jeweiligen Wert als Argument an die Funktion. Die Pfeilfunktion kann auch andere Argumente übergeben bekommen.

> In the previous example, a new item was added to the array using the method push. When using React, techniques from functional programming are often used. One characteristic of the functional programming paradigm is the use of immutable data structures. In React code, it is preferable to use the method concat, which does not add the item to the array, but creates a new array in which the content of the old array and the new item are both included.

Im vorangegangenen Beispiel wurde neue Werte mit der push-Methode zum Array hinzugefügt. Wenn React verwendet wird, nutzt man oft Techniken aus der funktionalen Programmierung. Eine Charakteristik des funktionalen Programmierungsparadigmas ist das Benutzen von unveränderbaren Datenstrukturen. In React wird oft die Methode "concat" verwendet. Diese hängt keinen neuen Wert an das Array, sondern erstellt ein neues Array, dass den Inhalt des alten Arrays und den neuen Wert enthält.

```javascript
const t = [1, -1, 3]

const t2 = t.concat(5)

console.log(t)  // [1, -1, 3] is printed
console.log(t2) // [1, -1, 3, 5] is printed
```

> The method call t.concat(5) does not add a new item to the old array but returns a new array which, besides containing the items of the old array, also contains the new item.

Der Aufruf der Methode "t.concat(5)" hängt keinen neuen Wert an das Array "t", sondern gibt ein neues Array aus, das die Werte von "t" und den Wert "5" enthält.

> There are plenty of useful methods defined for arrays. Let's look at a short example of using the map method.

Es gibt viele nützliche Methoden, die für Arrays definiert worden sind. Schauen wir uns ein kurzes Beispiel der map-Methode an

```javascript
const t = [1, 2, 3]

const m1 = t.map(value => value * 2)
console.log(m1)   // [2, 4, 6] is printed
```

> Based on the old array, map creates a new array, for which the function given as a parameter is used to create the items. In the case of this example the original value is multiplied by two.

Basierend auf dem Array t, erstellt map ein neues Array. Dieses bekommt als Parameter eine Funktion übergeben, die dafür genutzt wird, Werte des Arrays zu erstellen.

> Map can also transform the array into something completely different:

map kann also ein Array in etwas ganz anderes verwandeln:

```javascript
const m2 = t.map(value => '<li>' + value + '</li>')
console.log(m2)  
// [ '<li>1</li>', '<li>2</li>', '<li>3</li>' ] is printed
```

> Here an array filled with integer values is transformed into an array containing strings of HTML using the map method. In part 2 of this course, we will see that map is used quite frequently in React.

Hier wird ein Array, das nur Integers enthält, in ein Array umgewandelt, dass nur HTML-Strings enthält. Im zweiten Abschnitt des Kurses werden wir sehen, dass die map-Methode sehr häufig in React zum Einsatz kommt.

> Individual items of an array are easy to assign to variables with the help of the destructuring assignment.

Einzelne Werte eines Arrays können leicht über "destructuring" Variablen zugewiesen werden.

```javascript
const t = [1, 2, 3, 4, 5]

const [first, second, ...rest] = t

console.log(first, second)  // 1, 2 is printed
console.log(rest)          // [3, 4, 5] is printed
```

> Thanks to the assignment, the variables first and second will receive the first two integers of the array as their values. The remaining integers are "collected" into an array of their own which is then assigned to the variable rest.

Hier werden den Variablen "first" und "second" die ersten beiden Integer des Arrays als Werte zugewiesen. Die verbliebenen Integer werden gesammelt einem eigenen Array zugewiesen, das selbst der Variable "rest" zugewiesen wird.

## Objects

> There are a few different ways of defining objects in JavaScript. One very common method is using object literals, which happens by listing its properties within braces:

Es gibt verschiedene Möglichkeiten, um Objekte in Javascript zu definieren. Eine häufig verwendete ist die Benutzung von "object literals", das die Eigenschaften eines Objekts innerhalb geschwungener Klammern darstellt:

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

> The values of the properties can be of any type, like integers, strings, arrays, objects...

Die Werte der Eigenschaften können von jedem Datentyp sein, wie z.B. Integer, Strings, Arrays, andere Objekte...

> The properties of an object are referenced by using the "dot" notation, or by using brackets:

Die Eigenschaften eines Objekts können über die Punktnotierung oder über eckige Klammern zugeordnet werden:

```javascript
console.log(object1.name)         // Arto Hellas is printed
const fieldName = 'age' 
console.log(object1[fieldName])    // 35 is printed
```

> You can also add properties to an object on the fly by either using dot notation or brackets:

Man kann über die Punktnotierung bzw. eckige Klammern dem Objekt auch neue Eigenschaften hinzufügen:

```javascript
object1.address = 'Helsinki'
object1['secret number'] = 12341
```

> The latter of the additions has to be done by using brackets, because when using dot notation, secret number is not a valid property name because of the space character.

Die untere Ergänzung muss über eckige Klammern erfolgen, da bei der Punktnotierung kein Leerzeichen im Namen sein darf.

> Naturally, objects in JavaScript can also have methods. However, during this course we do not need to define any objects with methods of their own. This is why they are only discussed briefly during the course.

Selbstverständlich können Objekte in Javascript auch Methoden haben. Allerdings werden wir in diesem Kurs keine Objekte mit eigenen Methoden definieren. Deswegen sprechen wir sie auch nur kurz an

> Objects can also be defined using so-called constructor functions, which results in a mechanism reminiscent of many other programming languages, e.g. Java's classes. Despite this similarity, JavaScript does not have classes in the same sense as object-oriented programming languages. There has been, however, an addition of the class syntax starting from version ES6, which in some cases helps structure object-oriented classes.

Objekte können auch über sogenannte Constructor-Funktionen definiert werden, was an Techniken anderer Programmiersprachen erinnert, z.B. Javas Klassen. Abgesehen von diesr Ähnlichkeit, hat Javascript nichts ähnliches wie Klassen aus objektorientierten Programmiersprachen. Es gab allerdings eine Ergänzung zur Klassensyntax in Version ES6, die es in einigen Fällen ermöglicht, objektorientierte Klassen zu erstellen.

## Functions

> We have already become familiar with defining arrow functions. The complete process, without cutting corners, to defining an arrow function is as follows:

Wir wissen bereits, wie man Pfeilfunktionen definiert. Der komplette Prozess dafür lautet:

```javascript
const sum = (p1, p2) => {
  console.log(p1)
  console.log(p2)
  return p1 + p2
}
```

> and the function is called as can be expected:

und die Funktion wird erwartet aufgerufen:

```javascript
const result = sum(1, 5)
console.log(result)
```

> If there is just a single parameter, we can exclude the parentheses from the definition:

Wenn nur ein Parameter übergeben wird, kann man die runden Klammern weglassen:

```javascript
const square = p => {
  console.log(p)
  return p * p
}
```

> If the function only contains a single expression then the braces are not needed. In this case the function only returns the result of its only expression. Now, if we remove console printing, we can further shorten the function definition:

Wenn die Funktion nur einen einzigen Ausdruck enthält, können auch die geschwungenen Klammern weggelassen werden. In diesem Fall gibt die Funktion nur das Ergebnis ihres einzigen Ausdrucks aus. Wenn wir jetzt noch den Befehl "console.log" weglassen, können wir die Definition der Funktion wir folgt kürzen:

```javascript
const square = p => p * p
```

> This form is particularly handy when manipulating arrays - e.g. when using the map method:

Diese Form ist besonders nützlich, wenn man Arrays verändert, z.B. mit der map-Methode:

```javascript
const t = [1, 2, 3]
const tSquared = t.map(p => p * p)
// tSquared is now [1, 4, 9]
```

> The arrow function feature was added to JavaScript only a couple of years ago, with version ES6. Prior to this the only way to define functions was by using the keyword function.

Pfeilfunktionen gibt es erst seit Version ES6. Vorher gab es nur die Möglichkeit mit dem Schlagwort "function" Funktionen zu definieren.

> There are two ways to reference the function; one is giving a name in a function declaration.

Es gibt zwei Arten, um auf Funktionen zu verweisen. Eine davon ist, einen Namen in der Funktionsdeklaration zu verwenden:

```javascript
function product(a, b) {
  return a * b
}

const result = product(2, 6)
// result is now 12
```

>The other way to define the function is using a function expression. In this case there is no need to give the function a name and the definition may reside among the rest of the code:

Die andere Art ist die Funktion durch einen Funktionsaudruck zu definieren. In diesem Fall wird für die Funktion kein Name vergeben und die Definition der Funktion kann im übrigen Code bleiben:

```javascript
const average = function(a, b) {
  return (a + b) / 2
}

const result = average(2, 5)
// result is now 3.5
```

> During this course we will define all functions using the arrow syntax.

In diesem Kurs definieren wir Funktionen nur als Pfeilfunktionen.

## Exercises

> We continue building the application that we started working on in the previous exercises. You can write the code into the same project, since we are only interested in the final state of the submitted application.

Wir arbeiten weiter an der Anwendung, mit wir schon in den vorangegangen Aufgaben gearbeitet haben. Ihr könnt mit dem gleichen Code weiterarbeiten, da nur der Endstand der eingereichten Anwendung erforderlich ist.

> Pro-tip: you may run into issues when it comes to the structure of the props that components receive. A good way to make things more clear is by printing the props to the console, e.g. as follows:

Hinweis: Ihr könnt Probleme bekommen, wenn es um die Struktur geht, mit der props an Komponenten weitergegeben werden. Um zu wissen, um welche props es geht, könnt ihr sie einfach auf der Konsole ausgeben, z.B. so:

```javascript
const Header = (props) => {
  console.log(props)
  return <h1>{props.course}</h1>
}
```

### 1.3: course information step3

> Let's move forward to using objects in our application. Modify the variable definitions of the App component as follows and also refactor the application so that it still works:

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

### 1.4: course information step4

> And then place the objects into an array. Modify the variable definitions of App into the following form and modify the other parts of the application accordingly:

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

> NB at this point you can assume that there are always three items, so there is no need to go through the arrays using loops. We will come back to the topic of rendering components based on items in arrays with a more thorough exploration in the next part of the course.

Hinweis: Ab jetzt könnt ihr immer davon ausgehen, dass es sich um 3 Teile handelt, also müsst ihr keine Schleifen für das Array schreiben. Wir kommen auf das Thema - Komponenten im Bezug auf die Werte in einem Array zu erstellen - im nächsten Abschnitt zurück.

> However, do not pass different objects as separate props from the App component to the components Content and Total. Instead, pass them directly as an array:

Übergebt allerdings nicht die verschiedenen Objekte als separate props vom App-Komponenten an die Komponenten Content und Total. Übergebt sie stattdessen diekt als Array:

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

### 1.5: course information step5

> Let's take the changes one step further. Change the course and its parts into a single JavaScript object. Fix everything that breaks.

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

## Object-methods-and-"this"

> Due to the fact that during this course we are using a version of React containing React Hooks we have no need for defining objects with methods. The contents of this chapter are not relevant to the course but are certainly in many ways good to know. In particular when using older versions of React one must understand the topics of this chapter.

Da wir in diesem Kurs eine Reactversion verwenden, die React Hooks kennt, müssen wir nicht wissen, wie man Objekte mit Methoden erstellen. Das Folgende ist nicht relevant für den weiteren Kurs, aber trotzdem gut zu wissen. Besonders, wenn man ältere Reactversionen pflegt.

> Arrow functions and functions defined using the function keyword vary substantially when it comes to how they behave with respect to the keyword this, which refers to the object itself.

Pfeilfunktionen und Funktionen, die mit dem Schlagwort function erstellt wurden, unterscheiden sich erheblich, wenn es darum geht, wie mit "this" umgegangen wird.

> We can assign methods to an object by defining properties that are functions:

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

> Methods can be assigned to objects even after the creation of the object:

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

> Let's slightly modify the object:

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

> Now the object has the method doAddition which calculates the sum of numbers given to it as parameters. The method is called in the usual way, using the object arto.doAddition(1, 4) or by storing a method reference in a variable and calling the method through the variable: referenceToAddition(10, 15).

Jetzt hat das Objekt die Methode "doAddition", die die Summe zweier Zahlen errechnet, die als Parameter übergeben werden. Die Methode kann entweder über das Objekt aufgerufen werden

```javascript
arto.doAddition(1, 4)
```

oder indem der Verweis auf die Methode in einer Variable gespeichert wird und die Variable aufgerufen wird

```javascript
referenceToAddition(10, 15)
```

> If we try to do the same with the method greet we run into an issue:

Wenn wir das gleiche mit der greet-Methode machen, laufen wir in ein Problem:

```javascript
arto.greet()       // "hello, my name is Arto Hellas" gets printed

const referenceToGreet = arto.greet
referenceToGreet() // prints "hello, my name is undefined"
```

> When calling the method through a reference, the method loses knowledge of what the original this was. Contrary to other languages, in JavaScript the value of this is defined based on how the method is called. When calling the method through a reference the value of this becomes the so-called global object and the end result is often not what the software developer had originally intended.

Wenn wir die Methode über einen Verweis aufrufen, verliert die Methode das Wissen, auf wen "this" verweist. Im Gegensatz zu anderen Sprachen beruht in Javascript die Definition von "this" darauf, wie die Methode aufgerufen wurde. Geschieht der Aufruf über einen Verweis, zeigt der Wert von "this" auf das sogenannte globale Objekt und das Endergebnis ist oft nicht das, was der Softwareentwickler im Sinn hatte.

> Losing track of this when writing JavaScript code brings forth a few potential issues. Situations often arise where React or Node (or more specifically the JavaScript engine of the web browser) needs to call some method in an object that the developer has defined. However, in this course we avoid these issues by using the "this-less" JavaScript.

Wenn man den Überblick darüber verliert, auf wen "this" verweist, bekomt man Probleme. Es gibt öfter Situationen, in denen React oder Node (genauer gesagt, die Javascript-Engine des Browsers) eine Methode eines Objektes aufruft, die der Entwickler definiert hat. In diesem Kurs umgehen wir diese Probleme, in dem wir wenig "this" verwenden.

> One situation leading to the "disappearance" of this arises when we set a timeout to call the greet function on the arto object, using the setTimeout function.

Eine Möglichkeit, um "this" verschwinden zu lassen, ist einen Timeout zu setzen, der die greet-Funktion aufruft. Wir verwenden dafür die Funktion setTimeout:

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

> As mentioned, the value of this in JavaScript is defined based on how the method is being called. When setTimeout is calling the method, it is the JavaScript engine that actually calls the method and, at that point, this refers to the global object.

Wie bereits erwähnt wird der Wert von "this" in Javascript darüber bestimmt, wie die Methode aufgrufen wird, zu diesem Punkt verweist "this" auf das globale Objekt.

> There are several mechanisms by which the original this can be preserved. One of these is using a method called bind:

Es gibt verschiedene Techniken, wie man das originiale "this" erhalten kann. Eine davon ist die Methode bind zu nutzen: 

```javascript
setTimeout(arto.greet.bind(arto), 1000)
```

> Calling arto.greet.bind(arto) creates a new function where this is bound to point to Arto, independent of where and how the method is being called.

Der Funktionsaufruf arto.greet.bind(arto) erstellt eine neue Funktion, bei der "this" an Arto gebunden ist, unabhängig davon, wie die Methode aufgerufen wird.

> Using arrow functions it is possible to solve some of the problems related to this. They should not, however, be used as methods for objects because then this does not work at all. We will come back later to the behavior of this in relation to arrow functions.

Indem man Pfeilfunktionen nutzt, ist es möglich einige Probleme zu umgehen, die mit "this" verbunden sind. Allerdings sollten Sie nicht für Objektmethoden verwendet werden, weil dann "this" überhaupt nicht funktioniert. Wir kommen später nochmal auf die Beziehung von Pfeilfunktionenn und "this" zurück.

> If you want to gain a better understanding of how this works in JavaScript, the Internet is full of material about the topic, e.g. the screencast series Understand JavaScript's this Keyword in Depth by egghead.io is highly recommended!

Wenn ihr ein besseres Verständnis für "this" in Javascript bekommen wollt, dann empfehlen wir unbedingt die Screencastserie "Understand JavaScript's this Keyword in Depth" von [egghead.io](egghead.io).

## Classes

> As mentioned previously, there is no class mechanism in JavaScript like the ones in object-oriented programming languages. There are, however, features to make "simulating" object-oriented classes possible.

Wie bereits erwähnt gibt es in Javascript keine Klassen, die mit denen von objektorientierten Sprachen vergleichbar sind. Allerdings gibt es Features mit denen sich objektorientierte Klassen simulieren lassen.

> Let's take a quick look at the class syntax that was introduced into JavaScript with ES6, which substantially simplifies the definition of classes (or class-like things) in JavaScript.

Schauen wir uns kurz die Klassensyntax an, die mit ES6 eingeführt wurde. Diese Syntax vereinfachte die Definition von Klassen in Javascript ungemein.

> In the following example we define a "class" called Person and two Person objects:

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

> When it comes to syntax, the classes and the objects created from them are very reminiscent of Java classes and objects. Their behavior is also quite similar to Java objects. At the core they are still objects based on JavaScript's prototypal inheritance. The type of both objects is actually Object, since JavaScript essentially only defines the types Boolean, Null, Undefined, Number, String, Symbol, BigInt, and Object.

Diese Syntax erinnert stark daran, wie Klassen und Objekte in Java erstellt weren. Ihr Verhalten ist dem von Java-Objekten auch sehr ähnlich. Im Kern sind sie aber immer noch Objekte, die auf Javascripts Prototypenvererbung basieren. Der Datentyp von beiden Objekten ist tatsächlich Object, weil Javascript im Wesentlichen nur 7 Datentypen kennt: Boolean, Null, Undefined, Number, String, Symbol, BigInt und Object.

> The introduction of the class syntax was a controversial addition. Check out Not Awesome: ES6 Classes or Is “Class” In ES6 The New “Bad” Part? on Medium for more details.

Die Einführung von Klassen war eine kontroverse Ergänzung. Schaut euch [Not Awesome: ES6 Classes or Is “Class” In ES6 The New “Bad” Part?](link-missing) auf Medium für mehr Details an.

> The ES6 class syntax is used a lot in "old" React and also in Node.js, hence an understanding of it is beneficial even in this course. However, since we are using the new Hooks feature of React throughout this course, we have no concrete use for JavaScript's class syntax.

Diese Klassensyntax aus ES6 wird viel in älteren React- und Node.js-Projekten verwendet, daher ist ihr Verstehen vorteilhaft. Allerdings nutzen wir in diesem Kurs die neuen Hooks von React, daher gibt gibt es für die Klassen keine direkte Anwendung.

## Javascript Materials

> There exist both good and poor guides for JavaScript on the Internet. Most of the links on this page relating to JavaScript features reference Mozilla's JavaScript Guide.

Es gibt gute und schlechte Anleitungen für Javascript im Internet. Die meisten Links auf dieser Seite zu Javascript verweisen auf [Mozilla's JavaScript Guide](link-missing)

> It is highly recommended to immediately read A re-introduction to JavaScript (JS tutorial) on Mozilla's website.

Wir empfehlen dringend sofort [A re-introduction to JavaScript (JS tutorial)](link-missing) auf der Webseite von [Mozilla](link-missing)

> If you wish to get to know JavaScript deeply there is a great free book series on the Internet called You-Dont-Know-JS.

Wenn ihr euch richtig gut mit Javascript auskennen wollt, es gibt da eine großartige, kostenfreie Buchserie [You-Dont-Know-JS](link-missing)

> Another great resource for learning JavaScript is javascript.info.

Eine andere großartige Quelle, um Javascript zu lernen, ist [javascript.info](link-missing)

> The free and highly engaging book Eloquent JavaScript https://eloquentjavascript.net. Takes you from the basics to interesting stuff quickly, a mixture of theory projects and exercises, covers general programming theory as well as the JavaScript language.

Das kostenfreie und sehr fesselnde Buch [Eloquent Javascript](https://eloquentjavascript.net). Bringt euch sehr schnell von den Grundlagen zum interessanten Zeug, eine Mischung aus theoretischen Projekten und Aufgaben, es geht sowohl um generelle Programmierkenntnisse, sowie um die Sprache an sich.

> egghead.io has plenty of quality screencasts on JavaScript, React, and other interesting topics. Unfortunately, some of the material is behind a paywall.

[egghead.io](egghead.io) hat viele qualitativ vervollte Screencasts zu Javascript, React und anderen interessanten Themen. Leider ist ein Teil des Materials hinter einer Zahlwand.

[back to Part 1a](part_1a.md)

[next Part 1c](part_1c.md)
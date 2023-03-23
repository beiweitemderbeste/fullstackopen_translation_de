# part 1a

> table of contents

## Inhaltsverzeichnis

- [Component](#Component)
- [JSX](#JSX)
- [Multiple Components](#Multiple-Components)
- [props: passing data to components](#props-passing-data-to-components)
- [Some notes](#Some-notes)
- [Do not render objects](#do-not-render-objects)
- [Exercises](#Exercises)

> We will now start getting familiar with probably the most important topic of this course, namely the React-library. Let's start off by making a simple React application as well as getting to know the core concepts of React.

Wir befassen uns jetzt mit dem wahrscheinlich wichtigstem Thema dieses Kurses, der React-Bibliothek. Wir fangen an, indem wir eine einfache React-Anwendung erstellen und uns mit den Grundkonzepten von React auseinandersetzen.

> The easiest way to get started by far is by using a tool called create-react-app. It is possible (but not necessary) to install create-react-app on your machine if the npm tool that was installed along with Node has a version number of at least 5.3.

Der bei weitem einfachste Weg zu starten ist "create-react-app" zu benutzen. Es ist möglich (aber nicht notwendig) create-react-app auf eurem Rechner zu installieren, wenn npm mit Nodejs (mind. Version 5.3) installiert wurde.

> Let's create an application called part1 and navigate to its directory.

Wir beginnen, indem wir die Anwendung "part1" erstellen und in das Verzeichnis wechseln:

```
npx create-react-app part1
cd part1
```

>The application is run as follows

Die Anwendung wird folgendermaßen gestartet:

```
npm start
```

> By default, the application runs on localhost port 3000 with the address http://localhost:3000

Standardmäßig läuft die Anwendung unter http://localhost:3000

> Your default browser should launch automatically. Open the browser console immediately. Also open a text editor so that you can view the code as well as the webpage at the same time on the screen:

Euer Standardbrowser sollte sie automatisch öffnen. Öffnet direkt die Browserkonsole.

!["fullstack content"](./images/part1a_image1.png?raw=true)

> The code of the application resides in the src folder. Let's simplify the default code such that the contents of the file index.js look like:

Der Quellcode der Anwendung liegt im src-Verzeichnis. Wir vereinfachen den Inhalt der Datei index.js, so dass er so aussieht:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

> and file App.js looks like this

und die Datei App.js sieht so aus:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)

export default App
```

> The files App.css, App.test.js, index.css, logo.svg, setupTests.js and reportWebVitals.js may be deleted as they are not needed in our application right now.

Die Dateien App.css, App.test.js, index.css, logo.svg, setupTests.js und reportWebVitals.js können gelöscht werden, weil wir sie momentan nicht benötigt werden. 

> If you end up with the following error:

Wenn ihr folgenden Fehler angezeigt bekommt:

!["fullstack content"](./images/part1a_image2.png?raw=true)

> Then, for some reason you are using a React version older than the current version 18.

liegt das daran, dass ihr aus irgeneinem Grund eine ältere React-Version als die aktuelle (18) benutzt.

> The fix is to change index.js as follows:

Die Lösung dafür ist index.js so abzuändern:

```javascript
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))
```

> You quite likely need to do the same for your other projects.

Ihr müsst das dann wahrscheinlich auch in euren anderen Projekten machen.

> See this for more about the version differences.

Schaut [hier](https://fullstackopen.com/en/part1/a_more_complex_state_debugging_react_apps/#a-note-on-react-version) nach, wenn ihr mehr über die Versionsunterschieden lesen wollt.

## Component

> The file App.js now defines a React component with the name App. The command on the final line of file index.js

Die Datei App.js definiert einen React-Komponenten "App". Der Befehl in der letzten Zeile von index.js

```javascript
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

> renders its contents into the div-element, defined in the file public/index.html, having the id value 'root'.

stellt die Inhalte von diesem App-Komponenten einem div-Element mit der id "root" dar. Das div-Element wird in der Datei public/index.html eingefügt.

> By default, the file public/index.html doesn't contain any HTML markup that is visible to us in the browser. You can try adding some HTML to the file. However, when using React, all content that needs to be rendered is usually defined as React components.

Standardäßig enthält public/index.html kein HTML, das für uns im Browser sichtbar ist. Ihr könnt versuchen HTML in diese Datei zu schreiben, aber normalerweise wird in React jeder Inhalt über Komponenten dargestellt.

> Let's take a closer look at the code defining the component:

Schauen wir uns den Code, der den Komponenten definiert, ein bisschen genauer an:

```javascript
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)
```

> As you probably guessed, the component will be rendered as a div-tag, which wraps a p-tag containing the text Hello world.

Wir ihr wahrscheinlich schon geraten habt, wird der Komponent als div-Tag angezeigt, der einen p-Tag mit dem Text "Hello world" umschließt.

> Technically the component is defined as a JavaScript function. The following is a function (which does not receive any parameters):

Technisch gesehen wird der Komponent als Javascriptfunktion definiert. Das Folgende ist eine Funktion (an die keine Parameter übergeben werden):

```javascript
() => (
  <div>
    <p>Hello world</p>
  </div>
)
```

> The function is then assigned to a constant variable App:

Die Funktion wird dann der Konstanten App zugewiesen:

```javascript
const App = ...
```

> There are a few ways to define functions in JavaScript. Here we will use arrow functions, which are described in a newer version of JavaScript known as ECMAScript 6, also called ES6.

Es gibt verschiedene Wege, um Funktionen in Javascript zu definieren. Wir verwenden hier Pfeilfunktionen, die in einer neueren Version von Javascript (bekannt als ECMAScript 6) beschrieben werden.

> Because the function consists of only a single expression we have used a shorthand, which represents this piece of code:

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

> In other words, the function returns the value of the expression.

In anderen Worten, die Funktion gibt den Wert des Ausdrucks zurück.

> The function defining the component may contain any kind of JavaScript code. Modify your component to be as follows and observe what happens in the console:

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

> It is also possible to render dynamic content inside of a component.

Es ist auch möglich dynamischen Inhalt in einem Komponenten anzuzeigen.

> Modify the component as follows:

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

> Any JavaScript code within the curly braces is evaluated and the result of this evaluation is embedded into the defined place in the HTML produced by the component.

Jeglicher Javascriptcode innerhalb der geschwungenen Klammern wird ausgewertet und das Ergebnis davon wird in seinem jeweiligem Platz im HTML-Code eingefügt.

## JSX

> It seems like React components are returning HTML markup. However, this is not the case. The layout of React components is mostly written using JSX. Although JSX looks like HTML, we are actually dealing with a way to write JavaScript. Under the hood, JSX returned by React components is compiled into JavaScript.

Es sieht so aus als würden React-Komponenten HTML ausgeben, was aber nicht der Fall ist. React-Kompenenten werden meistens in JSX geschrieben. Auch wenn JSX wie HTML aussieht, handelt es sich tatsächlich um eine andere Art Javascript zu schreiben:

> After compiling, our application looks like this:

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

> The compilation is handled by Babel. Projects created with create-react-app are configured to compile automatically. We will learn more about this topic in part 7 of this course.

Die Kompilierung wird von "babel" übernommen. Projekte, die mit create-react-app erstellt wurden, sind so kofiguriert, das die Kompilierung automatisch geschieht. Wir werden uns in Abschnitt 7 dieses Kurs näher mit diesem Thema beschäftigen.

> It is also possible to write React as "pure JavaScript" without using JSX. Although, nobody with a sound mind would actually do so.

Es ist möglich, React in "purem" Javascript zu schreiben ohne JSX zu verwenden. Allerdings würde das niemand, der bei klarem Verstand ist, tun.

> In practice, JSX is much like HTML with the distinction that with JSX you can easily embed dynamic content by writing appropriate JavaScript within curly braces. The idea of JSX is quite similar to many templating languages, such as Thymeleaf used along with Java Spring, which are used on servers.

In der Praxis sieht JSX ähnlich wie HTML aus, mit dem Unterschied, dass man dynamischen Inhalt leicht einfügen kann, indem man Javascript in geschwungenen Klammern einfügt. Die Grundgedanke von JSX ist sehr ähnlich zu anderen Template-Sprachen, wie z.B. Thymeleaf für Java Spring.

> JSX is "XML-like", which means that every tag needs to be closed. For example, a newline is an empty element, which in HTML can be written as follows:

JSX ist ähnlich wie XML, was bedeudet, dass jeder Tag geschlossen werden muss. Zum Beispiel ist ein Zeilenumbruch ein leeres Element, dass in HTML so geschrieben werden kann:

```
<br>
```

> but when writing JSX, the tag needs to be closed:

aber wenn man JSX schreibt, muss man den Tag schließen:

```
<br />
```

## Multiple Components

> Let's modify the file App.js as follows (NB: export at the bottom is left out in these examples, now and in the future. It is still needed for the code to work):

Verändern wir die Datei App.js wie folgt:

Hinweis: der Export am unteren Rand wird in diesen und anderen Beispielen weggelassen. Ihr müsst ihn trotzdem einfügen, damit die Anwendung funktioniert.

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

> We have defined a new component Hello and used it inside the component App. Naturally, a component can be used multiple times:

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

> Writing components with React is easy, and by combining components, even a more complex application can be kept fairly maintainable. Indeed, a core philosophy of React is composing applications from many specialised reusable components.

Mit React ist sehr einfach Komponenten zu erstellen und wenn man mehrere Komponenten miteinander kombiniert, lassen sich auch komplexere Anwendungen leicht verwalten. Tatsächlich ist eine Kernphilosophie von React das Kombinieren vieler spezialisierter und wiederverwendbarer Komponenten.

> Another strong convention is the idea of a root component called App at the top of the component tree of the application. Nevertheless, as we will learn in part 6, there are situations where the component App is not exactly the root, but is wrapped within an appropriate utility component.

Eine andere Konvention ist die Idee einen Hauptkomponenten "App" an die Spitze der Appstruktur zu setzen. Trotzdem werden wir in Abschnitt 6 noch lernen, dass es Situationen gibt, wo der Komponent "App" nicht nicht ganz oben steht.

## props: passing data to components

> It is possible to pass data to components using so-called props.

Es ist möglich an die Komponenten Daten zu übergeben, diese werden "props" genannt.

> Let's modify the component Hello as follows:

Ändern wir den Komponenten Hello ab:

```javascript
const Hello = (props) => {
  return (
    <div>
      <p>Hello {props.name}</p>
    </div>
  )
}
```

> Now the function defining the component has a parameter props. As an argument, the parameter receives an object, which has fields corresponding to all the "props" the user of the component defines.

Jetzt übergibt die Funktion, die den Komponenten definiert, einen Parameter props. Als Argument erhält der Parameter ein Objekt, das zugehörige Felder für alle "props", die der Komponent definiert, hat.

> The props are defined as follows:

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

> There can be an arbitrary number of props and their values can be "hard-coded" strings or the results of JavaScript expressions. If the value of the prop is achieved using JavaScript it must be wrapped with curly braces.

Es kann unzählige props geben und ihre Werte können hartkodierte Strings sein oder das Ergebnis von Javascript-Ausdrücken. Wenn die Werte der props mit Javascript ermittelt werden, müssen sie in geschwungen Klammern einfügt werden.

> Let's modify the code so that the component Hello uses two props:

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

> The props sent by the component App are the values of the variables, the result of the evaluation of the sum expression and a regular string.

Die props, die vom Komponenten App überreicht werden, sind die Werte der Variablen: das Ergebnis der Evaluierung der Summe und eines normalen Strings.

## Some notes

> React has been configured to generate quite clear error messages. Despite this, you should, at least in the beginning, advance in very small steps and make sure that every change works as desired.

React wurde so konfiguriert, dass es sehr klare Fehlermeldungen ausgibt. Abgesehen davon solltet ihr gerade am Anfang in sehr kleinen Schritten vorgehen und sicherstellen, das jede Änderung funktioniert.

> The console should always be open. If the browser reports errors, it is not advisable to continue writing more code, hoping for miracles. You should instead try to understand the cause of the error and, for example, go back to the previous working state:

Die Konsole sollte immer geöffnet sein. Wenn der Browser Fehlermeldungen anzeigt, ist es nicht ratsam weiter zu programmieren und auf Wunder zu hoffen. Ihr solltet stattdessen versuchen zu verstehen, was den Fehler verursacht hat und zurück zum letzten Stand gehen, der funktioniert hat:

!["fullstack content"](./images/part1a_image3.png?raw=true)

> It is good to remember that in React it is possible and worthwhile to write console.log() commands (which print to the console) within your code.

Es ist immer gut, sich daran zu erinnern, dass es in React möglich ist console.log()-Befehle in den Code zu setzen.

> Also keep in mind that React component names must be capitalized. If you try defining a component as follows:

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
> and use it like this

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

> the page is not going to display the content defined within the Footer component, and instead React only creates an empty footer element, i.e. the built-in HTML element instead of the custom React element of the same name. If you change the first letter of the component name to a capital letter, then React creates a div-element defined in the Footer component, which is rendered on the page.

Die Seite wird den Inhalt des Footer-Komponenten nicht anzeigen und stattdessen erstellt React nur ein leeres Footer-Element, mit anderen Worten das eingebaute HTML-Element statt des React-Elements mit demselben Namen. Wenn ihr den Namen des Komponent am Anfang großschreibt, erstellt React ein div-Element für den Footer und zeigt darin seinen Inhalt an.

> Note that the content of a React component (usually) needs to contain one root element. If we, for example, try to define the component App without the outermost div-element:

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

> the result is an error message.

ist das Ergebnis eine Fehlermeldung.

!["fullstack content"](./images/part1a_image4.png?raw=true)

> Using a root element is not the only working option. An array of components is also a valid solution:

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

> However, when defining the root component of the application this is not a particularly wise thing to do, and it makes the code look a bit ugly.

Jedoch ist dies beim Definieren des Hauptkomponenten nicht besonders ratsam und es macht den Code hässlich.

> Because the root element is stipulated, we have "extra" div-elements in the DOM-tree. This can be avoided by using fragments, i.e. by wrapping the elements to be returned by the component with an empty element:

Weil ein Hauptelement vorgeschrieben ist, haben wir ein Extra-div-Element in der DOM. Das kann man vermeiden, indem man Fragemente verwendet, mit anderen Worten, man umschließt den Inhalt des Komponenten mit einem leeren Element:

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

> It now compiles successfully, and the DOM generated by React no longer contains the extra div-element.

Jetzt ist die Kompilierung erfolgreich und die von React generierte DOM enthält nicht länger ein unnötiges div-Element.

## Do not render objects

> Consider an application that prints the names and ages of our friends on the screen:

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

> However, nothing appears on the screen. I've been trying to find a problem in the code for 15 minutes, but I can't figure out where the problem could be.

Allerdings erscheint nichts am Monitor. Ich habe 15 Minuten lang versucht das Problem zu finden, aber ich habe es trotzdem nicht gefunden.


> I finally remember the promise we made

    I promise to keep the console open all the time during this course, and for the rest of my life when I'm doing web development

Schließlich habe ich mich an das Versprechen erinnert, das wir gemacht haben:

  | Ich verspreche die Browserkonsole die ganze Zeit geöffnet zu haben, während des Kurses und für mein restliches Leben, wenn ich Webentwicklung betreibe.

> The console screams in red:

Die Konsole schreit in rot:

!["fullstack content"](./images/part1a_image5.png?raw=true)

> The core of the problem is Objects are not valid as a React child, i.e. the application tries to render objects and it fails again.

Die Ursache des Problems ist, dass Objekte keine validen React-Kinder sind, was bedeutet, dass die Anwendung versucht Objekte anzuzeigen und immer wieder scheitert.

> The code tries to render the information of one friend as follows

Der Code versucht die Informationen eines Freundes wie folgt anzuzeigen:

```javascript
<p>{friends[0]}</p>
```

> and this causes a problem because the item to be rendered in the braces is an object.

und das verursacht das Problem, weil das Element in den geschweiften Klammern, das angezeigt werden soll, ein Objekt ist.

```javascript
{ name: 'Peter', age: 4 }
```

> In React, the individual things rendered in braces must be primitive values, such as numbers or strings.

In React müssen die individuellen Elemente in geschweiften Klammern, die angezeigt werden sollen, primitive Werte sein, also z.B. Strings oder Nummern.

> The fix is ​​as follows

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

> So now the friend's name is rendered separately inside the curly braces

Jetzt wird der Name des Freundes gesondert innerhalb der geschweiften Klammern angezeigt

```javascript
{friends[0].name}
```

> and age

und auch sein Alter

```javascript
{friends[0].age}
```

> After correcting the error, you should clear the console error messages by pressing 🚫 and then reload the page content and make sure that no error messages are displayed.

Nach der Korrektur des Fehlers solltet ihr die Fehlermeldungen in eurer Browserkonsole löschen, in dem ihr 🚫 betätigt, danach die Seite aktualisieren und sicherstellen, dass keine weiteren Fehlermeldungen angezeigt werden.

> A small additional note to the previous one. React also allows arrays to be rendered if the array contains values ​​that are eligible for rendering (such as numbers or strings). So the following program would work, although the result might not be what we want:

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

> In this part, it is not even worth trying to use the direct rendering of the tables, we will come back to it in the next part.

In diesem Abschnitt ist der Versuch vom direkten Anzeigen von Tabellen wertlos, wir werden später darauf zürückkommen. 

## Exercises

> Exercises are submitted through GitHub and by marking completed exercises in the submission application.

Die Aufgaben werden über GitHub eingereicht und müssen im Submissionsystem als erledigt markiert werden.

> You may submit all the exercises of this course into the same repository, or use multiple repositories. If you submit exercises of different parts into the same repository, please use a sensible naming scheme for the directories.

Ihr könnt alle Aufgaben über ein Repository einreichen oder verschiedene Repositories benutzen. Bitte benennt eure Verzeichnisse korrekt, wenn ihr Aufgaben verschiedener Abschnitte im selben Repository einreicht. 

> One very functional file structure for the submission repository is as follows:

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

> See this example submission repository!

> For each part of the course there is a directory, which further branches into directories containing a series of exercises, like "unicafe" for part 1.

Für jeden Abschnitt des Kurses gibt es ein Verzeichnis, das sich weiter in verschiedene Verzeichnisse aufteilt, die die Aufgaben enthalten, wie z.B. "unicafe" in Abschnitt 1.

> For each web application for a series of exercises, it is recommended to submit all files relating to that application, except for the directory node_modules.

Für jede Webapplikation gibt es mehrere Aufgaben. Es wird empfohlen immer alle Dateien, die zu einer Anwendung gehören, einzureichen, abgesehen vom Verzeichnis node_modules.

> The exercises are submitted one part at a time. When you have submitted the exercises for a part of the course you can no longer submit undone exercises for the same part.

Die Aufgaben werden Abschnitt für Abschnitt eingereicht. Wenn ihr Aufgaben für einen Abschnitt eingereicht habt, könnt ihr nicht länger noch fehlende Aufgaben für diesen Abschnitt abgeben.

> Note that in this part, there are more exercises besides those found below. Do not submit your work until you have completed all of the exercises you want to submit for the part.

Beachtet, dass es in diesem Abschnitt noch mehr Aufgaben gibt als die folgenden:

### 1.1: course information, step1

> The application that we will start working on in this exercise will be further developed in a few of the following exercises. In this and other upcoming exercise sets in this course, it is enough to only submit the final state of the application. If desired, you may also create a commit for each exercise of the series, but this is entirely optional.

Die Anwendung, mit der wir in dieser Übung anfangen, werden wir in den nächsten Übungen fortführen. Für dieses und andere Kapitel reicht es aus, nur den Endstand der Anwendung einzureichen. Wenn ihr wollt, könnte ihr für jede einzelne Aufgabe einen Commit erstellen, aber das ist komplett optional.

> Use create-react-app to initialize a new application. Modify index.js to match the following

Benutzt create-react-app, um die Anwendung zu initialisieren. Ändert index.js wie folgt ab:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

> and App.js to match the following

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

> and remove extra files (App.css, App.test.js, index.css, logo.svg, setupTests.js, reportWebVitals.js)).

und löscht alle unnötigen Dateien (App.css, App.test.js, index.css, logo.svg, setupTests.js, reportWebVitals.js).

> Unfortunately, the entire application is in the same component. Refactor the code so that it consists of three new components: Header, Content, and Total. All data still resides in the App component, which passes the necessary data to each component using props. Header takes care of rendering the name of the course, Content renders the parts and their number of exercises and Total renders the total number of exercises.

Leider ist die ganze Anwendung im selben Komponenten. Verändert den Code so, dass sie aus drei neuen Komponenten besteht: Header, Content und Total. Alle Daten verbleiben im App-Komponenten, der die benötigten Daten über props an jeden Komponenten weiterreicht.

> Define the new components in file App.js.

Definiert die Komponenten in App.js.

> The App component's body will approximately be as follows:

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

> WARNING create-react-app automatically makes the project a git repository unless the application is created within an already existing repository. Most likely you do not want the project to become a repository, so run the command rm -rf .git in the root of the project.

ACHTUNG: create-react-app wandelt das Projekt in ein git-Repository um, sofern es nicht innerhalb eines bereits bestehendenen Repositorys erstellt wurde. Ihr wollt eurer Projekt sicherlich nicht in ein Repository umwandeln, also gebt den Befehl "rm -rf .git" im Hauptverzeichnis eures Projektes ein.

### 1.2: course information, step2

> Refactor the Content component so that it does not render any names of parts or their number of exercises by itself. Instead it only renders three Part components of which each renders the name and number of exercises of one part.

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

> Our application passes on information in quite a primitive way at the moment, since it is based on individual variables. This situation will improve soon.

Unsere Anwendung gibt die verschiedenen Informationen auf recht primitive Weise weiter, da die Informationen auf individuellen Variablen basieren. Diese Situation werden wir in Kürze verbessern.

[back to Part 1](part_1.md)

[next Part 1b](part_1b.md)

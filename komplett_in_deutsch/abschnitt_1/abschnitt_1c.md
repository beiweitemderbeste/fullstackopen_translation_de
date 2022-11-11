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

In unserem vorangegangenen Code haben wir auf die Daten, die an unseren Komponenten übergeben wurden, mit props.name und props.age verwiesen. Von diesen beiden Ausdrücken mussten wir props.age zweimal verwenden.

> Since props is an object

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
const Hello = ({ name, age }) => {  const bornYear = () => new Date().getFullYear() - age

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

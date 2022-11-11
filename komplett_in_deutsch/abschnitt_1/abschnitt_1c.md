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


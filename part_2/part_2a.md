# part 2a

> table of contents

## Inhaltsverzeichnis

- [console.log](#console-log)
- [Protip: Visual Studio Code snippets](#Protip-Visual-Studio-Code-snippets)
- [Javascript Arrays](#Javascript-Arrays)
- [event handlers revisited](#event-handlers-revisited)
- [rendering collections](#rendering-collections)
- [key-attribute](#key-attribute)
- [map](#map)
- [anti-pattern: array indexes as keys](#anti-pattern:-array-indexes-as-keys)
- [refactoring modules](#refactoring-modules)
- [when the application breaks](#when-the-application-breaks)
- [exercises](#exercises)

> Before starting a new part, let's recap some of the topics that proved difficult last year.

## console.log

> What's the difference between an experienced JavaScript programmer and a rookie? The experienced one uses console.log 10-100 times more.

> Paradoxically, this seems to be true even though a rookie programmer would need console.log (or any debugging method) more than an experienced one.

> When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging.

> NB As explained in part 1, when you use the command console.log for debugging, don't concatenate things 'the Java way' with a plus. Instead of writing:

```javascript
console.log('props value is ' + props)
```

separate the things to be printed with a comma:

```javascript
console.log('props value is', props)
```

If you concatenate an object with a string and log it to the console (like in our first example), the result will be pretty useless: 

```javascript
props value is [object Object]
```

On the contrary, when you pass objects as distinct arguments separated by commas to console.log, like in our second example above, the content of the object is printed to the developer console as strings that are insightful. If necessary, read more about debugging React-applications.

## Protip: Visual Studio Code snippets

> With Visual Studio Code it's easy to create 'snippets', i.e., shortcuts for quickly generating commonly re-used portions of code, much like how 'sout' works in Netbeans.

> Instructions for creating snippets can be found here.

> Useful, ready-made snippets can also be found as VS Code plugins, in the marketplace.

> The most important snippet is the one for the console.log() command, for example, clog. This can be created like so: 

```javascript
{
  "console.log": {
    "prefix": "clog",
    "body": [
      "console.log('$1')",
    ],
    "description": "Log output to console"
  }
}
```

> Debugging your code using console.log() is so common that Visual Studio Code has that snippet built in. To use it, type log and hit tab to autocomplete. More fully featured console.log() snippet extensions can be found in the marketplace.
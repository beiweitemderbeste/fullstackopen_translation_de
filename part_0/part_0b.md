# part 0b

> table of contents

# Inhaltsverzeichnis

- [Grundlagen von Webapplikationen](#grundlagen-von-webapplikationen)
- [HTTP GET](#HTTP-GET)
- [Traditionelle Webanwendungen](#Traditionelle-Webanwendungen)
- [Anwendungslogik im Browser laufen lassen](#Anwendungslogik-im-Browser-laufen-lassen)
- [Event Handlers und Callback-Funktionen](#Event-Handlers-und-Callback-Funktionen)
- [Document Object Model bzw DOM](#Document-Object-Model-bzw-DOM)
- [Bearbeiten des document objects von der Konsole aus](#Bearbeiten-des-document-objects-von-der-Konsole-aus)
- [CSS](#CSS)
- [Wie eine Seite geladen wird, die JavaScript enthält](#Wie-eine-Seite-geladen-wird-die-JavaScript-enthält)
- [Formulare und HTTP POST](#Formulare-und-HTTP-POST)
- [AJAX](#AJAX)
- [Single page app](#Single-page-app)
- [Javascript-libraries](#Javascript-libraries)
- [Full stack web development](#Full-stack-web-development)
- [JavaScript fatigue](#JavaScript-fatigue)
- [Exercises](#Exercises)

> Fundamentals of Web apps

## Grundlagen von Webapplikationen

> Before we start programming, we will go through some principles of web development by examining an example application at https://studies.cs.helsinki.fi/exampleapp.

Bevor wir mit programmieren anfangen, befassen wir uns mit den Grundsätzen der Webentwicklung, indem wir eine Beispielanwendung (https://studies.cs.helsinki.fi/exampleapp) analysieren.

> The application exists only to demonstrate some basic concepts of the course, and is, by no means, an example of how a modern web application should be made. On the contrary, it demonstrates some old techniques of web development, which could even be considered bad practices nowadays.

Die Anwendung dient nur zu Demonstrationszwecken einiger Grundkonzepte des Kurses und ist auf keinen Fall ein Beispiel dafür, wie moderne Webanwendungen gebaut werden. Eher im Gegenteil, es werden alte Methoden der Webentwicklung gezeigt, die heutzutage als "bad practices" bezeichnet werden.

> Code will conform to contemporary best practices from part 1 onwards.

Der Quellcode entspricht ab Abschnitt 1 zeitgemäßen "best practices".

> Open the example application in your browser. Sometimes this takes a while.

Öffnet die Beispielanwendung in eurem Browser. Manchmal dauert das etwas.

> The 1st rule of web development: Always keep the Developer Console open on your web browser. On macOS, open the console by pressing F12 or option-cmd-i simultaneously. On Windows or Linux, open the console by pressing F12 or ctrl-shift-i simultaneously. The console can also be opened via the context menu.

Die erste Regel der Webentwicklung: Habt immer die Entwicklerkonsole in euerem Browser offen. Unter Linux könnt ihr die Konsole mit der F12-Taste öffnen. Die Konsole kann auch über das Browsermenü geöffnet werden.

> Remember to always keep the Developer Console open when developing web applications.

Merkt euch immer die Entwicklerkonsole beim Entwickeln von Webanwendungen geöffnet zu haben.

> The console looks like this: 

Die Konsole sieht ungefähr so aus:

!["A screenshot of the developer tools open in a browser"](./images/part0b_image1.png?raw=true)

> NB: The most important tab is the Console tab. However, in this introduction we will be using the Network tab quite a bit.

Hinweis: Der wichtigste Tab ist der "Console"-Tab. Trotzdem werden wir uns in dieser Einleitung auch mit dem "Network"-Tab beschäftigen.

## HTTP GET

> The server and the web browser communicate with each other using the HTTP protocol. The Network tab shows how the browser and the server communicate.

Der Server und der Browser kommunizieren über HTTP. Der "Network"-Tab zeigt, wie der Browser und der Server kommunizieren.

> When you reload the page (press the F5 key or the ↻ symbol on your browser), and the console will show that two events have happened:

Wenn ihr die Seite erneut ladet (durch das Drücken der F5-Taste oder das ↻ Symbol in eurem Browser), wird die Konsole zeigen, das zwei Ereignisse passiert sind:

> - The browser has fetched the contents of the page studies.cs.helsinki.fi/exampleapp from the server

- Der Browser hat die Inhalte der Webseite vom Server studies.cs.helsinki.fi/exampleapp geladen

> - And has downloaded the image kuva.png

- und auch die Bilddatei kuva.png

!["Screenshot of the developer console showing these two events"](./images/part0b_image2.png?raw=true)

> On a small screen you might have to widen the console window to see these.

Auf kleineren Displays müsst ihr möglicherweise das Konsolenfenster vergrößern, um alles zu sehen.

> Clicking the first event reveals more information on what's happening: 

Wenn ihr auf das erste Ereignis klickt werden mehr Informationen darüber angezeigt, was passiert:

!["Detail view of a single event"](./images/part0b_image3.png?raw=true)

> The upper part, General, shows that the browser made a request to the address https://studies.cs.helsinki.fi/exampleapp (though the address has changed slightly since this picture was taken) using the GET method, and that the request was successful, because the server response had the Status code 200.

Der obere Teil unter "General" zeigt, dass der Browser über die GET-Methode eine Anfrage zur Adresse https://studies.cs.helsinki.fi/exampleapp geschickt hat (Bitte beachtet, dass sich die adresse leicht geändert hat seitdem der Screenshot erstellt wurde). Die Anfrage war erfolgreich, weil die Serverantwort den Statuscode 200 hatte.

> The request and the server response have several headers:

Die Anfrage und die Serverantwort haben mehrere Header:

!["fullstack content"](./images/part0b_image4.png?raw=true)

> The Response headers on top tell us e.g. the size of the response in bytes, and the exact time of the response. An important header Content-Type tells us that the response is a text file in utf-8-format, contents of which have been formatted with HTML. This way the browser knows the response to be a regular HTML-page, and to render it to the browser 'like a web page'.

Die oberen Antwortheader zeigen z.B. die Größe der Antwort in Bytes und die genaue Zeit der Antwort an. Der wichtige Header "Content-Type" zeigt an, dass die Antwort eine Textdatei im utf-8-Format ist, deren Inhalt mit HTML formattiert wurde. Auf diese Weise weiß der Browser, dass die Antwort eine reguläre HTML-Seite ist und stellt sie als eine "Webseite" dar.

> The Response tab shows the response data, a regular HTML-page. The body section determines the structure of the page rendered to the screen: 

Der "Response"-Tab zeigt die Antwortdaten, eine reguläre HTML-Seite. Die Body-Sektion definiert die Seitenstruktur, die am Bildschirm angezeigt wird:

!["Screenshot of the response tab"](./images/part0b_image5.png?raw=true)

> The page contains a div element, which in turn contains a heading, a link to the page notes, and an img tag, and displays the number of notes created.

Die Seite enthält ein div-Element, das wiederum eine Überschrift enthält, einen Link zu den Seitennotizen und einen img-Tag, ebenso wie die Anzahl der Notizen, die erstellt wurden.

> Because of the img tag, the browser does a second HTTP-request to fetch the image kuva.png from the server. The details of the request are as follows: 

Wegen des img-Tags stellt der Browser eine zweite HTTP-Anfrage, um die Bilddatei kuva.png vom Server zu laden. Die Details der Anfrage sind folgendermaßen:

!["Detail view of the second event"](./images/part0b_image6.png?raw=true)

> The request was made to the address https://studies.cs.helsinki.fi/exampleapp/kuva.png and its type is HTTP GET. The response headers tell us that the response size is 89350 bytes, and its Content-type is image/png, so it is a png image. The browser uses this information to render the image correctly to the screen. 

Die Anfrage (vom Typ HTTP GET) wurde an die Adresse https://studies.cs.helsinki.fi/exampleapp/kuva.png gestellt. Der Antwortheader zeigt uns, dass die Antwort 89350 Bytes groß war und ihr "Content-type" image/png ist. Also ist es eine png-Bilddatei. Der Browser benutzt diese Information, um die Bilddatei korrekt am Bildschirm darzustellen.

> The chain of events caused by opening the page https://studies.cs.helsinki.fi/exampleapp on a browser form the following sequence diagram:

So sieht die Ereigniskette beim Öffnen der Seite https://studies.cs.helsinki.fi/exampleapp in einem Browser in einem Sequenzdiagramm aus: 

!["Sequence diagram of the flow covered above"](./images/part0b_image7.png?raw=true)

> First, the browser sends an HTTP GET request to the server to fetch the HTML code of the page. The img tag in the HTML prompts the browser to fetch the image kuva.png. The browser renders the HTML page and the image to the screen. 

Zuerst schickt der Browser eine HTTP GET-Anfrage an den Server, um den HTML-Quellcode der Seite zu laden. Der img-Tag in dem HTML-Dokument fordert den Browser auf, die Bilddatei kuva.png zu laden. Der Browser zeigt die HTML-Seite und die Bilddatei auf dem Bildschirm an.

> Even though it is difficult to notice, the HTML page begins to render before the image has been fetched from the server. 

Auch wenn es schwer zu bemerken ist: die Darstellung der HTML-Seite beginnt bevor die Bilddatei vom Server geladen wurde.

> Traditional web applications 

## Traditionelle Webanwendungen

> The homepage of the example application works like a traditional web application. When entering the page, the browser fetches the HTML document detailing the structure and the textual content of the page from the server.

Die Homepage der Beispielanwendungen funktioniert wie eine traditionelle Webanwendung. Wenn die Seite aufgerufen wird, lädt der Browser vom Server das HTML-Dokument, das die Struktur und den textlichen Inhalt der Seite beschreibt.

> The server has formed this document somehow. The document can be a static text file saved into the server's directory. The server can also form the HTML documents dynamically according to the application code, using, for example, data from a database. The HTML code of the example application has been formed dynamically, because it contains information on the number of created notes. 

Der Server hat dieses Dokument irgendwie erstellt. Das Dokument kann eine statische Textdatei sein, die im Serververzeichnis abgespeichert wurde. Der Server kann auch dynamisch HTML-Dokumente erstellen, indem er bspw. Daten aus einer Datenbank lädt. Der HTML-Quellcode der Beispielanwendung wurde dynamisch erstellt, weil er die Anzahl der erstellten Notizen enthält.

> The HTML code of the homepage is as follows: 

Der HTML-Quellcode der Hompage lautet wie folgt:

```javascript
const getFrontPageHtml = (noteCount) => {
  return(`
    <!DOCTYPE html>
    <html>
      <head>
      </head>
      <body>
        <div class='container'>
          <h1>Full stack example app</h1>
          <p>number of notes created ${noteCount}</p>
          <a href='/notes'>notes</a>
          <img src='kuva.png' width='200' />
        </div>
      </body>
    </html>
`)
} 

app.get('/', (req, res) => {
  const page = getFrontPageHtml(notes.length)
  res.send(page)
})
```

> You don't have to understand the code just yet. 

Ihr müsst den Quellcode jetzt noch nicht verstehen können.

> The content of the HTML page has been saved as a template string, or a string which allows for evaluating, for example, variables in the midst of it. The dynamically changing part of the homepage, the number of saved notes (in the code noteCount), is replaced by the current number of notes (in the code notes.length) in the template string.

Der Inhalt der HTML-Seite wurde als "template string" gespeichert bzw. als String, der eine Auswertung erlaubt, z.B. ob er Variablen enthält. Der sich dynamisch ändernde Teil der Homepage (die Anzahl der gespeicherten Notizen (im Quellcode "noteCount")) wird von der aktuellen Anzahl der Notizen (im Quellcode "notes.length") im "template string" ersetzt.

> Writing HTML in the midst of the code is of course not smart, but for old-school PHP-programmers it was a normal practice.

HTML im Quellcode zu schreiben ist natürlich nicht sehr clever, aber für Oldschool-PHP-Programmierer war das völlig normal.

> In traditional web applications the browser is "dumb". It only fetches HTML data from the server, and all application logic is on the server. A server can be created using Java Spring (like in the University of Helsinki course Web-palvelinohjelmointi), Python Flask (like in the course tietokantasovellus) or with Ruby on Rails to name just a few examples.

In traditionellen Webanwendungen ist der Browser "dumm". Er lädt nur die HTML-Daten vom Server und die ganze Anwendungslogik ist auf dem Server. Ein Server kann mit Java Spring (wie im Kurs Web-palvelinohjelmointi der Universität von Helsinki), Python Flask (wie im Kurs tietokantasovellus) oder mit Ruby on Rails erstellt werden, nur um einige Beispiele zu nennen.

> The example uses Express library with the Node.js. This course will use Node.js and Express to create web servers. 

Die Beispielanwendung wurde mit Node.js und der Express-Bibliothek erstellt. Diese Technologien werden im Kurs für Webserver eingesetzt.

> Running application logic in the browser

## Anwendungslogik im Browser laufen lassen

> Keep the Developer Console open. Empty the console by clicking the 🚫 symbol, or by typing clear() in the console. Now when you go to the notes page, the browser does 4 HTTP requests: 

Lasst die Entwicklerkonsole offen. Löscht sie, indem ihr auf das 🚫-Symbol klickt oder clear() in der Konsole eingebt. Wenn ihr jetzt die "notes"-Seite öffnet, stellt der Browser 4 HTTP-Anfragen:

!["Screenshot of the developer console with the 4 requests visible"](./images/part0b_image8.png?raw=true)

> All of the requests have different types. The first request's type is document. It is the HTML code of the page, and it looks as follows: 

Jede Anfrage hat einen verschiedenen Typen. Der Typ der ersten Anfrage ist "document". Das ist der HTML-Quellcode der Seite und sieht folgendermaßen aus:

!["Detail view of the first request"](./images/part0b_image9.png?raw=true)

> When we compare the page shown on the browser and the HTML code returned by the server, we notice that the code does not contain the list of notes. The head-section of the HTML contains a script-tag, which causes the browser to fetch a JavaScript file called main.js.

Wenn wir die Seite, die im Browser angezeigt wird, mit dem HTML-Quellcode, den der Server ausgibt, vergleichen, sehen wir, dass der Quellcode keine Liste von Notizen enthält. Die Head-Sektion des HTML-Dokuments enthält einen script-Tag, der dafür sorgt, dass der Browser die Javascript-Datei main.js herunterlädt.

> The JavaScript code looks as follows:

Der Javascript-Quellcode sieht folgendermaßen aus:

```javascript
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    console.log(data)

    var ul = document.createElement('ul')
    ul.setAttribute('class', 'notes')

    data.forEach(function(note) {
      var li = document.createElement('li')

      ul.appendChild(li)
      li.appendChild(document.createTextNode(note.content))
    })

    document.getElementsByClassName('notes').appendChild(ul)
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

> The details of the code are not important right now, but some code has been included to spice up the images and the text. We will properly start coding in part 1. The sample code in this part is actually not relevant at all to the coding techniques of this course. 

Die Details des Quellcodes sind im Moment nicht wichtig, er wurde eingefügt, um die Bilder und den Text interessanter zu gestalten. Das "richtige" Programmieren geht ab Abschnitt 1 los. Der Beispielquellcode in diesem Abschnitt ist nicht wichtig für den weiteren Verlauf dieses Kurs.

> Immediately after fetching the script tag, the browser begins to execute the code. 

Direkt nach dem Laden des script-Tags beginnt der Browser den Quellcode auszuführen.

> The last two lines instruct the browser to do an HTTP GET request to the server's address /data.json:

Die letzten zwei Zeilen weisen den Browser an, eine HTTP GET-Anfrage an die Serveradresse /data.json zu stellen:

```javascript
xhttp.open('GET', '/data.json', true)
xhttp.send()
```

> This is the bottom-most request shown on the Network tab. 

Das ist die Serveranfrage, die im Network-Tab ganz unten steht.

> We can try going to the address https://studies.cs.helsinki.fi/exampleapp/data.json straight from the browser:

Wir können die Adresse https://studies.cs.helsinki.fi/exampleapp/data.json direkt im Browser aufrufen:

!["fullstack content"](./images/part0b_image10.png?raw=true)

> There we find the notes in JSON "raw data". By default, Chromium-based browsers are not too good at displaying JSON data. Plugins can be used to handle the formatting. Install, for example, JSONVue on Chrome, and reload the page. The data is now nicely formatted: 

Hier finden wir die Notizen also "rohe" JSON-Daten. Chromium-basierte Browser sind nicht gut im Darstellen von JSON-Daten. Daher solltet ihr Firefox installieren, der das von Haus aus kann.

!["Formatted JSON output"](./images/part0b_image11.png?raw=true)

> So, the JavaScript code of the notes page above downloads the JSON-data containing the notes, and forms a bullet-point list from the note contents:

Der Javascriptcode der Notizenseite lädt die JSON-Daten, die die Notizen enthalten und erstellt eine Aufzählungsliste der Notizen:

> This is done by the following code: 

Das wird über folgenden Quellcode erreicht:

```javascript
const data = JSON.parse(this.responseText)
console.log(data)

var ul = document.createElement('ul')
ul.setAttribute('class', 'notes')

data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li)
  li.appendChild(document.createTextNode(note.content))
})

document.getElementById('notes').appendChild(ul)
```

> The code first creates an unordered list with a ul-tag...

Der Quellcode erstellt zuerst eine ungeordnete Liste mit dem ul-Tag...

```javascript
var ul = document.createElement('ul')
ul.setAttribute('class', 'notes')
```

> ...and then adds one li-tag for each note. Only the content field of each note becomes the contents of the li-tag. The timestamps found in the raw data are not used for anything here. 

... und fügt dann einen li-Tag für jede Notiz hinzu. Nur das "content field" der jeweiligen Notiz wird zum Inhalt des li-Tags. Die Zeitstempel, die in den rohen Daten angezeigt werden, werden hier nicht genutzt.

```javascript
data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li)
  li.appendChild(document.createTextNode(note.content))
})
```

> Now open the Console-tab on your Developer Console:

Öffnet jetzt den Console-Tab in euer Entwicklerkonsole:

!["Screenshot of the console tab on the developer console"](./images/part0b_image12.png?raw=true)

> By clicking the little triangle at the beginning of the line, you can expand the text on the console.

Klickt man auf das kleine Dreieck am Beginn der Zeile, kann man den Text in der Konsole ausklappen.

!["Screenshot of one of the previously collapsed entries expanded"](./images/part0b_image13.png?raw=true)

> This output on the console is caused by the console.log command in the code:

Diese Ausgabe in der Konsole wird vom "console.log"-Befehl erzeugt:

```javascript
const data = JSON.parse(this.responseText)
console.log(data)
```

> So, after receiving data from the server, the code prints it to the console. 

Das bedeudet, dass nach dem Erhalt der Daten vom Server, diese in der Konsole angezeigt werden.

> The Console tab and the console.log command will become very familiar to you during the course. 

Mit dem Console-Tab und dem console-log-Befehl werdet ihr in diesem Kurs noch sehr vertraut werden.

> Event handlers and Callback functions

## Event Handlers und Callback-Funktionen

> The structure of this code is a bit odd:

Die Struktur von diesem Quellcode ist ein bisschen seltsam:

```javascript
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  // code that takes care of the server response
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

> The request to the server is sent on the last line, but the code to handle the response can be found further up. What's going on? 

Die Serveranfrage wird mit der letzten Zeile abgeschickt, aber der Quellcode, der die Antwort abarbeitet steht weiter oben. Was passiert hier?

```javascript
xhttp.onreadystatechange = function () {
```

> On this line, an event handler for event onreadystatechange is defined for the xhttp object doing the request. When the state of the object changes, the browser calls the event handler function. The function code checks that the readyState equals 4 (which depicts the situation The operation is complete) and that the HTTP status code of the response is 200. 

In dieser Zeile wird ein Event Handler für das Event onreadystatechange für das xhttp-Object definiert, das die Anfrage startet. When sich der Zustand des Objektes verändert, dann ruft der Browser die Event Handler-Funktion auf. Der Quellcode der Funktion überprüft, ob der readyState gleich 4 ist (was bedeutet, das die Operation vollständig durchgeführt wurde) und ob der HTTP Statuscode der Antwort 200 ist.

```javascript
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    // code that takes care of the server response
  }
}
```

> The mechanism of invoking event handlers is very common in JavaScript. Event handler functions are called callback functions. The application code does not invoke the functions itself, but the runtime environment - the browser, invokes the function at an appropriate time, when the event has occurred. 

Event Handler aufzurufen ist sehr gebräuchlich in der Javascriptentwicklung. Event Handler-Funktionen werden Callback-Funktionen gennant. Der Anwendungsquellcode ruft die Funktionen nicht selbst auf, sondern die Laufzeitumgebung - der Browser selbst - ruft die Funktion zur passenden Zeit, nämlich wenn das Ereignis eintritt, auf.

> Document Object Model or DOM

## Document Object Model bzw DOM

> We can think of HTML-pages as implicit tree structures.

Wir können uns HTML-Seiten als geschlossene Baumstrukturen vorstellen.

```
html
  head
    link
    script
  body
    div
      h1
      div
        ul
          li
          li
          li
      form
        input
        input
```

> The same treelike structure can be seen on the console tab Elements.

Die gleiche baumartige Struktur kann man im Konsolentab "Elements" sehen.

!["A screenshot of the Elements tab of the developer console"](./images/part0b_image14.png?raw=true)

> The functioning of the browser is based on the idea of depicting HTML elements as a tree.

Die Funktionstüchtigkeit des Browser basiert auf der Idee HTML-Elemente als Baum darzustellen.

> Document Object Model, or DOM, is an Application Programming Interface (API) which enables programmatic modification of the element trees corresponding to web-pages.

Das Document Object Model, oder kurz DOM, ist eine API (Application Programming Interface), die die Veränderung der Baumelemente von Webseiten ermöglicht.

> The JavaScript code introduced in the previous chapter used the DOM-API to add a list of notes to the page.

Der Javascriptquellcode, der im vorangegangen Kapitel vorgestellt wurde, nutzt die DOM-API, um eine Liste der Notizen auf der Webseite darzustellen.

> The following code creates a new node to the variable ul, and adds some child nodes to it: 

Der folgende Quellcode erstellt einen Knoten für die Variable ul und fügt einige untergeordnete Knoten hinzu:

```javascript
var ul = document.createElement('ul')

data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li)
  li.appendChild(document.createTextNode(note.content))
})
```

> Finally, the tree branch of the ul variable is connected to its proper place in the HTML tree of the whole page: 

Schließlich wird in der HTML-Struktur der ganzen Seite, der Teil der ul-Variable eingefügt:

```javascript
document.getElementsByClassName('notes').appendChild(ul)
```

> Manipulating the document-object from console

## Bearbeiten des document objects von der Konsole aus

> The topmost node of the DOM tree of an HTML document is called the document object. We can perform various operations on a web-page using the DOM-API. You can access the document object by typing document into the Console-tab:

Der oberste Knoten des DOM-Baumes eines HTML-Dokuments wird document object genannt. Wir können eine Webseite mit der DOM-API auf verschiedene Weise bearbeiten. Auf das document object könnt ihr direkt in der Konsole zugreifen, indem ihr document eingebt:

!["A screenshot of the Elements tab of the developer console"](./images/part0b_image15.png?raw=true)

> Let's add a new note to the page from the console.

Jetzt hängen wir über die Konsole eine neue Notiz an die Webseite.

> First, we'll get the list of notes from the page. The list is in the first ul-element of the page: 

Jetzt fügen wir über die Konsole eine neue Notiz in die Webseite ein.

```javascript
list = document.getElementsByTagName('ul')[0]
```

> Then create a new li-element and add some text content to it:

Dann erstellen wir ein neues li-Element und ergänzen es mit ein wenig Text:

```javascript
newElement = document.createElement('li')
newElement.textContent = 'Page manipulation from console is easy'

```

> And add the new li-element to the list:

Und dann hängen wir das li-Element an die Liste:

```javascript
list.appendChild(newElement)
```

!["Screenshot of the page with the new note added to the list"](./images/part0b_image16.png?raw=true)

> Even though the page updates on your browser, the changes are not permanent. If the page is reloaded, the new note will disappear, because the changes were not pushed to the server. The JavaScript code the browser fetches will always create the list of notes based on JSON-data from the address https://studies.cs.helsinki.fi/exampleapp/data.json.

Auch wenn die Seite in eurem Browser verändert wird, sind die Änderungen nicht dauerhaft. Wenn die Seite neugeladen wird, verschwindet die Notiz, da die Änderungen nicht an den Server übertragen wurden. Der JavaScriptquellcode, der vom Browser geladen wird, erstellt immer eine Liste mit den Notizen aus den JSON-Daten, die von der Adresse https://studies.cs.helsinki.fi/exampleapp/data.json geladen werden.

> CSS

## CSS

> The head element of the HTML code of the Notes page contains a link tag, which determines that the browser must fetch a CSS style sheet from the address main.css.

Das Head-Element des HTML-Quellcode enthält einen Link-Tag, der festlegt, dass der Browser die CSS-Datei main.css laden soll.

> Cascading Style Sheets, or CSS, is a style sheet language used to determine the appearance of web pages.

Cascading Style Sheets, oder kurz CSS, ist eine Style Sheet-Sprache, die genutzt wird, um das Aussehen von Webseiten festzulegen.

> The fetched CSS-file looks as follows: 

Die CSS-Datei sieht folgendermaßen aus:

```css
.container {
  padding: 10px;
  border: 1px solid; 
}

.notes {
  color: blue;
}
```

> The file defines two class selectors. These are used to select certain parts of the page and to define styling rules to style them.

Die Datei definiert 2 Class-Selectors. Diese werden benutzt, um bestimmte Teile der Seite auszuwählen und Regeln für deren Aussehen zu bestimmen.

> A class selector definition always starts with a period, and contains the name of the class.

Eine Class-Selector-Definition beginnt immer mit einem Punkt und enthält den Namen der Klasse.

> The classes are attributes, which can be added to HTML elements.

Klassen sind Attribute, die HTML-Elementen hinzugefügt werden können.

> CSS attributes can be examined on the elements tab of the console: 

CSS-Attribute können im Elements-Tab der Konsole betrachtet werden:

!["Screenshot of the Elements tab on the developer console"](./images/part0b_image17.png?raw=true)

> The outermost div element has the class container. The ul element containing the list of notes has the class notes.

Das äußerste Div-Element hat die Klasse "container". Das ul-Element, das die Notizenliste enthält, hat die Klasse "notes".

> The CSS rule defines that elements with the container class will be outlined with a one pixel wide border. It also sets 10 pixel padding on the element. This adds some empty space between the element's content and the border.

Die CSS-Regel bestimmt, dass Elemente mit der Klasse "container" eine 1 Pixel-breite Umrandung haben. Die Klasse legt auch einen Padding-Wert von 10 Pixeln fest. Dadurch gibt es einen freien Bereich zwischen den Elementen und deren Umrandung.

> The second CSS rule sets the text color of the notes as blue.

Die zweite CSS-Regel ändert die Textfarbe der Notizen zu blau.

> HTML elements can also have other attributes apart from classes. The div element containing the notes has an id attribute. JavaScript code uses the id to find the element.

HTML-Elemente können auch andere Attribute als Klassen haben. Das Div-Element, dass die Notizen enthält, hat auch ein id-Attribut. Der Javascriptquellcode nutzt diese id, um das Element zu finden.

> The Elements tab of the console can be used to change the styles of the elements.

Der Elements-Tab der Konsole kann dazu benutzt werden, das Aussehen der verschiedenen Elemente zu ändern.

!["fullstack content"](./images/part0b_image18.png?raw=true)

> Changes made on the console will not be permanent. If you want to make lasting changes, they must be saved to the CSS style sheet on the server. 

Veränderungen der Seite, die in der Konsole durchgeführt werden, sind nicht dauerhaft. Wenn ihr dauerhafte Änderungen durchführen wollt, muss die CSS-Datei auf dem Server gespeichert werden.

> Loading a page containing JavaScript - review

## Wie eine Seite geladen wird die JavaScript enthält

> Let's review what happens when the page https://studies.cs.helsinki.fi/exampleapp/notes is opened on the browser. 

Schauen wir nach, was passiert, wenn die Seite https://studies.cs.helsinki.fi/exampleapp/notes im Browser geöffnet wird.

!["fullstack content"](./images/part0b_image19.png?raw=true)

> - The browser fetches the HTML code defining the content and the structure of the page from the server using an HTTP GET request.

Über eine HTTP GET-Anfrage lädt der Browser den HTML-Quellcode, der den Inhalt und die Struktur der Seite bestimmt.

> - Links in the HTML code cause the browser to also fetch the CSS style sheet main.css...

- Die Links im HTML-Quellcode veranlassen den Browser auch die CSS-Datei main.css zu laden...

> - ...and a JavaScript code file main.js

- ... ebenso wie die JavaScript-Datei main.js

> - The browser executes the JavaScript code. The code makes an HTTP GET request to the address https://studies.cs.helsinki.fi/exampleapp/data.json, which returns the notes as JSON data. 

- Der Browser für den JavaScript-Quellecode aus. Dadurch wird eine HTTP GET-Anfrage an die Adresse https://studies.cs.helsinki.fi/exampleapp/data.json geschickt, woraufhin die Notizen als JSON-Daten zurückkommen.

> - When the data has been fetched, the browser executes an event handler, which renders the notes to the page using the DOM-API. 

- Wenn die Daten geladen wurden, führt der Browser einen Event Handler aus, der Notizen auf der Seite anzeigt, indem er die DOM-API nutzt.

> Forms and HTTP POST

## Formulare und HTTP POST

> Next let's examine how adding a new note is done. 

Als Nächstes untersuchen wir, wie das Hinzufügen einer neuen Notiz funktioniert.

> The Notes page contains a form-element.

Die Notizenseite enthält ein form-Element.

!["fullstack content"](./images/part0b_image20.png?raw=true)

> When the button on the form is clicked, the browser will send the user input to the server. Let's open the Network tab and see what submitting the form looks like: 

Wenn der Button des Formulars geklickt wird, sendet der Browser die Benutzereingabe zum Server. Öffnen wir den Network-Tab und sehen uns an, wie das Absenden des Formulars aussieht:

!["fullstack content"](./images/part0b_image21.png?raw=true)

> Surprisingly, submitting the form causes no less than five HTTP requests. The first one is the form submit event. Let's zoom into it: 

Überraschenderweise löst das Abschicken des Formulars nicht weniger als fünf HTTP-Anfragen aus. Die Erste ist das Abschicken selbst. Schauen wir genauer hin:

!["fullstack content"](./images/part0b_image22.png?raw=true)

> It is an HTTP POST request to the server address new_note. The server responds with HTTP status code 302. This is a URL redirect, with which the server asks the browser to do a new HTTP GET request to the address defined in the header's Location - the address notes.

Wir sehen eine HTTP POST-Anfrage an die Serveradresse new_note. Der Server antwortet mit dem HTTP-Statuscode 302. Das ist eine URL-Umleitung mit der der Server den Browser auffordert, eine neue HTTP-Anfrage zu starten, deren Adresse im Header definiert ist.

> So, the browser reloads the Notes page. The reload causes three more HTTP requests: fetching the style sheet (main.css), the JavaScript code (main.js), and the raw data of the notes (data.json).

Dadurch lädt der Server die Notizenseite erneut. Dieses Neuladen verursacht drei weitere HTTP-Anfragen: das Laden der CSS-Datei (main.css), die JavaScript-Datei (main.js), und die Rohdaten der Notizen (data.json).

> The network tab also shows the data submitted with the form:

Der Network-Tab zeigt auch die Daten, die mit dem Formular abgeschickt werden.

!["fullstack content"](./images/part0b_image23.png?raw=true)

> The Form tag has attributes action and method, which define that submitting the form is done as an HTTP POST request to the address new_note. 

Der Form-Tag hat die Attribute "action" und "method", die festlegen, dass das Abschicken des Formlars als HTTP POST-Anfrage an die Adresse new_note geschieht.

!["fullstack content"](./images/part0b_image24.png?raw=true)

> The code on the server responsible for the POST request is quite simple (NB: this code is on the server, and not on the JavaScript code fetched by the browser):

Der Quellcode auf dem Server, der für POST-Anfragen zuständig ist, ist relativ einfach (Hinweis: der Quellcode liegt auf dem Server und ist nicht die JavaScript-Datei, die heruntergeladen wurde):

```javascript
app.post('/new_note', (req, res) => {
  notes.push({
    content: req.body.note,
    date: new Date(),
  })

  return res.redirect('/notes')
})
```

> Data is sent as the body of the POST-request. 

Die über das Formular abgeschickten Daten, werden im "body" der HTTP POST-Anfrage verschickt.

> The server can access the data by accessing the req.body field of the request object req.

Der Server kann auf diese Daten zu greifen, in dem er das Feld "req.body" des Request Objects "req" einsieht.

> The server creates a new note object, and adds it to an array called notes.

Der Server erstellt ein neues Notizobjekt und hängt es an das Array "notes":

```javascript
notes.push({
  content: req.body.note,
  date: new Date(),
})
```

> The Note objects have two fields: content containing the actual content of the note, and date containing the date and time the note was created. 

Das Notizobjekt hat zwei Felder: "content" enthält den tatsächlichen Inhalt der Notiz und "date" enthält das Datum und die Uhrzeit, an dem die Notiz erstellt wurde.

> The server does not save new notes to a database, so new notes disappear when the server is restarted. 

Der Server speichert keine neuen Notizen, so dass die Notizen verschwinden, wenn der Server neugestartet wird.

> AJAX

## AJAX

> The Notes page of the application follows an early-nineties style of web development and uses "Ajax". As such, it's on the crest of the wave of early 2000's web technology.

Die Notizenseite folgt einem Webentwicklungstrend aus den frühen 90ern und nutzt "Ajax". Als solches hatte es seine Hochzeit in den frühen 00er Jahren.

> AJAX (Asynchronous JavaScript and XML) is a term introduced in February 2005 on the back of advancements in browser technology to describe a new revolutionary approach that enabled the fetching of content to web pages using JavaScript included within the HTML, without the need to rerender the page.

AJAX (Asynchronous JavaScript and XML) ist ein Begriff, der im Februar 2005 eingeführt wurde und einen revolutionären Ansatz beschreibt, wie Inhalt von Webseiten geladen werden ohne die Seite erneut zu laden.

> Prior to the AJAX era, all web pages worked like the traditional web application we saw earlier in this chapter. All of the data shown on the page was fetched with the HTML-code generated by the server.

Vor der AJAX-Zeit funktionierten alle Webseiten wie unsere traditionelle Webanwendung, die wir schon eher in diesem Kapitel gesehen haben. Alle Daten, die auf der Webseite dargestellt werden

> The Notes page uses AJAX to fetch the notes data. Submitting the form still uses the traditional mechanism of submitting web-forms.

Die Notizenseite lädt die Daten der Notizen über AJAX. Das Abschicken des Formulars geschieht immer noch über traditionelle Techniken.

> The application URLs reflect the old, carefree times. JSON data is fetched from the url https://studies.cs.helsinki.fi/exampleapp/data.json and new notes are sent to the URL https://studies.cs.helsinki.fi/exampleapp/new_note. Nowadays URLs like these would not be considered acceptable, as they don't follow the generally acknowledged conventions of RESTful APIs, which we'll look into more in part 3.

Die URLS der Anwendung spiegeln die alten sorgenfreien Zeiten wieder. Die JSON-Daten werden von der URL https://studies.cs.helsinki.fi/exampleapp/data.json geladen und neue Notizen werden an die URL https://studies.cs.helsinki.fi/exampleapp/new_note gesendet. Heutzutage werden solche URLS nicht mehr akzeptiert, da sie nicht den anerkannten Konventionen von RESTful APIs (mehr davon in Abschnitt3) folgen.

> The thing termed AJAX is now so commonplace that it's taken for granted. The term has faded into oblivion, and the new generation has not even heard of it. 

AJAX ist heute so verbreitet, dass es als selbstverständlich angenommen wird. Der Begriff ist in Vergessenheit geraten und die neue Generation hat noch nicht mal davon gehört.

## Single page app

> In our example app, the home page works like a traditional web-page: All of the logic is on the server, and the browser only renders the HTML as instructed.

In unserer Beispielapp funktioniert die Homepage wie eine traditionelle Webseite: Die komplette Logik liegt auf dem Server, der Browser ist nur für die HTML-Darstellung zuständig.

> The Notes page gives some of the responsibility, generating the HTML code for existing notes, to the browser. The browser tackles this task by executing the JavaScript code it fetched from the server. The code fetches the notes from the server as JSON-data and adds HTML elements for displaying the notes to the page using the DOM-API.

Die Notizenseite gibt einen Teil der Arbeit, das Generieren des HTML-Codes für die Notizen, an den Brower ab.

> In recent years, the Single-page application (SPA) style of creating web-applications has emerged. SPA-style websites don't fetch all of their pages separately from the server like our sample application does, but instead comprise only one HTML page fetched from the server, the contents of which are manipulated with JavaScript that executes in the browser.

Seit ein paar Jahren gibt es einen Trend, dass man Webanwendungen als SPAs (single page apps) erstellt.

> The Notes page of our application bears some resemblance to SPA-style apps, but it's not quite there yet. Even though the logic for rendering the notes is run on the browser, the page still uses the traditional way of adding new notes. The data is sent to the server with form submit, and the server instructs the browser to reload the Notes page with a redirect.

Die Notizenseite hat ein wenig Ähnlichkeit mit SPAs, aber nicht wirklich viel. Auch wenn die Logik für das Darstellen der Notizen im Browser läuft, werden neue Notizen noch über traditionelle Wege erstellt. Die Daten werden über das Abschicken des Formulars an den Server geschickt und der Server weist den Browser über eine Umleitung an, die Notizenseite neuzuladen.

> A single page app version of our example application can be found at https://studies.cs.helsinki.fi/exampleapp/spa. At first glance, the application looks exactly the same as the previous one. The HTML code is almost identical, but the JavaScript file is different (spa.js) and there is a small change in how the form-tag is defined: 

Eine Single page app-Version unserer Beispielanwendung findet man unter https://studies.cs.helsinki.fi/exampleapp/spa. Auf den ersten Blick sieht die Anwendung genauso aus, wie die andere. Der HTML-Code ist fast identisch, aber die JavaScript-Datei ist eine andere (spa.js) und es gibt eine kleine Änderung, wie der form-Tag definiert wird:

!["fullstack content"](./images/part0b_image25.png?raw=true)

> The form has no action or method attributes to define how and where to send the input data.

Das Formular hat keine action- oder method-Attribut, die definieren wie und wohin die Formulardaten geschickt werden sollen.

> Open the Network-tab and empty it. When you now create a new note, you'll notice that the browser sends only one request to the server. 

Öffnet den Network-Tab und löscht ihn. Wenn ihr jetzt eine neue Notiz erstellt, werdet ihr bemerken, dass der Browser nur eine Anfrage an den Server schickt:

!["fullstack content"](./images/part0b_image26.png?raw=true)

> The POST request to the address new_note_spa contains the new note as JSON-data containing both the content of the note (content) and the timestamp (date): 

Die POST-Anfrage an die Adresse new_note_spa schickt die Notiz im JSON-Format, in dem der Inhalt der Notiz (content) und der Zeitstempel (date) enthalten sind.

```json
{
  content: "single page app does not reload the whole page",
  date: "2019-05-25T15:15:59.905Z"
}
```

> The Content-Type header of the request tells the server that the included data is represented in the JSON format. 

Der "Content-Type-Header" der Anfrage sagt dem Server, dass die mitgeschickten Daten im JSON-Format sind.

!["fullstack content"](./images/part0b_image27.png?raw=true)

> Without this header, the server would not know how to correctly parse the data.

Ohne diesen Header würde der Server nicht genau wissen, wie er die mitgeschickten Daten interpretieren soll.

> The server responds with status code 201 created. This time the server does not ask for a redirect, the browser stays on the same page, and it sends no further HTTP requests.

Der Server antwortet mit dem Statuscode "201 created". Dieses Mal fragt der Server nicht nach einer Weiterleitung, der Browser bleibt auf der selben Seite und stellt keine weiteren HTTP-Anfragen.

> The SPA version of the app does not send the form data in the traditional way, but instead uses the JavaScript code it fetched from the server. We'll look into this code a bit, even though understanding all the details of it is not important just yet. 

Die SPA-Version der App sendet die Formulardaten nicht auf traditionelle Weise, sondern benutzt stattdessen den JavaScript-Code, der vom Server geladen wird. Wir schauen uns jetzt den Quellcode an, wenn auch das Verständis aller Details unwichtig ist.

```javascript
var form = document.getElementById('notes_form')
form.onsubmit = function(e) {
  e.preventDefault()

  var note = {
    content: e.target.elements[0].value,
    date: new Date(),
  }

  notes.push(note)
  e.target.elements[0].value = ''
  redrawNotes()
  sendToServer(note)
}
```

> The command document.getElementById('notes_form') instructs the code to fetch the form-element from the page, and to register an event handler to handle the form submit event. The event handler immediately calls the method e.preventDefault() to prevent the default handling of form submit. The default method would send the data to the server and cause a new GET request, which we don't want to happen.

Der Befehl "document.getElementById('notes_form')" ruft das Formular der Seite auf und hängt einen Event Handler an das Formular, so dass das Abschicken des Formulars von diesem erledigt wird. Der Event Handler ruft sofort die Methode e.preventDefault() auf, um das voreingestellte Abschicken des Formulars zu unterbinden. Das voreingestellte Abschicken würde Daten an den Server sichken und eine GET-Anfrage auslösen, was wir verhindern wollen.

> Then the event handler creates a new note, adds it to the notes list with the command notes.push(note), rerenders the note list on the page and sends the new note to the server.

Dann erstellt der Event Handler eine neue Notiz, fügt sie mit dem Befehl notes.push(note) an die Notizenliste, zeigt die neue Notizenlist auf der Seite an und sendet die neue Noitz an den Server.

> The code for sending the note to the server is as follows: 

Der Quellcode für das Senden der Notiz an den Server sieht so aus:

```javascript
var sendToServer = function(note) {
  var xhttpForPost = new XMLHttpRequest()
  // ...

  xhttpForPost.open('POST', '/new_note_spa', true)
  xhttpForPost.setRequestHeader(
    'Content-type', 'application/json'
  )
  xhttpForPost.send(JSON.stringify(note))
}
```

> The code determines that the data is to be sent with an HTTP POST request and the data type is to be JSON. The data type is determined with a Content-type header. Then the data is sent as JSON-string.

Dieser Quellcode definiert, dass die Daten per HTTP POST-Anfrage geschickt werden und dass der Datentyp JSON ist. Der Datentyp wird durch den Content-Type-Header bestimmt. Dann werden alle Daten als JSON-String verschickt.

> The application code is available at https://github.com/mluukkai/example_app. It's worth remembering that the application is only meant to demonstrate the concepts of the course. The code follows a poor style of development in some measure, and should not be used as an example when creating your own applications. The same is true for the URLs used. The URL new_note_spa, which new notes are sent to, does not adhere to current best practices. 

Der Quellcode der Anwendung ist unter https://github.com/mluukkai/example_app verfügbar. Denkt dran, dass dieser nur die Konzepte des Kurses demonstrieren soll. In gewissen Maße folgt er einem schlechten Programmier-/Entwicklungsstil und sollte auf keinen Fall als Vorbild dafür dienen, wie ihr Anwendungen baut. Das gleiche gilt für die URLS, die hier benutzt werden. Die URL new_note_spa, an die neuen Notizen geschickt werden, hält sich nicht an aktuelle "best practices".

## Javascript-libraries

> The sample app is done with so called vanilla JavaScript, using only the DOM-API and JavaScript to manipulate the structure of the pages.

Die Beispielanwendung ist in sogenanntem Vanilla JavaScript geschrieben und nutzt nur dieses und die DOM-API, um die Strukturen der Seiten zu verändern.

> Instead of using JavaScript and the DOM-API only, different libraries containing tools that are easier to work with compared to the DOM-API are often used to manipulate pages. One of these libraries is the ever-so-popular jQuery.

Anstatt nur reinem JavaScript und DOM-API, kann man auch verschiedene Bibliotheken nutzen, mit denen sich viel leichter Webseiten verändern lassen. Eine dieser Bilbiotheken ist jQuery.

> jQuery was developed back when web applications mainly followed the traditional style of the server generating HTML pages, the functionality of which was enhanced on the browser side using JavaScript written with jQuery. One of the reasons for the success of jQuery was its so-called cross-browser compatibility. The library worked regardless of the browser or the company that made it, so there was no need for browser-specific solutions. Nowadays using jQuery is not as justified given the advancement of JavaScript, and the most popular browsers generally support basic functionalities well.

jQuery wurde entwickelt, als Webapplikationen noch hauptsächlich aus HTML-Seiten bestanden, die von Server generiert wurden. Diese Funktionalität wurde durch, auf Browserseite genutztem, JavaScript und jQuery erweitert. Eine der Gründe für den Erfolg von jQuery war seine Kompabilität über mehrere Browser. Die Bibliothek arbeitete unabhängig vom eingesetzten Browser oder der Firma, die ihn erstellt hat. Dadurch waren keine browserspezifischen Lösungen mehr nötig. Heutzutage ist wegen der Weiterentwicklung von JavaScript der Einsatz von jQuery nicht mehr gerechtfertigt und die meisten Browser unterstützen sowieso die meisten Grundfunktionien.

> The rise of the single page app brought several more "modern" ways of web development than jQuery. The favorite of the first wave of developers was BackboneJS. After its launch in 2012, Google's AngularJS quickly became almost the de facto standard of modern web development.

Der Aufstieg der SPAs brauchte auch mehrere verschiedene "moderne" Wege zur Webentwicklung zum Vorschein als jQuery. Der Liebling der ersten Welle von Entwicklern war BackboneJS. Nach seiner Einführung 2012 wurde Googles AngularJS schnell zum Quasistandard der Webentwicklung.

> However, the popularity of Angular plummeted in October 2014 after the Angular team announced that support for version 1 will end, and Angular 2 will not be backwards compatible with the first version. Angular 2 and the newer versions have not gotten too warm of a welcome.

Wie auch immer, die Beliebtheit von Angular stürzte im Oktober 2014 ab, als das Angularteam bekannt gab den Support für Version 1 einzustellen und das Angular 2 nicht rückwärtskompatibel sein wird. Angular 2 und neuere Versionen erreichten nie die Beliebtheit von Version 1.

> Currently the most popular tool for implementing the browser-side logic of web-applications is Facebook's React library. During this course, we will get familiar with React and the Redux library, which are frequently used together.

Das momentan beliebteste Werkzeug für das Implementieren von Anwendungslogik auf Browserseite ist Facebooks React-Bibliothek. In diesem Kurs werden wir uns mit der React- und Redux-Bibliothek vertraut machen, die sehr häufig zusammen genutzt werden.

> The status of React seems strong, but the world of JavaScript is ever changing. For example, recently a newcomer - VueJS - has been capturing some interest. 

Der Beliebtheitsgrad von React ist sehr groß, aber die Welt von JavaScript ändert sich ständig. Es gibt z.B. zur Zeit einen Newcomer - VueJs - der für einigies Interesse sorgt.

## Full stack web development

> What does the name of the course, Full stack web development, mean? Full stack is a buzzword that everyone talks about, while no one really knows what it means. Or at least, there is no agreed-upon definition for the term.

Was bedeutet eigentlich Full Stack Webentwicklung? Full Stack ist ein Modewort, über das jeder redet, aber keiner so wirklich weiß, was es bedeutet. Oder zumindest gibt es keine allgemein gültige Definition für diesen Begriff.

> Practically all web applications have (at least) two "layers": the browser, being closer to the end-user, is the top layer, and the server the bottom one. There is often also a database layer below the server. We can therefore think of the architecture of a web application as a kind of stack of layers.

Praktisch gesehen habe alle Webanwendung (mindestens) 2 "Schichten": den Browser (die obere Schicht), der näher am Endbenutzer ist und den Server (die untere Schicht). Es gibt oft noch eine Datenbankschicht unter der Serverschicht. Wir können uns daher die Architektur von Webanwendungen als einen Stapel (Stack) verschiedener Schichten vorstellen.

> Often, we also talk about the frontend and the backend. The browser is the frontend, and JavaScript that runs on the browser is frontend code. The server on the other hand is the backend.

Oft wird auch vom Frontend und Backend gesprochen. Der Browser ist das Frontend und JavaScript, das im Browser läuft, ist Fontendcode. Der Server auf der anderen Seite, ist Backend.

> In the context of this course, full stack web development means that we focus on all parts of the application: the frontend, the backend, and the database. Sometimes the software on the server and its operating system are seen as parts of the stack, but we won't go into those.

Im Zusamenhang mit diesem Kurses bedeutet Full Stack Webentwicklung das wir uns auf alle teile einer Anwendung konzentrieren: das Frontend, das Backend und die Datenbank. Manchmal wird die Software auf dem Server und sein Betriebssystem als Teile des Stacks bezeichnet, aber damit werden wir uns nicht beschäftigen.

> We will code the backend with JavaScript, using the Node.js runtime environment. Using the same programming language on multiple layers of the stack gives full stack web development a whole new dimension. However, it's not a requirement of full stack web development to use the same programming language (JavaScript) for all layers of the stack.

Wir werden das Backend mit Javascript schreiben, in dem wir die Node.js-Entwicklungsumgebung nutzen. Die gleiche Programmiersprache in den verschiedenen Schichten des Stacks nutzen gibt der Webentwicklung eine ganz andere Dimension. Wie auch immer ist es keine Vorraussetzung für Full Stack Entwicklung die gleiche Sprache (JavaScript) für alle Schichten des Stacks zu benutzen.

> It used to be more common for developers to specialize in one layer of the stack, for example the backend. Technologies on the backend and the frontend were quite different. With the Full stack trend, it has become common for developers to be proficient on all layers of the application and the database. Oftentimes, full stack developers must also have enough configuration and administration skills to operate their application, for example, in the cloud. 

Es war üblicher, dass sich Entwickler auf eine Schicht des Stacks konzentriert haben, z.B. auf das Backend. Die Technologien auf dem Backend und dem Frontend ware sehr verschieden. Mit dem Full Stack-Trend ist es jetzt verbreiteter, dass sich Entwickler auf allen Schichten der Anwendung auskennen, ebenso wie der Datenbank. Oft müssen Full Stack-Entwickler auch genug Konfigurations- und Administrationsfähigkeiten besizen, um ihre Anwendungen z.B. in der Cloud laufen zu lassen.

## JavaScript fatigue

> Full stack web development is challenging in many ways. Things are happening in many places at once, and debugging is quite a bit harder than with regular desktop applications. JavaScript does not always work as you'd expect it to (compared to many other languages), and the asynchronous way its runtime environments work causes all sorts of challenges. Communicating on the web requires knowledge of the HTTP protocol. One must also handle databases and server administration and configuration. It would also be good to know enough CSS to make applications at least somewhat presentable.

Die Full Stack-Webentwicklung ist auf vielen Seiten herausfordernd. Sachen passieren an vielen Orten gleichzeitig, debuggen ist schwieriger im Vergleich zu regulären Desktopanwendungen. JavaScript funktioniert auch nicht immer, wie man erwartet (im Vergleich zu vielen anderen Sprachen), und die asynchrone Weise, auf die seine Entwicklungsumgebung arbeitet, verursacht alle Arten von Herausforderungen. Um mit dem Server zu kommunizieren, benötigt es Wissen über HTTP. Außerdem muss man mit Datenbanken umgehen können und Server verwalten und konfigurieren können. Es wäre auch gut zu wissen, wie man CSS einsetzt, um die Anwendungen präsentieren zu können.

> The world of JavaScript develops fast, which brings its own set of challenges. Tools, libraries and the language itself are under constant development. Some are starting to get tired of the constant change, and have coined a term for it: JavaScript fatigue. See How to Manage JavaScript Fatigue on auth0 or JavaScript fatigue on Medium.

Die Welt von JavaScript ändert sich schnell, was zu einer eigenen Reihe von Herausforderungen führt. Werkzeuge, Bibliothken und die Sprache selber sind unter konstanter Entwicklung. Manche werden langsam müde von der dauerhaften Veränderung und haben dafür ein Wort gefunden: JavaScript-Müdigkeit. Wie ihr damit umzugehen lernt, könnt ihr [hier](https://auth0.com/blog/how-to-manage-javascript-fatigue/) oder [hier](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4) nachlesen.

> You will suffer from JavaScript fatigue yourself during this course. Fortunately for us, there are a few ways to smooth the learning curve, and we can start with coding instead of configuration. We can't avoid configuration completely, but we can merrily push ahead in the next few weeks while avoiding the worst of configuration hells. 

Auch ihr werden während des Kurses an JavaScript-Müdigkeit leiden. Glücklicherweise gibt es ein paar Arten, wie man die Lernkurve abschwächen kann und wir beginnen damit, indem wir programmieren anstatt zu konfigurieren. Wir können das Konfigurieren zwar nicht ganz verhindern, aber wir können es um ein paar Wochen verschieben.

## Exercises

> The exercises are submitted via GitHub, and by marking the exercises as done in the submission system.

Die Aufgaben werden über GitHub eingereicht und in dem sie als erledigt im Submissionsystem markiert sind.

> You can submit all of the exercises into the same repository, or use multiple different repositories. If you submit exercises from different parts into the same repository, name your directories well. If you use a private repository to submit the exercises, add mluukkai as a collaborator to it.

Ihr könnt alle Aufgaben über eine Repository einreichen oder verschiedene Repositories benutzen. Bitte benennt eure Verzeichnisse korrekt, wenn ihr Aufgaben verschiedener Abschnitte im selben Repository einreicht. Wenn ihr ein privates Repository mit Aufgaben einreichen wollt, fügt mluukkai Collaborator hinzu.

> One good way to name the directories in your submission repository is as follows: 

Ein Beispiel für eine gelungene Benennung eurer Verzeichnisse könnte so aussehen:

```
part0
part1
  courseinfo
  unicafe
  anecdotes
part2
  courseinfo
  phonebook
  countries
```

> So, each part has its own directory, which contains a directory for each exercise set (like the unicafe exercises in part 1).

Dadurch hat jeder Abschnitt sein eigenes Verzeichnis, dass jeweils ein Verzeichnis für jede Aufgabenzusammenstellung enthält (z.B. die unicafe-Aufgaben in Abschnitt 1).

> The exercises are submitted one part at a time. When you have submitted the exercises for a part, you can no longer submit any missed exercises for that part.

Die Aufgaben werden Abschnitt für Abschnitt eingereicht. Wenn ihr Aufgaben für einen Abschnitt eingereicht habt, könnt ihr nicht länger noch fehlende Aufgaben für diesen Abschnitt abgeben.

### 0.1: HTML

> Review the basics of HTML by reading this tutorial from Mozilla: HTML tutorial.
> This exercise is not submitted to GitHub, it's enough to just read the tutorial

Schaut euch nochmal die Grundlagen von HTML an, in dem ihr diese Anleitung von Mozilla lest: [HTML Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)

Bei dieser Aufgabe müsst ihr nichts abgeben, nur den Link lesen.

### 0.2: CSS

> Review the basics of CSS by reading this tutorial from Mozilla: CSS tutorial.
> This exercise is not submitted to GitHub, it's enough to just read the tutorial

Schaut euch nochmal die Grundlagen von HTML an, in dem ihr diese Anleitung von Mozilla lest: [CSS Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics)

Bei dieser Aufgabe müsst ihr nichts abgeben, nur den Link lesen.

### 0.3: HTML forms

> Learn about the basics of HTML forms by reading Mozilla's tutorial Your first form.
> This exercise is not submitted to GitHub, it's enough to just read the tutorial

Lernt über die Grundlagen von HTML-Formularen, indem ihr diese Anleitung von Mozilla lest: [Your first form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form)

### 0.4: New note

> In chapter Loading a page containing JavaScript - review the chain of events caused by opening the page https://studies.cs.helsinki.fi/exampleapp/notes is depicted as a sequence diagram

Im Abschnitt "Wie eine Seite geladen wird, die Javascript enthält" wird die Abfolge der Ereignisse beim Öfnnen der Seite https://studies.cs.helsinki.fi/exampleapp/notes als Sequenzdiagramm dargestellt.

> The diagram was made using websequencediagrams service as follows: 

Das Diagramm wurde mithilfe von websequencediagrams service folgendermaßen erstellt:

```
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/notes
server-->browser: HTML-code
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.css
server-->browser: main.css
browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/main.js
server-->browser: main.js

note over browser:
browser starts executing js-code
that requests JSON data from server 
end note

browser->server: HTTP GET https://studies.cs.helsinki.fi/exampleapp/data.json
server-->browser: [{ content: "HTML is easy", date: "2019-05-23" }, ...]

note over browser:
browser executes the event handler
that renders notes to display
end note
```

> Create a similar diagram depicting the situation where the user creates a new note on page https://studies.cs.helsinki.fi/exampleapp/notes when writing something into the text field and clicking the submit button.

Erstellt ein ähnliches Diagramm, dass die Situation beschreibt, wo ein Benutzer eine neue Notiz auf der Seite https://studies.cs.helsinki.fi/exampleapp/notes erstellt, indem ihr etwas ins Textfeld schreibt und auf den Abschickenbutton klickt.

> If necessary, show operations on the browser or on the server as comments on the diagram.

Bei Bedarf könnt ihr die Operationen im Browser oder auf dem Server als Kommentare im Diagramm zeigen.

> The diagram does not have to be a sequence diagram. Any sensible way of presenting the events is fine.

Das Diagramm muss kein Sequenzdiagramm sein. Jede vernünfte Art, wie die Ereignisse dargestellt werden, ist in Ordnung.

> All necessary information for doing this, and the next two exercises, can be found from the text of this part. The idea of these exercises is to read the text through once more, and to think through what is going on there. Reading the application code is not necessary, but it is of course possible.

Alle benötigten Informationen für diese und die nächsten beiden Aufgaben findet ihr im Text dieses Abschnitts. Die Intention dieser Aufgaben ist es, erneut den Text zu lesen und darüber zu denken, was hier passiert. Den Quellcode der Anwendung zu lesen ist dafür nicht nötig, aber natürlich möglich.

> Note perhaps the best way to do diagrams is the Mermaid syntax that is now implemented in GitHub markdown pages! 

Hinweis: Der wahrscheinlich beste Weg um Diagramme zu erstellen (die Mermaid syntax) ist jetzt auch auf Github möglich.

### 0.5: Single page app

> Create a diagram depicting the situation where the user goes to the single page app version of the notes app at https://studies.cs.helsinki.fi/exampleapp/spa.

Erstellt ein Diagramm, dass die Situation beschreibt, wie ein Benutzer die Single page app-Version der Notizenseite unter https://studies.cs.helsinki.fi/exampleapp/spa öffnet.

### 0.6: New note

> Create a diagram depicting the situation where the user creates a new note using the single page version of the app.

> This was the last exercise, and it's time to push your answers to GitHub and mark the exercises as done in the submission system.


[part 1](part_1/part_1.md)
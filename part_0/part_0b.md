
> table of contents

# Inhaltsverzeichnis

- [Grundlagen von Webapplikationen](#grundlagen-von-webapplikationen)
- [HTTP GET](#HTTP-GET)
- [Traditionelle Webanwendungen](#Traditionelle-Webanwendungen)
- [Anwendungslogik im Browser laufen lassen](#Anwendungslogik-im-Browser-laufen-lassen)
- [Event Handlers und Callback-Funktionen](#Event-Handlers-und-Callback-Funktionen)
- [Document Object Model bzw. DOM](#Document-Object-Model-bzw.-DOM)
- [Bearbeiten des document objects von der Konsole aus](#Bearbeiten-des-document-objects-von-der-Konsole-aus)
- [Wie eine Seite geladen wird, die JavaScript enthält](#Wie-eine-Seite-geladen-wird,-die-JavaScript-enthält)
- [Formulare und HTTP POST](#Formulare-und-HTTP-POST)
- [AJAX](#AJAX)

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

## Document Object Model bzw. DOM

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

Zuerst laden wir die Liste der Notizen von der Seite. Die Liste ist das erste ul-Element:

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

Auch wenn die Seite in eurem Browser angepasst wird, sind die Änderungen nicht dauerhaft. Wenn die Seite neugeladen wird, verschwindet die neue Notiz, da die Änderungen nicht an den Server übertragen wurden. Der JavaScriptquellcode, der vom Browser geladen wird, erstellt immer eine Liste mit Notizen, die auf den JSON-Daten beruhen, die von der Adresse https://studies.cs.helsinki.fi/exampleapp/data.json geladen werden.

> CSS

## CSS

> The head element of the HTML code of the Notes page contains a link tag, which determines that the browser must fetch a CSS style sheet from the address main.css.

Das Head-Element des HTML-Quellcode der Notizenseite enthält einen Link-Tag, der festlegt, dass der Browser die CSS-Datei main.css laden soll.

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

Eine Class-Selector-Definition begint immer mit einem Punkt und enthält den Namen der Klasse.

> The classes are attributes, which can be added to HTML elements.

Klassen sind Attribute, die HTML-Elementen hinzugefügt werden können.

> CSS attributes can be examined on the elements tab of the console: 

CSS-Attribute können im Elements-Tab der Konsole betrachtet werden:

!["Screenshot of the Elements tab on the developer console"](./images/part0b_image17.png?raw=true)

> The outermost div element has the class container. The ul element containing the list of notes has the class notes.

Das äußerste Div-Element hat die Klasse "container". Das ul-Element, das die Notizenliste enthält, hat die Klasse "notes".

> The CSS rule defines that elements with the container class will be outlined with a one pixel wide border. It also sets 10 pixel padding on the element. This adds some empty space between the element's content and the border.

Die CSS-Regel bestimmt, dass Elemente mit der Klasse "container" eine 1 Pixel-breite Umrandung haben. Die Klasse legt auch einen Padding-Wert von 10 Pixeln für zugehörige Elemente fest. Dadurch gibt es einen freien Bereich zwischen dem Inhalt der Elemente und deren Umrandung.

> The second CSS rule sets the text color of the notes as blue.

Die zweite CSS-Regel besagt, dass die Textfarbe der Notizen blau ist.

> HTML elements can also have other attributes apart from classes. The div element containing the notes has an id attribute. JavaScript code uses the id to find the element.

HTML-Elemente können auch andere Attribute als Klassen haben. Das Div-Element, dass die Notizen enthält, hat auch ein id-Attribut. Der Javascriptquellcode nutzt diese id, um das Element zu finden.

> The Elements tab of the console can be used to change the styles of the elements.

Der Elements-Tab der Konsole kann dazu benutzt werden, das Aussehen der verschiedenen Elemente zu ändern.

!["fullstack content"](./images/part0b_image18.png?raw=true)

> Changes made on the console will not be permanent. If you want to make lasting changes, they must be saved to the CSS style sheet on the server. 

Veränderungen der Seite, die in der Konsole durchgeführt werden, sind nicht dauerhaft. Wenn ihr dauerhafte Änderungen durchführen wollt, muss die CSS-Datei auf dem Server gespeichert werden.

> Loading a page containing JavaScript - review

## Wie eine Seite geladen wird, die JavaScript enthält

> Let's review what happens when the page https://studies.cs.helsinki.fi/exampleapp/notes is opened on the browser. 

Schauen wir nach, was passiert, wenn die Seite https://studies.cs.helsinki.fi/exampleapp/notes im Browser geöffnet wird.

!["fullstack content"](./images/part0b_image19.png?raw=true)

> - The browser fetches the HTML code defining the content and the structure of the page from the server using an HTTP GET request.

Über eine HTTP GET-Anfrage lädet der Browser den HTML-Quellcode, der den Inhalt und die Struktur der Seite bestimmt.

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

Als Nächstes untersuchen wir, wie das Hinzufügen einer neuen Notiz funktioniert

> The Notes page contains a form-element.

Die Notizenseite enthält ein form-Element.

!["fullstack content"](./images/part0b_image20.png?raw=true)

> When the button on the form is clicked, the browser will send the user input to the server. Let's open the Network tab and see what submitting the form looks like: 

Wenn der Button des Formulars geklickt wird, sendet der Browser die Benutzereingabe zum Server. Öffnen wir den Network-Tab und sehen uns an, wir das Absenden des Formulars aussieht:

!["fullstack content"](./images/part0b_image21.png?raw=true)

> Surprisingly, submitting the form causes no less than five HTTP requests. The first one is the form submit event. Let's zoom into it: 

Überraschenderweise löst das Abschicken des Formulars nicht weniger als fünf HTTP-Anfragen aus. Die Erste ist das Abschicken selbst. Schauen wir genauer hin:

!["fullstack content"](./images/part0b_image22.png?raw=true)

> It is an HTTP POST request to the server address new_note. The server responds with HTTP status code 302. This is a URL redirect, with which the server asks the browser to do a new HTTP GET request to the address defined in the header's Location - the address notes.

Es ist eine HTTP POST-Anfrage an die Serveradresse new_note. Der Server antwortet mit dem HTTP-Statuscode 302. Das ist eine URL-Umleitung mit der der Server den Browser auffordert, eine neue HTTP-Anfrage zu starten, deren Adresse im Header definiert ist.

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

Der Server erstellt ein neues Notizobjekt und hängt es an das Array "notes".

```javascript
notes.push({
  content: req.body.note,
  date: new Date(),
})
```

> The Note objects have two fields: content containing the actual content of the note, and date containing the date and time the note was created. 

Das Notizobjekt hat zwei Felder: "content" enthält den tatsächlichen Inhalt der Notiz und "date" enthält das Datum und die Uhrzeit, an dem die Notiz erstellt wurde.

> The server does not save new notes to a database, so new notes disappear when the server is restarted. 

Der Server speichert keine neuen Notizen in einer Daten, so dass die Notizen verschwinden, wenn der Server neugestartet wird.

> AJAX

## AJAX
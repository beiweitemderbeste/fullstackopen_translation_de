
> table of contents

# Inhaltsverzeichnis

- [Grundlagen von Webapplikationen](#grundlagen-von-webapplikationen)
- [HTTP GET](#HTTP-GET)
- [Traditionelle Webanwendungen](#Traditionelle-Webanwendungen)

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

Die Konsole sieht ungefänr so aus:

!["A screenshot of the developer tools open in a browser"](./images/part0/part0b_image1.png?raw=true)

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

!["Screenshot of the developer console showing these two events"](./images/part0/part0b_image2.png?raw=true)

> On a small screen you might have to widen the console window to see these.

Auf kleineren Displays müsst ihr möglicherweise das Konsolenfenster vergrößern, um alles zu sehen.

> Clicking the first event reveals more information on what's happening: 

Wenn ihr auf das erste Ereignis klickt werden mehr Informationen darüber angezeigt, was passiert:

!["Detail view of a single event"](./images/part0/part0b_image3.png?raw=true)

> The upper part, General, shows that the browser made a request to the address https://studies.cs.helsinki.fi/exampleapp (though the address has changed slightly since this picture was taken) using the GET method, and that the request was successful, because the server response had the Status code 200.

Der obere Teil unter "General" zeigt, dass der Browser über die GET-Methode eine Anfrage zur Adresse https://studies.cs.helsinki.fi/exampleapp geschickt hat (Bitte beachtet, dass sich die adresse leicht geändert hat seitdem der Screenshot erstellt wurde). Die Anfrage war erfolgreich, weil die Serverantwort den Statuscode 200 hatte.

> The request and the server response have several headers:

Die Anfrage und die Serverantwort haben mehrere Header:

!["fullstack content"](./images/part0/part0b_image4.png?raw=true)

> The Response headers on top tell us e.g. the size of the response in bytes, and the exact time of the response. An important header Content-Type tells us that the response is a text file in utf-8-format, contents of which have been formatted with HTML. This way the browser knows the response to be a regular HTML-page, and to render it to the browser 'like a web page'.

Die oberen Antwortheader zeigen z.B. die Größe der Antwort in Bytes und die genaue Zeit der Antwort an. Der wichtige Header "Content-Type" zeigt an, dass die Antwort eine Textdatei im utf-8-Format ist, deren Inhalt mit HTML formattiert wurde. Auf diese Weise weiß der Browser, dass die Antwort eine reguläre HTML-Seite ist und stellt sie als eine "Webseite" dar.

> The Response tab shows the response data, a regular HTML-page. The body section determines the structure of the page rendered to the screen: 

Der "Response"-Tab zeigt die Antwortdaten, eine reguläre HTML-Seite. Die Body-Sektion definiert die Seitenstruktur, die am Bildschirm angezeigt wird:

!["Screenshot of the response tab"](./images/part0/part0b_image5.png?raw=true)

> The page contains a div element, which in turn contains a heading, a link to the page notes, and an img tag, and displays the number of notes created.

Die Seite enthält ein div-Element, das wiederum eine Überschrift enthält, einen Link zu den Seitennotizen und einen img-Tag, ebenso wie die Anzahl der Notizen, die erstellt wurden.

> Because of the img tag, the browser does a second HTTP-request to fetch the image kuva.png from the server. The details of the request are as follows: 

Wegen des img-Tags stellt der Browser eine zweite HTTP-Anfrage, um die Bilddatei kuva.png vom Server zu laden. Die Details der Anfrage sind folgendermaßen:

!["Detail view of the second event"](./images/part0/part0b_image6.png?raw=true)

> The request was made to the address https://studies.cs.helsinki.fi/exampleapp/kuva.png and its type is HTTP GET. The response headers tell us that the response size is 89350 bytes, and its Content-type is image/png, so it is a png image. The browser uses this information to render the image correctly to the screen. 

Die Anfrage (vom Typ HTTP GET) wurde an die Adresse https://studies.cs.helsinki.fi/exampleapp/kuva.png gestellt. Der Antwortheader zeigt uns, dass die Antwort 89350 Bytes groß war und ihr "Content-type" image/png ist. Also ist es eine png-Bilddatei. Der Browser benutzt diese Information, um die Bilddatei korrekt am Bildschirm darzustellen.

> The chain of events caused by opening the page https://studies.cs.helsinki.fi/exampleapp on a browser form the following sequence diagram:

So sieht die Ereigniskette beim Öffnen der Seite https://studies.cs.helsinki.fi/exampleapp in einem Browser in einem Sequenzdiagramm aus: 

!["Sequence diagram of the flow covered above"](./images/part0/part0b_image7.png?raw=true)

> First, the browser sends an HTTP GET request to the server to fetch the HTML code of the page. The img tag in the HTML prompts the browser to fetch the image kuva.png. The browser renders the HTML page and the image to the screen. 

Zuerst schickt der Browser eine HTTP GET-Anfrage an den Server, um den HTML-Quellcode der Seite zu laden. Der img-Tag in dem HTML-Dokument fordert den Browser auf, die Bilddatei kuva.png zu laden. Der Browser zeigt die HTML-Seite und die Bilddatei auf dem Bildschirm an.

> Even though it is difficult to notice, the HTML page begins to render before the image has been fetched from the server. 

Auch wenn es schwer zu bemerken ist: es wird begonnen die HTML-Seite darzustellen, bevor die Bilddatei vom Server geladen wurde.

> Traditional web applications 

## Traditionelle Webanwendungen

> The homepage of the example application works like a traditional web application. When entering the page, the browser fetches the HTML document detailing the structure and the textual content of the page from the server.

Die Homepage der Beispielanwendungen funktioniert wie eine traditionelle Webanwendung. Wenn die Seite aufgerufen wird, lädt der Browser vom Server das HTML-Dokument, das die Struktur und den textlichen Inhalt der Seite beschreibt.

> The server has formed this document somehow. The document can be a static text file saved into the server's directory. The server can also form the HTML documents dynamically according to the application code, using, for example, data from a database. The HTML code of the example application has been formed dynamically, because it contains information on the number of created notes. 

Der Server hat dieses Dokument irgendwie erstellt. Das Dokument kann eine statische Textdatei sein, die im Serververzeichnis abgespeichert wurde. Der Server kann auch dynamisch HTML-Dokumente erstellen in dem er bspw. Daten aus einer Datenbank lädt. Der HTML-Quellcode der Beispielanwendung wurde dynamisch erstellt, weil er die Anzahl der erstellten Notizen enthält.

> The HTML code of the homepage is as follows: 

Der HTML-Quellcode der Hompage lautet wie folgt:

```
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

Der Inhalt der HTML-Seite wurde als "template string" gespeichert bzw. als String, der eine Auswertung erlaubt, z.B. wenn er Variablen erhält. Der sich dynamisch ändernde Teil der Homepage: die Anzahl der gespeicherten Notizen (im Quellcode "noteCount"), wird von der aktuellen Anzahl der Notizen (im Quellcode "notes.length") im "template string" ersetzt.

> Writing HTML in the midst of the code is of course not smart, but for old-school PHP-programmers it was a normal practice.

HTML im Quellcode zu schreiben ist natürlich nicht sehr clever, aber für Oldschool-PHP-Programmierer war das völlig normal.

> In traditional web applications the browser is "dumb". It only fetches HTML data from the server, and all application logic is on the server. A server can be created using Java Spring (like in the University of Helsinki course Web-palvelinohjelmointi), Python Flask (like in the course tietokantasovellus) or with Ruby on Rails to name just a few examples.

In traditionellen Webanwendungen ist der Browser "dumm". Er lädt nur die HTML-Daten vom Server und die ganze Anwendungslogik ist auf dem Server. Ein Server kann mit Java Spring (wie im Kurs Web-palvelinohjelmointi der Universität von Helsinki), Python Flask (wie im Kurs tietokantasovellus) oder mit Ruby on Rails erstellt werden, nur um einige Beispiele zu nennen.

> The example uses Express library with the Node.js. This course will use Node.js and Express to create web servers. 

Die Beispielanwendung nutzt Node.js mit der Express-Bibliothek. Dieser Kurs erstellt Webserver mit Node.js und Express.


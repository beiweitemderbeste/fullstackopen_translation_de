> Fundamentals of Web apps

# Grundlagen von Webapplikationen

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











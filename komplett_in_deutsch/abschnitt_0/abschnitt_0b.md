# Abschnitt 0b

## Inhaltsverzeichnis

- [Grundlagen von Webapplikationen](#grundlagen-von-webapplikationen)
- [HTTP GET](#HTTP-GET)
- [Traditionelle Webanwendungen](#Traditionelle-Webanwendungen)
- [Anwendungslogik im Browser laufen lassen](#Anwendungslogik-im-Browser-laufen-lassen)
- [Event Handlers und Callback-Funktionen](#Event-Handlers-und-Callback-Funktionen)
- [Document Object Model bzw. DOM](#Document-Object-Model-bzw.-DOM)
- [Bearbeiten des document objects von der Konsole aus](#Bearbeiten-des-document-objects-von-der-Konsole-aus)
- [CSS](#CSS)
- [Wie eine Seite geladen wird, die JavaScript enthält](#Wie-eine-Seite-geladen-wird,-die-JavaScript-enthält)
- [Formulare und HTTP POST](#Formulare-und-HTTP-POST)
- [AJAX](#AJAX)
- [Single page app](#Single-page-app)


## Grundlagen von Webapplikationen

Bevor wir mit programmieren anfangen, befassen wir uns mit den Grundsätzen der Webentwicklung, indem wir eine Beispielanwendung (https://studies.cs.helsinki.fi/exampleapp) analysieren.

Die Anwendung dient nur zu Demonstrationszwecken einiger Grundkonzepte des Kurses und ist auf keinen Fall ein Beispiel dafür, wie moderne Webanwendungen gebaut werden. Eher im Gegenteil, es werden alte Methoden der Webentwicklung gezeigt, die heutzutage als "bad practices" bezeichnet werden.

Der Quellcode entspricht ab Abschnitt 1 zeitgemäßen "best practices".

Öffnet die Beispielanwendung in eurem Browser. Manchmal dauert das etwas.

Die erste Regel der Webentwicklung: Habt immer die Entwicklerkonsole in euerem Browser offen. Unter Linux könnt ihr die Konsole mit der F12-Taste öffnen. Die Konsole kann auch über das Browsermenü geöffnet werden.

Merkt euch immer die Entwicklerkonsole beim Entwickeln von Webanwendungen geöffnet zu haben.

Die Konsole sieht ungefähr so aus:

!["Ein Screenshot der geöffneten Entwicklertools in einem Browser"](./bilder/abschnitt0b_bild1.png?raw=true)

Hinweis: Der wichtigste Tab ist der "Console"-Tab. Trotzdem werden wir uns in dieser Einleitung auch mit dem "Network"-Tab beschäftigen.

## HTTP GET

Der Server und der Browser kommunizieren über HTTP. Der "Network"-Tab zeigt, wie der Browser und der Server kommunizieren.

Wenn ihr die Seite erneut ladet (durch das Drücken der F5-Taste oder das ↻-Symbol in eurem Browser), wird die Konsole zeigen, das zwei Ereignisse passiert sind:

- Der Browser hat die Inhalte der Webseite vom Server studies.cs.helsinki.fi/exampleapp geladen

- und auch die Bilddatei kuva.png

!["Ein Screenshot der Entwickerkonsole, die die beiden Ereignisse anzeigt"](./bilder/abschnitt0b_bild2.png?raw=true)

Auf kleineren Displays müsst ihr möglicherweise das Konsolenfenster vergrößern, um alles zu sehen.

Wenn ihr auf das erste Ereignis klickt werden mehr Informationen darüber angezeigt, was passiert:

!["Detaillierte Anzeige eines einzelnen Ereignisses"](./bilder/abschnitt0b_bild3.png?raw=true)

Der obere Teil unter "General" zeigt, dass der Browser über die GET-Methode eine Anfrage zur Adresse https://studies.cs.helsinki.fi/exampleapp geschickt hat (Bitte beachtet, dass sich die adresse leicht geändert hat seitdem der Screenshot erstellt wurde). Die Anfrage war erfolgreich, weil die Serverantwort den Statuscode 200 hatte.

Die Anfrage und die Serverantwort haben mehrere Header:

!["vollständiger Inhalt"](./bilder/abschnitt0b_bild4.png?raw=true)

Die oberen Antwortheader zeigen z.B. die Größe der Antwort in Bytes und die genaue Zeit der Antwort an. Der wichtige Header "Content-Type" zeigt an, dass die Antwort eine Textdatei im utf-8-Format ist, deren Inhalt mit HTML formattiert wurde. Auf diese Weise weiß der Browser, dass die Antwort eine reguläre HTML-Seite ist und stellt sie als eine "Webseite" dar.

Der "Response"-Tab zeigt die Antwortdaten, eine reguläre HTML-Seite. Die Body-Sektion definiert die Seitenstruktur, die am Bildschirm angezeigt wird:

!["Ein Screenshot vom Response-Tab"](./bilder/abschnitt0b_bild5.png?raw=true)

Die Seite enthält ein div-Element, das wiederum eine Überschrift enthält, einen Link zu den Seitennotizen und einen img-Tag, ebenso wie die Anzahl der Notizen, die erstellt wurden.

Wegen des img-Tags stellt der Browser eine zweite HTTP-Anfrage, um die Bilddatei kuva.png vom Server zu laden. Die Details der Anfrage sind folgendermaßen:

!["Detaillierte Ansicht des zweiten Ereignisses"](./bilder/abschnitt0b_bild6.png?raw=true)

Die Anfrage (vom Typ HTTP GET) wurde an die Adresse https://studies.cs.helsinki.fi/exampleapp/kuva.png gestellt. Der Antwortheader zeigt uns, dass die Antwort 89350 Bytes groß war und ihr "Content-type" image/png ist. Also ist es eine png-Bilddatei. Der Browser benutzt diese Information, um die Bilddatei korrekt am Bildschirm darzustellen.

So sieht die Ereigniskette beim Öffnen der Seite https://studies.cs.helsinki.fi/exampleapp in einem Browser in einem Sequenzdiagramm aus: 

!["Sequenzdiagramm"](./bilder/abschnitt0b_bild7.png?raw=true)

Zuerst schickt der Browser eine HTTP GET-Anfrage an den Server, um den HTML-Quellcode der Seite zu laden. Der img-Tag in dem HTML-Dokument fordert den Browser auf, die Bilddatei kuva.png zu laden. Der Browser zeigt die HTML-Seite und die Bilddatei auf dem Bildschirm an.

Auch wenn es schwer zu bemerken ist: die Darstellung der HTML-Seite beginnt bevor die Bilddatei vom Server geladen wurde.

## Traditionelle Webanwendungen

Die Homepage der Beispielanwendungen funktioniert wie eine traditionelle Webanwendung. Wenn die Seite aufgerufen wird, lädt der Browser vom Server das HTML-Dokument, das die Struktur und den textlichen Inhalt der Seite beschreibt.

Der Server hat dieses Dokument irgendwie erstellt. Das Dokument kann eine statische Textdatei sein, die im Serververzeichnis abgespeichert wurde. Der Server kann auch dynamisch HTML-Dokumente erstellen, indem er bspw. Daten aus einer Datenbank lädt. Der HTML-Quellcode der Beispielanwendung wurde dynamisch erstellt, weil er die Anzahl der erstellten Notizen enthält.

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

Ihr müsst den Quellcode jetzt noch nicht verstehen können.

Der Inhalt der HTML-Seite wurde als "template string" gespeichert bzw. als String, der eine Auswertung erlaubt, z.B. ob er Variablen enthält. Der sich dynamisch ändernde Teil der Homepage (die Anzahl der gespeicherten Notizen (im Quellcode "noteCount")) wird von der aktuellen Anzahl der Notizen (im Quellcode "notes.length") im "template string" ersetzt.

HTML im Quellcode zu schreiben ist natürlich nicht sehr clever, aber für Oldschool-PHP-Programmierer war das völlig normal.

In traditionellen Webanwendungen ist der Browser "dumm". Er lädt nur die HTML-Daten vom Server und die ganze Anwendungslogik ist auf dem Server. Ein Server kann mit Java Spring (wie im Kurs Web-palvelinohjelmointi der Universität von Helsinki), Python Flask (wie im Kurs tietokantasovellus) oder mit Ruby on Rails erstellt werden, nur um einige Beispiele zu nennen.

Die Beispielanwendung wurde mit Node.js und der Express-Bibliothek erstellt. Diese Technologien werden im Kurs für Webserver eingesetzt.

## Anwendungslogik im Browser laufen lassen

Lasst die Entwicklerkonsole offen. Löscht sie, indem ihr auf das 🚫-Symbol klickt oder clear() in der Konsole eingebt. Wenn ihr jetzt die "notes"-Seite öffnet, stellt der Browser 4 HTTP-Anfragen:

!["Ein Screenshot der Entwicklerkonsole mit 4 sichtbaren Anfragen"](./bilder/abschnitt0b_bild8.png?raw=true)

Jede Anfrage hat einen verschiedenen Typen. Der Typ der ersten Anfrage ist "document". Das ist der HTML-Quellcode der Seite und sieht folgendermaßen aus:

!["Detaillierte Ansicht der ersten Anfrage](./bilder/abschnitt0b_bild9.png?raw=true)

Wenn wir die Seite, die im Browser angezeigt wird, mit dem HTML-Quellcode, den der Server ausgibt, vergleichen, sehen wir, dass der Quellcode keine Liste von Notizen enthält. Die Head-Sektion des HTML-Dokuments enthält einen script-Tag, der dafür sorgt, dass der Browser die Javascript-Datei main.js herunterlädt.

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

Die Details des Quellcodes sind im Moment nicht wichtig, er wurde eingefügt, um die Bilder und den Text interessanter zu gestalten. Das "richtige" Programmieren geht ab Abschnitt 1 los. Der Beispielquellcode in diesem Abschnitt ist nicht wichtig für den weiteren Verlauf dieses Kurs.

Direkt nach dem Laden des script-Tags beginnt der Browser den Quellcode auszuführen.

Die letzten zwei Zeilen weisen den Browser an, eine HTTP GET-Anfrage an die Serveradresse /data.json zu stellen:

```javascript
xhttp.open('GET', '/data.json', true)
xhttp.send()
```

Das ist die Serveranfrage, die im Network-Tab ganz unten steht.

Wir können die Adresse https://studies.cs.helsinki.fi/exampleapp/data.json direkt im Browser aufrufen:

Hier finden wir die Notizen also "rohe" JSON-Daten. Chromium-basierte Browser sind nicht gut im Darstellen von JSON-Daten. Daher solltet ihr Firefox installieren, der das von Haus aus kann.

!["Formattierte JSON-Ausgabe"](./bilder/abschnitt0b_bild11.png?raw=true)

Der Javascriptcode der Notizenseite lädt die JSON-Daten, die die Notizen enthalten und erstellt eine Aufzählungsliste der Notizen:

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

Der Quellcode erstellt zuerst eine ungeordnete Liste mit dem ul-Tag...

```javascript
var ul = document.createElement('ul')
ul.setAttribute('class', 'notes')
```

... und fügt dann einen li-Tag für jede Notiz hinzu. Nur das "content field" der jeweiligen Notiz wird zum Inhalt des li-Tags. Die Zeitstempel, die in den rohen Daten angezeigt werden, werden hier nicht genutzt.

```javascript
data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li)
  li.appendChild(document.createTextNode(note.content))
})
```

Öffnet jetzt den Console-Tab in euer Entwicklerkonsole:

Klickt man auf das kleine Dreieck am Beginn der Zeile, kann man den Text in der Konsole ausklappen.

!["Ein Screenshot der aufgeklappten Einträge"](./images/part0/part0b_image13.png?raw=true)

Diese Ausgabe in der Konsole wird vom "console.log"-Befehl erzeugt:

```javascript
const data = JSON.parse(this.responseText)
console.log(data)
```
Das bedeudet, dass nach dem Erhalt der Daten vom Server, diese in der Konsole angezeigt werden.

Mit dem Console-Tab und dem console-log-Befehl werdet ihr in diesem Kurs noch sehr vertraut werden.

## Event Handlers und Callback-Funktionen

Die Struktur von diesem Quellcode ist ein bisschen ungewöhnlich:

```javascript
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  // code that takes care of the server response
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

Die Serveranfrage wird mit der letzten Zeile abgeschickt, aber der Quellcode, der die Antwort abarbeitet steht weiter oben. Was passiert hier?

```javascript
xhttp.onreadystatechange = function () {
```

In dieser Zeile wird ein Event Handler für das Event onreadystatechange für das xhttp-Object definiert, das die Anfrage startet. When sich der Zustand des Objektes verändert, dann ruft der Browser die Event Handler-Funktion auf. Der Quellcode der Funktion überprüft, ob der readyState gleich 4 ist (was bedeutet, das die Operation vollständig durchgeführt wurde) und ob der HTTP Statuscode der Antwort 200 ist.

```javascript
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    // code that takes care of the server response
  }
}
```

Event Handler aufzurufen ist sehr gebräuchlich in der Javascriptentwicklung. Event Handler-Funktionen werden Callback-Funktionen gennant. Der Anwendungsquellcode ruft die Funktionen nicht selbst auf, sondern die Laufzeitumgebung - der Browser selbst - ruft die Funktion zur passenden Zeit, nämlich wenn das Ereignis eintritt, auf.

## Document Object Model bzw. DOM

Wir können uns HTML-Seiten als vorstellen.

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

Die gleiche baumartige Struktur kann man im Konsolentab "Elements" sehen.

Die Funktionstüchtigkeit des Browser basiert auf der Idee HTML-Elemente als Baum darzustellen.

Das Document Object Model, oder kurz DOM, ist eine API (Application Programming Interface), die die Veränderung der Elemente von Webseiten ermöglicht.

Der Javascriptquellcode, der im vorangegangen Kapitel vorgestellt wurde, nutzt die DOM-API, um eine Liste der Notizen auf der Webseite darzustellen.

Der folgende Quellcode erstellt einen Knoten für die Variable ul und fügt einige untergeordnete Knoten hinzu:

```javascript
var ul = document.createElement('ul')

data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li)
  li.appendChild(document.createTextNode(note.content))
})
```

Schließlich wird in der HTML-Struktur der ganzen Seite, der Teil der ul-Variable eingefügt:

```javascript
document.getElementsByClassName('notes').appendChild(ul)
```

## Bearbeiten des document objects von der Konsole aus

Der oberste Knoten des DOM-Baumes eines HTML-Dokuments wird document object genannt. Wir können eine Webseite mit der DOM-API auf verschiedene Weise bearbeiten. Auf das document object könnt ihr direkt in der Konsole zugreifen, indem ihr document eingebt:

!["Ein Screenshot des Elements-Tab der Entwicklerkonsole"](./bilder/abschnitt0b_bild15.png?raw=true)

Jetzt fügen wir über die Konsole eine neue Notiz in die Webseite ein.

```javascript
list = document.getElementsByTagName('ul')[0]
```

Dann erstellen wir ein neues li-Element und ergänzen es mit ein wenig Text:

```javascript
newElement = document.createElement('li')
newElement.textContent = 'Page manipulation from console is easy'

```

Und dann hängen wir das li-Element an die Liste:

```javascript
list.appendChild(newElement)
```

!["Ein Screenshot der Seite mit der neuen Notiz in der Liste"](./bilder/abschnitt0b_bild16.png?raw=true)

Auch wenn die Seite in eurem Browser verändert wird, sind die Änderungen nicht dauerhaft. Wenn die Seite neugeladen wird, verschwindet die Notiz, da die Änderungen nicht an den Server übertragen wurden. Der JavaScriptquellcode, der vom Browser geladen wird, erstellt immer eine Liste mit den Notizen aus den JSON-Daten, die von der Adresse https://studies.cs.helsinki.fi/exampleapp/data.json geladen werden.

## CSS

Das Head-Element des HTML-Quellcode enthält einen Link-Tag, der festlegt, dass der Browser die CSS-Datei main.css laden soll.

Cascading Style Sheets, oder kurz CSS, ist eine Style Sheet-Sprache, die genutzt wird, um das Aussehen von Webseiten festzulegen.

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

Die Datei definiert 2 Class-Selectors. Diese werden benutzt, um bestimmte Teile der Seite auszuwählen und Regeln für deren Aussehen zu bestimmen.

Eine Class-Selector-Definition beginnt immer mit einem Punkt und enthält den Namen der Klasse.

Klassen sind Attribute, die HTML-Elementen hinzugefügt werden können.

CSS-Attribute können im Elements-Tab der Konsole betrachtet werden:

!["Ein Screenshot des Elements-Tab der Entwicklerkonsole"](./bilder/abschnitt0b_bild17.png?raw=true)

Das äußerste Div-Element hat die Klasse "container". Das ul-Element, das die Notizenliste enthält, hat die Klasse "notes".

Die CSS-Regel bestimmt, dass Elemente mit der Klasse "container" eine 1 Pixel-breite Umrandung haben. Die Klasse legt auch einen Padding-Wert von 10 Pixeln fest. Dadurch gibt es einen freien Bereich zwischen den Elementen und deren Umrandung.

Die zweite CSS-Regel ändert die Textfarbe der Notizen zu blau.

HTML-Elemente können auch andere Attribute als Klassen haben. Das Div-Element, dass die Notizen enthält, hat auch ein id-Attribut. Der Javascriptquellcode nutzt diese id, um das Element zu finden.

Der Elements-Tab der Konsole kann dazu benutzt werden, das Aussehen der verschiedenen Elemente zu ändern.

!["fullstack content"](./bilder/abschnitt0b_bild18.png?raw=true)

Veränderungen der Seite, die in der Konsole durchgeführt werden, sind nicht dauerhaft. Wenn ihr dauerhafte Änderungen durchführen wollt, muss die CSS-Datei auf dem Server gespeichert werden.

## Wie eine Seite geladen wird, die JavaScript enthält

Schauen wir nach, was passiert, wenn die Seite https://studies.cs.helsinki.fi/exampleapp/notes im Browser geöffnet wird.

!["fullstack content"](./bilder/abschnitt0b_bild19.png?raw=true)

Über eine HTTP GET-Anfrage lädt der Browser den HTML-Quellcode, der den Inhalt und die Struktur der Seite bestimmt.

- Die Links im HTML-Quellcode veranlassen den Browser die CSS-Datei main.css zu laden...

- ... ebenso wie die JavaScript-Datei main.js

- Der Browser für den JavaScript-Quellcode aus. Dadurch wird eine HTTP GET-Anfrage an die Adresse https://studies.cs.helsinki.fi/exampleapp/data.json geschickt, woraufhin die Notizen als JSON-Daten zurückkommen.

- Wenn die Daten geladen wurden, führt der Browser einen Event Handler aus, der Notizen auf der Seite anzeigt, indem er die DOM-API nutzt.

## Formulare und HTTP POST

Als Nächstes untersuchen wir, wie das Hinzufügen einer neuen Notiz funktioniert.

Die Notizenseite enthält ein form-Element.

!["fullstack content"](./bilder/abschnitt0b_bild20.png?raw=true)

Wenn der Button des Formulars geklickt wird, sendet der Browser die Benutzereingabe zum Server. Öffnen wir den Network-Tab und sehen uns an, wie das Absenden des Formulars aussieht:

!["fullstack content"](./bilder/abschnitt0b_bild21.png?raw=true)

Überraschenderweise löst das Abschicken des Formulars nicht weniger als fünf HTTP-Anfragen aus. Die Erste ist das Abschicken selbst. Schauen wir genauer hin:

!["fullstack content"](./bilder/abschnitt0b_bild22.png?raw=true)

Wir sehen eine HTTP POST-Anfrage an die Serveradresse new_note. Der Server antwortet mit dem HTTP-Statuscode 302. Das ist eine URL-Umleitung mit der der Server den Browser auffordert, eine neue HTTP-Anfrage zu starten, deren Adresse im Header definiert ist.

Dadurch lädt der Server die Notizenseite erneut. Dieses Neuladen verursacht drei weitere HTTP-Anfragen: das Laden der CSS-Datei (main.css), die JavaScript-Datei (main.js), und die Rohdaten der Notizen (data.json).

Der Network-Tab zeigt auch die Daten, die mit dem Formular abgeschickt werden.

!["fullstack content"](./bilder/abschnitt0b_bild23.png?raw=true)

Der Form-Tag hat die Attribute "action" und "method", die festlegen, dass das Abschicken des Formlars als HTTP POST-Anfrage an die Adresse new_note geschieht.

!["fullstack content"](./bilder/abschnitt0b_bild24.png?raw=true)

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
Die über das Formular abgeschickten Daten, werden im "body" der HTTP POST-Anfrage verschickt.

Der Server kann auf diese Daten zu greifen, in dem er das Feld "req.body" des Request Objects "req" einsieht.


Der Server erstellt ein neues Notizobjekt und hängt es an das Array "notes":

```javascript
notes.push({
  content: req.body.note,
  date: new Date(),
})
```

Das Notizobjekt hat zwei Felder: "content" enthält den tatsächlichen Inhalt der Notiz und "date" enthält das Datum und die Uhrzeit, an dem die Notiz erstellt wurde.

Der Server speichert keine neuen Notizen, so dass die Notizen verschwinden, wenn der Server neugestartet wird.

## AJAX

Die Notizenseite folgt einem Webentwicklungstrend aus den frühen 90ern und nutzt "Ajax". Als solches hatte es seine Hochzeit in den frühen 00er Jahren.

AJAX (Asynchronous JavaScript and XML) ist ein Begriff, der im Februar 2005 eingeführt wurde und einen revolutionären Ansatz beschreibt, wie Inhalt von Webseiten geladen werden ohne die Seite erneut zu laden.

Vor der AJAX-Zeit funktionierten alle Webseiten wie unsere traditionelle Webanwendung, die wir schon eher in diesem Kapitel gesehen haben. Alle Daten, die auf der Webseite dargestellt werden

Die Notizenseite lädt die Daten der Notizen über AJAX. Das Abschicken des Formulars geschieht immer noch über traditionelle Techniken.

Die URLS der Anwendung spiegeln die alten sorgenfreien Zeiten wieder. Die JSON-Daten werden von der URL https://studies.cs.helsinki.fi/exampleapp/data.json geladen und neue Notizen werden an die URL https://studies.cs.helsinki.fi/exampleapp/new_note gesendet. Heutzutage werden solche URLS nicht mehr akzeptiert, da sie nicht den anerkannten Konventionen von RESTful APIs (mehr davon in Abschnitt3) folgen.

AJAX ist heute so verbreitet, dass es als selbstverständlich angenommen wird. Der Begriff ist in Vergessenheit geraten und die neue Generation hat noch nicht mal davon gehört.

## Single page app




[Abschnitt 1](../abschnitt_1/abschnitt_1.md)
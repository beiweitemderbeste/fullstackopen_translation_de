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

In unserer Beispielapp funktioniert die Homepage wie eine traditionelle Webseite: Die komplette Logik liegt auf dem Server, der Browser ist nur für die HTML-Darstellung zuständig.

Die Notizenseite gibt einen Teil der Arbeitan den Brower ab: das Generieren des HTML-Codes für die Notizen.

Seit ein paar Jahren gibt es einen Trend, dass man Webanwendungen als SPAs (single page apps) erstellt.

Die Notizenseite hat ein wenig Ähnlichkeit mit SPAs, aber nicht wirklich viel. Auch wenn die Logik für das Darstellen der Notizen im Browser läuft, werden neue Notizen noch über traditionelle Wege erstellt. Die Daten werden über das Abschicken des Formulars an den Server geschickt und der Server weist den Browser über eine Umleitung an, die Notizenseite neuzuladen.

Eine Single page app-Version unserer Beispielanwendung findet man unter https://studies.cs.helsinki.fi/exampleapp/spa. Auf den ersten Blick sieht die Anwendung genauso aus, wie die andere. Der HTML-Code ist fast identisch, aber die JavaScript-Datei ist eine andere (spa.js) und es gibt eine kleine Änderung, wie der form-Tag definiert wird:

!["fullstack content"](./bilder/abschnitt0b_bild25.png?raw=true)

Das Formular hat keine action- oder method-Attribut, die definieren wie und wohin die Formulardaten geschickt werden sollen.

Öffnet den Network-Tab und löscht ihn. Wenn ihr jetzt eine neue Notiz erstellt, werdet ihr bemerken, dass der Browser nur eine Anfrage an den Server schickt:

!["fullstack content"](./bilder/abschnitt0b_bild26.png?raw=true)

Die POST-Anfrage an die Adresse new_note_spa schickt die Notiz im JSON-Format, in dem der Inhalt der Notiz (content) und der Zeitstempel (date) enthalten sind.

```json
{
  content: "single page app does not reload the whole page",
  date: "2019-05-25T15:15:59.905Z"
}
```

Der "Content-Type-Header" der Anfrage sagt dem Server, dass die mitgeschickten Daten im JSON-Format sind.

!["fullstack content"](./bilder/abschnitt0b_bild27.png?raw=true)

Ohne diesen Header würde der Server nicht genau wissen, wie er die mitgeschickten Daten interpretieren soll.

Der Server antwortet mit dem Statuscode "201 created". Dieses Mal fragt der Server nicht nach einer Weiterleitung, der Browser bleibt auf der selben Seite und stellt keine weiteren HTTP-Anfragen.

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

Der Befehl "document.getElementById('notes_form')" ruft das Formular der Seite auf und hängt einen Event Handler an das Formular, so dass das Abschicken des Formulars von diesem erledigt wird. Der Event Handler ruft sofort die Methode e.preventDefault() auf, um das voreingestellte Abschicken des Formulars zu unterbinden. Das voreingestellte Abschicken würde Daten an den Server sichken und eine GET-Anfrage auslösen, was wir verhindern wollen.

Dann erstellt der Event Handler eine neue Notiz, fügt sie mit dem Befehl notes.push(note) an die Notizenliste, zeigt die neue Notizenlist auf der Seite an und sendet die neue Noitz an den Server.

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

Dieser Quellcode definiert, dass die Daten per HTTP POST-Anfrage geschickt werden und dass der Datentyp JSON ist. Der Datentyp wird durch den Content-Type-Header bestimmt. Dann werden alle Daten als JSON-String verschickt.

Der Quellcode der Anwendung ist unter https://github.com/mluukkai/example_app verfügbar. Denkt dran, dass dieser nur die Konzepte des Kurses demonstrieren soll. In gewissen Maße folgt er einem schlechten Programmier-/Entwicklungsstil und sollte auf keinen Fall als Vorbild dafür dienen, wie ihr Anwendungen baut. Das gleiche gilt für die URLS, die hier benutzt werden. Die URL new_note_spa, an die neuen Notizen geschickt werden, hält sich nicht an aktuelle "best practices".

## Javascript-libraries

Die Beispielanwendung ist in sogenanntem Vanilla JavaScript geschrieben und nutzt nur dieses und die DOM-API, um die Strukturen der Seiten zu verändern.

Anstatt nur reinem JavaScript und DOM-API, kann man auch verschiedene Bibliotheken nutzen, mit denen sich viel leichter Webseiten verändern lassen. Eine dieser Bilbiotheken ist jQuery.

jQuery wurde entwickelt, als Webapplikationen noch hauptsächlich aus HTML-Seiten bestanden, die von Server generiert wurden. Diese Funktionalität wurde durch, auf Browserseite genutztem, JavaScript und jQuery erweitert. Eine der Gründe für den Erfolg von jQuery war seine Kompabilität über mehrere Browser. Die Bibliothek arbeitete unabhängig vom eingesetzten Browser oder der Firma, die ihn erstellt hat. Dadurch waren keine browserspezifischen Lösungen mehr nötig. Heutzutage ist wegen der Weiterentwicklung von JavaScript der Einsatz von jQuery nicht mehr gerechtfertigt und die meisten Browser unterstützen sowieso die meisten Grundfunktionien.

Der Aufstieg der SPAs brauchte auch mehrere verschiedene "moderne" Wege zur Webentwicklung zum Vorschein als jQuery. Der Liebling der ersten Welle von Entwicklern war BackboneJS. Nach seiner Einführung 2012 wurde Googles AngularJS schnell zum Quasistandard der Webentwicklung.

Wie auch immer, die Beliebtheit von Angular stürzte im Oktober 2014 ab, als das Angularteam bekannt gab den Support für Version 1 einzustellen und das Angular 2 nicht rückwärtskompatibel sein wird. Angular 2 und neuere Versionen erreichten nie die Beliebtheit von Version 1.

Das momentan beliebteste Werkzeug für das Implementieren von Anwendungslogik auf Browserseite ist Facebooks React-Bibliothek. In diesem Kurs werden wir uns mit der React- und Redux-Bibliothek vertraut machen, die sehr häufig zusammen genutzt werden.

Der Beliebtheitsgrad von React ist sehr groß, aber die Welt von JavaScript ändert sich ständig. Es gibt z.B. zur Zeit einen Newcomer - VueJs - der für einigies Interesse sorgt.

## Full stack web development

Was bedeutet eigentlich Full Stack Webentwicklung? Full Stack ist ein Modewort, über das jeder redet, aber keiner so wirklich weiß, was es bedeutet. Oder zumindest gibt es keine allgemein gültige Definition für diesen Begriff.

Praktisch gesehen habe alle Webanwendung (mindestens) 2 "Schichten": den Browser (die obere Schicht), der näher am Endbenutzer ist und den Server (die untere Schicht). Es gibt oft noch eine Datenbankschicht unter der Serverschicht. Wir können uns daher die Architektur von Webanwendungen als einen Stapel (Stack) verschiedener Schichten vorstellen.

Oft wird auch vom Frontend und Backend gesprochen. Der Browser ist das Frontend und JavaScript, das im Browser läuft, ist Fontendcode. Der Server auf der anderen Seite, ist Backend.

Im Zusamenhang mit diesem Kurses bedeutet Full Stack Webentwicklung das wir uns auf alle teile einer Anwendung konzentrieren: das Frontend, das Backend und die Datenbank. Manchmal wird die Software auf dem Server und sein Betriebssystem als Teile des Stacks bezeichnet, aber damit werden wir uns nicht beschäftigen.

Wir werden das Backend mit Javascript schreiben, in dem wir die Node.js-Entwicklungsumgebung nutzen. Die gleiche Programmiersprache in den verschiedenen Schichten des Stacks nutzen gibt der Webentwicklung eine ganz andere Dimension. Wie auch immer ist es keine Vorraussetzung für Full Stack Entwicklung die gleiche Sprache (JavaScript) für alle Schichten des Stacks zu benutzen.

Es war üblicher, dass sich Entwickler auf eine Schicht des Stacks konzentriert haben, z.B. auf das Backend. Die Technologien auf dem Backend und dem Frontend ware sehr verschieden. Mit dem Full Stack-Trend ist es jetzt verbreiteter, dass sich Entwickler auf allen Schichten der Anwendung auskennen, ebenso wie der Datenbank. Oft müssen Full Stack-Entwickler auch genug Konfigurations- und Administrationsfähigkeiten besizen, um ihre Anwendungen z.B. in der Cloud laufen zu lassen.

## JavaScript fatigue

Die Full Stack-Webentwicklung ist auf vielen Seiten herausfordernd. Sachen passieren an vielen Orten gleichzeitig, debuggen ist schwieriger im Vergleich zu regulären Desktopanwendungen. JavaScript funktioniert auch nicht immer, wie man erwartet (im Vergleich zu vielen anderen Sprachen), und die asynchrone Weise, auf die seine Entwicklungsumgebung arbeitet, verursacht alle Arten von Herausforderungen. Um mit dem Server zu kommunizieren, benötigt es Wissen über HTTP. Außerdem muss man mit Datenbanken umgehen können und Server verwalten und konfigurieren können. Es wäre auch gut zu wissen, wie man CSS einsetzt, um die Anwendungen präsentieren zu können.

Die Welt von JavaScript ändert sich schnell, was zu einer eigenen Reihe von Herausforderungen führt. Werkzeuge, Bibliothken und die Sprache selber sind unter konstanter Entwicklung. Manche werden langsam müde von der dauerhaften Veränderung und haben dafür ein Wort gefunden: JavaScript-Müdigkeit. Wie ihr damit umzugehen lernt, könnt ihr [hier](https://auth0.com/blog/how-to-manage-javascript-fatigue/) oder [hier](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4) nachlesen.

Auch ihr werden während des Kurses an JavaScript-Müdigkeit leiden. Glücklicherweise gibt es ein paar Arten, wie man die Lernkurve abschwächen kann und wir beginnen damit, indem wir programmieren anstatt zu konfigurieren. Wir können das Konfigurieren zwar nicht ganz verhindern, aber wir können es um ein paar Wochen verschieben.

## Exercises

Die Aufgaben werden über GitHub eingereicht und in dem sie als erledigt im Submissionsystem markiert werden.

Ihr könnt alle Aufgaben über ein Repository einreichen oder verschiedene Repositories benutzen. Bitte benennt eure Verzeichnisse korrekt, wenn ihr Aufgaben verschiedener Abschnitte im selben Repository einreicht. Wenn ihr ein privates Repository mit Aufgaben einreichen wollt, fügt mluukkai Collaborator hinzu.

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

Dadurch hat jeder Abschnitt sein eigenes Verzeichnis, dass jeweils ein Verzeichnis für jede Aufgabenzusammenstellung enthält (z.B. die unicafe-Aufgaben in Abschnitt 1).

### 0.1: HTML

Schaut euch nochmal die Grundlagen von HTML an, in dem ihr diese Anleitung von Mozilla lest: [HTML Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)

Bei dieser Aufgabe müsst ihr nichts abgeben, nur den Link lesen.

### 0.2: CSS

Schaut euch nochmal die Grundlagen von HTML an, in dem ihr diese Anleitung von Mozilla lest: [CSS Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics)

Bei dieser Aufgabe müsst ihr nichts abgeben, nur den Link lesen.

### 0.3: HTML forms

Lernt über die Grundlagen von HTML-Formularen, indem ihr diese Anleitung von Mozilla lest: [Your first form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form)

Im Abschnitt "Wie eine Seite geladen wird, die Javascript enthält" wird die Abfolge der Ereignisse beim Öfnnen der Seite https://studies.cs.helsinki.fi/exampleapp/notes als Sequenzdiagramm dargestellt.

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

Erstellt ein ähnliches Diagramm, dass die Situation beschreibt, wo ein Benutzer eine neue Notiz auf der Seite https://studies.cs.helsinki.fi/exampleapp/notes erstellt, indem ihr etwas ins Textfeld schreibt und auf den Abschickenbutton klickt.

Bei Bedarf könnt ihr die Operationen im Browser oder auf dem Server als Kommentare im Diagramm zeigen.

Das Diagramm muss kein Sequenzdiagramm sein. Jede vernünfte Art, wie die Ereignisse dargestellt werden, ist in Ordnung.

Alle benötigten Informationen für diese und die nächsten beiden Aufgaben findet ihr im Text dieses Abschnitts. Die Intention dieser Aufgaben ist es, erneut den Text zu lesen und darüber zu denken, was hier passiert. Den Quellcode der Anwendung zu lesen ist dafür nicht nötig, aber natürlich möglich.

Hinweis: Der wahrscheinlich beste Weg um Diagramme zu erstellen (die Mermaid syntax) ist jetzt auch auf Github möglich.

### 0.5: Single page app

Erstellt ein Diagramm, dass die Situation beschreibt, wie ein Benutzer die Single page app-Version der Notizenseite unter https://studies.cs.helsinki.fi/exampleapp/spa öffnet.

### 0.6: New note

Erstellt ein Diagramm, das die Situation beschreibt, wie ein Benutzer eine neue Notiz auf der SPA-Version der App erstellt.

[Abschnitt 1](../abschnitt_1/abschnitt_1.md)
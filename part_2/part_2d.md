# part 2b

> table of contents

## Inhaltsverzeichnis

- [REST](#REST)
- [Sending Data to the Server](#Sending-Data-to-the-Server)
- [Changing the Importance of Notes](#Changing-the-Importance-of-Notes)
- [Extracting Communication with the Backend into a Separate Module](#Extracting-Communication-with-the-Backend-into-a-Separate-Module)
- [Cleaner Syntax for Defining Object Literals](#Cleaner-Syntax-for-Defining-Object-Literals)
- [Promises and Errors](#Promises-and-Errors)
- [exercises](#exercises)

> When creating notes in our application, we would naturally want to store them in some backend server. The json-server package claims to be a so-called REST or RESTful API in its documentation:

> - Get a full fake REST API with zero coding in less than 30 seconds (seriously)

> The json-server does not exactly match the description provided by the textbook definition of a REST API, but neither do most other APIs claiming to be RESTful.

> We will take a closer look at REST in the next part of the course. But it's important to familiarize ourselves at this point with some of the conventions used by json-server and REST APIs in general. In particular, we will be taking a look at the conventional use of routes, aka URLs and HTTP request types, in REST.

## REST

> In REST terminology, we refer to individual data objects, such as the notes in our application, as resources. Every resource has a unique address associated with it - its URL. According to a general convention used by json-server, we would be able to locate an individual note at the resource URL notes/3, where 3 is the id of the resource. The notes url, on the other hand, would point to a resource collection containing all the notes.

> Resources are fetched from the server with HTTP GET requests. For instance, an HTTP GET request to the URL notes/3 will return the note that has the id number 3. An HTTP GET request to the notes URL would return a list of all notes.

> Creating a new resource for storing a note is done by making an HTTP POST request to the notes URL according to the REST convention that the json-server adheres to. The data for the new note resource is sent in the body of the request.

> json-server requires all data to be sent in JSON format. What this means in practice is that the data must be a correctly formatted string, and that the request must contain the Content-Type request header with the value application/json.

## Sending Data to the Server

> Let's make the following changes to the event handler responsible for creating a new note:

```javascript
addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    date: new Date(),
    important: Math.random() < 0.5,
  }

  axios    
    .post('http://localhost:3001/notes', noteObject)    
    .then(response => {      
      console.log(response)    
    })
}
```

> We create a new object for the note but omit the id property, since it's better to let the server generate ids for our resources!

> The object is sent to the server using the axios post method. The registered event handler logs the response that is sent back from the server to the console.

> When we try to create a new note, the following output pops up in the console:

!["fullstack content"](./images/part2d_image1.png?raw=true)

> The newly created note resource is stored in the value of the data property of the response object.

> Sometimes it can be useful to inspect HTTP requests in the Network tab of Chrome developer tools, which was used heavily at the beginning of part 0:

!["fullstack content"](./images/part2d_image2.png?raw=true)

> We can use the inspector to check that the headers sent in the POST request are what we expected them to be, and that their values are correct.

> Since the data we sent in the POST request was a JavaScript object, axios automatically knew to set the appropriate application/json value for the Content-Type header.

> The new note is not rendered to the screen yet. This is because we did not update the state of the App component when we created the new note. Let's fix this:

```javascript
addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    date: new Date(),
    important: Math.random() > 0.5,
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then(response => {
      setNotes(notes.concat(response.data))      setNewNote('')    
    })
}
```

> The new note returned by the backend server is added to the list of notes in our application's state in the customary way of using the setNotes function and then resetting the note creation form. An important detail to remember is that the concat method does not change the component's original state, but instead creates a new copy of the list.

> Once the data returned by the server starts to have an effect on the behavior of our web applications, we are immediately faced with a whole new set of challenges arising from, for instance, the asynchronicity of communication. This necessitates new debugging strategies, console logging and other means of debugging become increasingly more important. We must also develop a sufficient understanding of the principles of both the JavaScript runtime and React components. Guessing won't be enough.

> It's beneficial to inspect the state of the backend server, e.g. through the browser:

!["fullstack content"](./images/part2d_image3.png?raw=true)

> This makes it possible to verify that all the data we intended to send was actually received by the server.

> In the next part of the course we will learn to implement our own logic in the backend. We will then take a closer look at tools like Postman that helps us to debug our server applications. However, inspecting the state of the json-server through the browser is sufficient for our current needs.

> NB: In the current version of our application, the browser adds the creation date property to the note. Since the clock of the machine running the browser can be wrongly configured, it's much wiser to let the backend server generate this timestamp for us. This is in fact what we will do in the next part of the course.

> The code for the current state of our application can be found in the part2-5 branch on GitHub.

## Changing the Importance of Notes

> Let's add a button to every note that can be used for toggling its importance.

> We make the following changes to the Note component:

```javascript
const Note = ({ note, toggleImportance }) => {
  const label = note.important
    ? 'make not important' : 'make important'

  return (
    <li>
      {note.content} 
      <button onClick={toggleImportance}>{label}</button>
    </li>
  )
}
```

> We add a button to the component and assign its event handler as the toggleImportance function passed in the component's props.

> The App component defines an initial version of the toggleImportanceOf event handler function and passes it to every Note component:


```javascript
const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  // ...

  const toggleImportanceOf = (id) => {    
    console.log('importance of ' + id + ' needs to be toggled')  
  }
  // ...

  return (
    <div>
      <h1>Notes</h1>
      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all' }
        </button>
      </div>      
      <ul>
        {notesToShow.map(note => 
          <Note
            key={note.id}
            note={note} 
            toggleImportance={() => toggleImportanceOf(note.id)}          
          />
        )}
      </ul>
      // ...
    </div>
  )
}
```

> Notice how every note receives its own unique event handler function, since the id of every note is unique.

> E.g., if note.id is 3, the event handler function returned by toggleImportance(note.id) will be:

```javascript
() => { console.log('importance of 3 needs to be toggled') }
```

> A short reminder here. The string printed by the event handler is defined in Java-like manner by adding the strings:

```javascript
console.log('importance of ' + id + ' needs to be toggled')
```

> The template string syntax added in ES6 can be used to write similar strings in a much nicer way:

```javascript
console.log(`importance of ${id} needs to be toggled`)
```

> We can now use the "dollar-bracket"-syntax to add parts to the string that will evaluate JavaScript expressions, e.g. the value of a variable. Note that we use backticks in template strings instead of quotation marks used in regular JavaScript strings.

> Individual notes stored in the json-server backend can be modified in two different ways by making HTTP requests to the note's unique URL. We can either replace the entire note with an HTTP PUT request, or only change some of the note's properties with an HTTP PATCH request.

> The final form of the event handler function is the following:

```javascript
const toggleImportanceOf = id => {
  const url = `http://localhost:3001/notes/${id}`
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  axios.put(url, changedNote).then(response => {
    setNotes(notes.map(n => n.id !== id ? n : response.data))
  })
}
```

> Almost every line of code in the function body contains important details. The first line defines the unique url for each note resource based on its id.

> The array find method is used to find the note we want to modify, and we then assign it to the note variable.

> After this we create a new object that is an exact copy of the old note, apart from the important property.

> The code for creating the new object that uses the object spread syntax may seem a bit strange at first:

```javascript
const changedNote = { ...note, important: !note.important }
```

> In practice, { ...note } creates a new object with copies of all the properties from the note object. When we add properties inside the curly braces after the spread object, e.g. { ...note, important: true }, then the value of the important property of the new object will be true. In our example the important property gets the negation of its previous value in the original object.

> There's a few things to point out. Why did we make a copy of the note object we wanted to modify, when the following code also appears to work?

```javascript
const note = notes.find(n => n.id === id)
note.important = !note.important

axios.put(url, note).then(response => {
  // ...
```

> This is not recommended because the variable note is a reference to an item in the notes array in the component's state, and as we recall we must never mutate state directly in React.

> It's also worth noting that the new object changedNote is only a so-called shallow copy, meaning that the values of the new object are the same as the values of the old object. If the values of the old object were objects themselves, then the copied values in the new object would reference the same objects that were in the old object.

> The new note is then sent with a PUT request to the backend where it will replace the old object.

> The callback function sets the component's notes state to a new array that contains all the items from the previous notes array, except for the old note which is replaced by the updated version of it returned by the server:

```javascript
axios.put(url, changedNote).then(response => {
  setNotes(notes.map(note => note.id !== id ? note : response.data))
})
```

> This is accomplished with the map method:

```javascript
notes.map(note => note.id !== id ? note : response.data)
```

> The map method creates a new array by mapping every item from the old array into an item in the new array. In our example, the new array is created conditionally so that if note.id !== id is true; we simply copy the item from the old array into the new array. If the condition is false, then the note object returned by the server is added to the array instead.

> This map trick may seem a bit strange at first, but it's worth spending some time wrapping your head around it. We will be using this method many times throughout the course.

## Extracting Communication with the Backend into a Separate Module

> The App component has become somewhat bloated after adding the code for communicating with the backend server. In the spirit of the single responsibility principle, we deem it wise to extract this communication into its own module.

> Let's create a src/services directory and add a file there called notes.js:

```javascript
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  return axios.get(baseUrl)
}

const create = newObject => {
  return axios.post(baseUrl, newObject)
}

const update = (id, newObject) => {
  return axios.put(`${baseUrl}/${id}`, newObject)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

> The module returns an object that has three functions (getAll, create, and update) as its properties that deal with notes. The functions directly return the promises returned by the axios methods.

> The App component uses import to get access to the module:

```javascript
import noteService from './services/notes'

const App = () => {
```

> The functions of the module can be used directly with the imported variable noteService as follows:

```javascript
const App = () => {
  // ...

  useEffect(() => {
    noteService      
      .getAll()      
      .then(response => {        
        setNotes(response.data)      
      })  
  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService      
      .update(id, changedNote)      
      .then(response => {        
        setNotes(notes.map(note => note.id !== id ? note : response.data))      
      })  
  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      date: new Date().toISOString(),
      important: Math.random() > 0.5
    }

    noteService      
      .create(noteObject)      
      .then(response => {        
        setNotes(notes.concat(response.data))        setNewNote('')      
      })  
  }

  // ...
}

export default App
```

> We could take our implementation a step further. When the App component uses the functions, it receives an object that contains the entire response for the HTTP request:

```javascript
noteService
  .getAll()
  .then(response => {
    setNotes(response.data)
  })
```

> The App component only uses the response.data property of the response object.

> The module would be much nicer to use if, instead of the entire HTTP response, we would only get the response data. Using the module would then look like this:

```javascript
noteService
  .getAll()
  .then(initialNotes => {
    setNotes(initialNotes)
  })
```

> We can achieve this by changing the code in the module as follows (the current code contains some copy-paste, but we will tolerate that for now):

```javascript
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = newObject => {
  const request = axios.post(baseUrl, newObject)
  return request.then(response => response.data)
}

const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

> We no longer return the promise returned by axios directly. Instead, we assign the promise to the request variable and call its then method:

```javascript
const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}
```

> The last row in our function is simply a more compact expression of the same code as shown below:

```javascript
const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => {    
    return response.data  
  })
}
```

> The modified getAll function still returns a promise, as the then method of a promise also returns a promise.

> After defining the parameter of the then method to directly return response.data, we have gotten the getAll function to work like we wanted it to. When the HTTP request is successful, the promise returns the data sent back in the response from the backend.

> We have to update the App component to work with the changes made to our module. We have to fix the callback functions given as parameters to the noteService object's methods, so that they use the directly returned response data:

```javascript
const App = () => {
  // ...

  useEffect(() => {
    noteService
      .getAll()
      .then(initialNotes => {        
        setNotes(initialNotes)      
      })
  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService
      .update(id, changedNote)
      .then(returnedNote => {        
        setNotes(notes.map(note => note.id !== id ? note : returnedNote))      
      })
  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      date: new Date().toISOString(),
      important: Math.random() > 0.5
    }

    noteService
      .create(noteObject)
      .then(returnedNote => {        
        setNotes(notes.concat(returnedNote))        setNewNote('')
      })
  }

  // ...
}
```

> This is all quite complicated and attempting to explain it may just make it harder to understand. The internet is full of material discussing the topic, such as this one.

> The "Async and performance" book from the You do not know JS book series explains the topic well, but the explanation is many pages long.

> Promises are central to modern JavaScript development and it is highly recommended to invest a reasonable amount of time into understanding them.

## Cleaner Syntax for Defining Object Literals
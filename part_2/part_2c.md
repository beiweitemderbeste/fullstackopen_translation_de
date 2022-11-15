# part 2c

> table of contents

## Inhaltsverzeichnis

- [the browser as a runtime environment](#the-browser-as-a-runtime-environment)
- [npm](#npm)
- [axios and promises](#axios-and-promises)
- [effect-hooks](#effect-hooks)
- [the development runtime environment](#the-development-runtime-environment)
- [exercises](#exercises)

> For a while now we have only been working on "frontend", i.e. client-side (browser) functionality. We will begin working on "backend", i.e. server-side functionality in the third part of this course. Nonetheless, we will now take a step in that direction by familiarizing ourselves with how code executing in the browser communicates with the backend.

> Let's use a tool meant to be used during software development called JSON Server to act as our server.

> Create a file named db.json in the root directory of the previous notes project with the following content:

```javascript
{
  "notes": [
    {
      "id": 1,
      "content": "HTML is easy",
      "date": "2022-1-17T17:30:31.098Z",
      "important": true
    },
    {
      "id": 2,
      "content": "Browser can execute only JavaScript",
      "date": "2022-1-17T18:39:34.091Z",
      "important": false
    },
    {
      "id": 3,
      "content": "GET and POST are the most important methods of HTTP protocol",
      "date": "2022-1-17T19:20:14.298Z",
      "important": true
    }
  ]
}
```

> You can install JSON server globally on your machine using the command npm install -g json-server. A global installation requires administrative privileges, which means that it is not possible on faculty computers or freshman laptops.

> After installing run the following command to run the json-server. The json-server starts running on port 3000 by default; but since projects created using create-react-app reserve port 3000, we must define an alternate port, such as port 3001, for the json-server. The --watch option automatically looks for any saved changes to db.json

```javascript
json-server --port 3001 --watch db.json
```

> However, a global installation is not necessary. From the root directory of your app, we can run the json-server using the command npx:

```javascript
npx json-server --port 3001 --watch db.json
```

> Let's navigate to the address http://localhost:3001/notes in the browser. We can see that json-server serves the notes we previously wrote to the file in JSON format:

!["fullstack content"](./images/part2c_image1.png?raw=true)

> If your browser doesn't have a way to format the display of JSON-data, then install an appropriate plugin, e.g. JSONVue to make your life easier.

> Going forward, the idea will be to save the notes to the server, which in this case means saving them to the json-server. The React code fetches the notes from the server and renders them to the screen. Whenever a new note is added to the application, the React code also sends it to the server to make the new note persist in "memory".

> json-server stores all the data in the db.json file, which resides on the server. In the real world, data would be stored in some kind of database. However, json-server is a handy tool that enables the use of server-side functionality in the development phase without the need to program any of it.

> We will get familiar with the principles of implementing server-side functionality in more detail in part 3 of this course.

### The browser as a runtime environment

> Our first task is fetching the already existing notes to our React application from the address http://localhost:3001/notes.

> In the part0 example project we already learned a way to fetch data from a server using JavaScript. The code in the example was fetching the data using XMLHttpRequest, otherwise known as an HTTP request made using an XHR object. This is a technique introduced in 1999, which every browser has supported for a good while now.

> The use of XHR is no longer recommended, and browsers already widely support the fetch method, which is based on so-called promises, instead of the event-driven model used by XHR.

> As a reminder from part0 (which one should in fact remember to not use without a pressing reason), data was fetched using XHR in the following way: 

```javascript
const xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    // handle the response that is saved in variable data
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

> Right at the beginning we register an event handler to the xhttp object representing the HTTP request, which will be called by the JavaScript runtime whenever the state of the xhttp object changes. If the change in state means that the response to the request has arrived, then the data is handled accordingly.

> It is worth noting that the code in the event handler is defined before the request is sent to the server. Despite this, the code within the event handler will be executed at a later point in time. Therefore, the code does not execute synchronously "from top to bottom", but does so asynchronously. JavaScript calls the event handler that was registered for the request at some point.

> A synchronous way of making requests that's common in Java programming, for instance, would play out as follows (NB, this is not actually working Java code):

```javascript
HTTPRequest request = new HTTPRequest();

String url = "https://fullstack-exampleapp.herokuapp.com/data.json";
List<Note> notes = request.get(url);

notes.forEach(m => {
  System.out.println(m.content);
});
```

> In Java the code executes line by line and stops to wait for the HTTP request, which means waiting for the command request.get(...) to finish. The data returned by the command, in this case the notes, are then stored in a variable, and we begin manipulating the data in the desired manner.

> On the other hand, JavaScript engines, or runtime environments, follow the asynchronous model. In principle, this requires all IO-operations (with some exceptions) to be executed as non-blocking. This means that code execution continues immediately after calling an IO function, without waiting for it to return.

> When an asynchronous operation is completed, or, more specifically, at some point after its completion, the JavaScript engine calls the event handlers registered to the operation.

> Currently, JavaScript engines are single-threaded, which means that they cannot execute code in parallel. As a result, it is a requirement in practice to use a non-blocking model for executing IO operations. Otherwise, the browser would "freeze" during, for instance, the fetching of data from a server.

> Another consequence of this single-threaded nature of JavaScript engines is that if some code execution takes up a lot of time, the browser will get stuck for the duration of the execution. If we added the following code at the top of our application:

```javascript
setTimeout(() => {
  console.log('loop..')
  let i = 0
  while (i < 50000000000) {
    i++
  }
  console.log('end')
}, 5000)
```

> everything would work normally for 5 seconds. However, when the function defined as the parameter for setTimeout is run, the browser will be stuck for the duration of the execution of the long loop. Even the browser tab cannot be closed during the execution of the loop, at least not in Chrome.

> For the browser to remain responsive, i.e., to be able to continuously react to user operations with sufficient speed, the code logic needs to be such that no single computation can take too long.

> There is a host of additional material on the subject to be found on the internet. One particularly clear presentation of the topic is the keynote by Philip Roberts called What the heck is the event loop anyway?

> In today's browsers, it is possible to run parallelized code with the help of so-called web workers. The event loop of an individual browser window is, however, still only handled by a single thread.

### npm

> Let's get back to the topic of fetching data from the server.

> We could use the previously mentioned promise based function fetch to pull the data from the server. Fetch is a great tool. It is standardized and supported by all modern browsers (excluding IE).

> That being said, we will be using the axios library instead for communication between the browser and server. It functions like fetch, but is somewhat more pleasant to use. Another good reason to use axios is our getting familiar with adding external libraries, so-called npm packages, to React projects.

> Nowadays, practically all JavaScript projects are defined using the node package manager, aka npm. The projects created using create-react-app also follow the npm format. A clear indicator that a project uses npm is the package.json file located at the root of the project:

```javascript
{
  "name": "notes",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.1",
    "@testing-library/react": "^12.1.2",
    "@testing-library/user-event": "^13.5.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "5.0.0",
    "web-vitals": "^2.1.3"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

> At this point the dependencies part is of most interest to us as it defines what dependencies, or external libraries, the project has.

> We now want to use axios. Theoretically, we could define the library directly in the package.json file, but it is better to install it from the command line.

```javascript
npm install axios
```

> NB npm-commands should always be run in the project root directory, which is where the package.json file can be found.

> Axios is now included among the other dependencies:

```javascript
{
  "name": "notes",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.1",
    "@testing-library/react": "^12.1.2",
    "@testing-library/user-event": "^13.5.0",
    "axios": "^0.24.0",    
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "5.0.0",
    "web-vitals": "^2.1.3"
  },
  // ...
}
```

> In addition to adding axios to the dependencies, the npm install command also downloaded the library code. As with other dependencies, the code can be found in the node_modules directory located in the root. As one might have noticed, node_modules contains a fair amount of interesting stuff.

> Let's make another addition. Install json-server as a development dependency (only used during development) by executing the command:

```javascript
npm install json-server --save-dev
```

> and making a small addition to the scripts part of the package.json file:

```javascript
{
  // ... 
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "server": "json-server -p3001 --watch db.json"  
  },
}
```

> We can now conveniently, without parameter definitions, start the json-server from the project root directory with the command:

```javascript
npm run server
```

> We will get more familiar with the npm tool in the third part of the course.

> NB The previously started json-server must be terminated before starting a new one; otherwise there will be trouble:

!["fullstack content"](./images/part2c_image2.png?raw=true)

> The red print in the error message informs us about the issue:

> Cannot bind to port 3001. Please specify another port number either through --port argument or through the json-server.json configuration file

> As we can see, the application is not able to bind itself to the port. The reason being that port 3001 is already occupied by the previously started json-server.

> We used the command npm install twice, but with slight differences:

```
npm install axios
npm install json-server --save-dev
```

> There is a fine difference in the parameters. axios is installed as a runtime dependency of the application, because the execution of the program requires the existence of the library. On the other hand, json-server was installed as a development dependency (--save-dev), since the program itself doesn't require it. It is used for assistance during software development. There will be more on different dependencies in the next part of the course.